# Real-Time Settlement - Implementation

## Core Components

### Settlement Batch Collector

```java
@Service
public class SettlementBatchCollector {
  
  private final KafkaTemplate<String, SettlementInstruction> kafkaTemplate;
  private final SettlementBatchRepository batchRepository;
  private final int BATCH_SIZE = 10000;
  private final long BATCH_TIMEOUT_MS = 100;

  @Async
  public void collectAndBatch() {
    while (true) {
      List<SettlementQueueItem> items = new ArrayList<>();
      long startTime = System.currentTimeMillis();
      
      // Collect until batch size or timeout
      while (items.size() < BATCH_SIZE && 
             (System.currentTimeMillis() - startTime) < BATCH_TIMEOUT_MS) {
        SettlementQueueItem item = settlementQueue.poll(10, TimeUnit.MILLISECONDS);
        if (item != null) {
          items.add(item);
        }
      }
      
      if (!items.isEmpty()) {
        processBatch(items);
      }
    }
  }

  private void processBatch(List<SettlementQueueItem> items) {
    SettlementBatch batch = createBatch(items);
    calculateNetPositions(batch);
    generateInstructions(batch);
    submitToRTGS(batch);
  }
}
```

### Net Position Calculator

```java
public class NetPositionCalculator {
  
  public Map<String, BigDecimal> calculateNetPositions(
      List<SettlementQueueItem> items) {
    
    Map<String, BigDecimal> netPositions = new HashMap<>();
    
    for (SettlementQueueItem item : items) {
      String key = item.getMerchantId() + "-" + item.getDestinationBank();
      
      netPositions.put(key, 
          netPositions.getOrDefault(key, BigDecimal.ZERO)
              .add(item.getAmount()));
    }
    
    return netPositions;
  }
}
```

### RTGS Connector

```java
@Service
public class RTGSConnector {
  
  private final RestTemplate restTemplate;
  private final String RTGS_ENDPOINT = "https://rtgs.federalreserve.gov/api";

  public SettlementConfirmation submitInstruction(
      SettlementInstruction instruction) throws IOException {
    
    // Create RTGS message (ISO 20022 format)
    String rtgsMessage = formatISO20022(instruction);
    
    // Send to RTGS
    HttpHeaders headers = new HttpHeaders();
    headers.setContentType(MediaType.APPLICATION_JSON);
    headers.set("Authorization", "Bearer " + getRTGSToken());
    
    HttpEntity<String> request = new HttpEntity<>(rtgsMessage, headers);
    
    ResponseEntity<SettlementConfirmation> response = 
        restTemplate.exchange(
            RTGS_ENDPOINT + "/submit",
            HttpMethod.POST,
            request,
            SettlementConfirmation.class
        );
    
    return response.getBody();
  }

  private String formatISO20022(SettlementInstruction instruction) {
    // Build ISO 20022 XML message
    return String.format(
        "<?xml version=\"1.0\"?>" +
        "<Document xmlns=\"urn:iso:std:iso:20022:tech:xsd:pain.001.003.02\">" +
        "<CstmrCdtTrfInitn>" +
        "  <GrpHdr>" +
        "    <MsgId>%s</MsgId>" +
        "    <CreDtTm>%s</CreDtTm>" +
        "  </GrpHdr>" +
        "  <PmtInf>" +
        "    <PmtInfId>%s</PmtInfId>" +
        "    <PmtMtd>TRF</PmtMtd>" +
        "    <DbtrAcct>" +
        "      <Id>" +
        "        <IBAN>%s</IBAN>" +
        "      </Id>" +
        "    </DbtrAcct>" +
        "  </PmtInf>" +
        "</CstmrCdtTrfInitn>" +
        "</Document>",
        instruction.getInstructionId(),
        LocalDateTime.now(),
        instruction.getBatchId(),
        instruction.getFromAccount()
    );
  }
}
```

### Webhook Confirmation Service

```java
@Service
public class ConfirmationWebhookService {
  
  @Autowired
  private WebClient webClient;
  
  @Autowired
  private ConfirmationRepository confirmationRepository;

  @Async
  @Retryable(maxAttempts = 4, delay = @Delay(100), 
             multiplier = 5)
  public void sendConfirmation(SettlementConfirmation confirmation) {
    Confirmation record = new Confirmation();
    record.setConfirmationId(UUID.randomUUID());
    record.setInstructionId(confirmation.getInstructionId());
    record.setStatus(ConfirmationStatus.PENDING);
    record.setWebhookUrl(getWebhookUrl(confirmation.getMerchantId()));
    confirmationRepository.save(record);
    
    // Send webhook
    String payload = objectMapper.writeValueAsString(confirmation);
    
    webClient.post()
        .uri(record.getWebhookUrl())
        .contentType(MediaType.APPLICATION_JSON)
        .bodyValue(payload)
        .retrieve()
        .toBodilessEntity()
        .subscribe(
            response -> {
              record.setStatus(ConfirmationStatus.ACKNOWLEDGED);
              confirmationRepository.save(record);
            },
            error -> {
              record.setRetryCount(record.getRetryCount() + 1);
              record.setLastRetryAt(LocalDateTime.now());
              confirmationRepository.save(record);
              throw new WebhookDeliveryException("Failed to deliver webhook", error);
            }
        );
  }
}
```

## Reconciliation Job

```java
@Component
public class SettlementReconciliation {
  
  @Scheduled(cron = "0 2 * * *") // 2 AM daily
  public void reconcileDaily() {
    // Get yesterday's date
    LocalDate yesterday = LocalDate.now().minusDays(1);
    
    // 1. Get confirmed settlements from DB
    List<SettlementInstruction> dbInstructions = 
        instructionRepository.findByDateAndStatus(
            yesterday, SettlementStatus.CONFIRMED);
    BigDecimal dbTotal = dbInstructions.stream()
        .map(SettlementInstruction::getAmount)
        .reduce(BigDecimal.ZERO, BigDecimal::add);
    
    // 2. Get balance from Federal Reserve
    BigDecimal rtgsBalance = federalReserveClient.getBalance(yesterday);
    
    // 3. Compare
    BigDecimal variance = dbTotal.subtract(rtgsBalance).abs();
    if (variance.compareTo(BigDecimal.valueOf(100)) > 0) {
      // Investigate
      investigateVariance(dbInstructions, variance);
      alertOpsTeam("Settlement variance detected: $" + variance);
    } else {
      log.info("Settlement reconciliation successful. Variance: $" + variance);
    }
  }
}
```

## Key SLAs

- Queue to batch: <100ms
- Batch processing: <50ms
- RTGS transmission: <200ms
- Total latency (queue to confirmation): <1 second
- Settlement success rate: 99.99%
- Reconciliation variance: <$1 per 1M transactions

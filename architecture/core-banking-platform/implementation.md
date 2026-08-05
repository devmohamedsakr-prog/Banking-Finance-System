# Core Banking Platform - Implementation

## System Design

### Transaction Flow

```
Customer Initiates Transaction
         ↓
API Gateway (auth + rate limit)
         ↓
Account Validation Service
  ├─ Check account exists
  ├─ Verify customer KYC status
  ├─ Check account status (active)
         ↓
Fraud Detection Service
  ├─ Velocity checks
  ├─ Amount anomaly detection
  ├─ Geographic checks
         ↓
Balance Check
  ├─ Verify available balance
  ├─ Reserve amount (hold)
         ↓
Transaction Queue (Kafka)
         ↓
Settlement Service (batch processing)
  ├─ Calculate net positions
  ├─ Generate ledger entries
  ├─ Post to general ledger
         ↓
Confirmation Response
         ↓
Event Stream (for reporting)
```

## Core Implementation

### Account Service

```java
@RestController
@RequestMapping("/api/v1/accounts")
public class AccountController {

  @Autowired
  private AccountService accountService;

  @GetMapping("/{accountId}")
  public ResponseEntity<AccountDTO> getAccount(@PathVariable UUID accountId) {
    Account account = accountService.getAccount(accountId);
    return ResponseEntity.ok(new AccountDTO(account));
  }

  @PostMapping
  public ResponseEntity<AccountDTO> createAccount(@RequestBody CreateAccountRequest request) {
    Account account = accountService.createAccount(request);
    return ResponseEntity.status(201).body(new AccountDTO(account));
  }

  @GetMapping("/{accountId}/balance")
  public ResponseEntity<BalanceDTO> getBalance(@PathVariable UUID accountId) {
    BalanceDTO balance = accountService.getBalance(accountId);
    return ResponseEntity.ok(balance);
  }
}

@Service
public class AccountService {
  
  @Autowired
  private AccountRepository accountRepository;
  
  @Autowired
  private BalanceRepository balanceRepository;
  
  @Autowired
  private RedisTemplate<String, String> redisTemplate;

  public Account getAccount(UUID accountId) {
    // Try cache first
    String cacheKey = "account:" + accountId;
    String cached = redisTemplate.opsForValue().get(cacheKey);
    if (cached != null) {
      return objectMapper.readValue(cached, Account.class);
    }
    
    // Query database
    Account account = accountRepository.findById(accountId)
        .orElseThrow(() -> new AccountNotFoundException("Account not found"));
    
    // Cache for 5 minutes
    redisTemplate.opsForValue().set(cacheKey, objectMapper.writeValueAsString(account), 
        Duration.ofMinutes(5));
    
    return account;
  }

  public Account createAccount(CreateAccountRequest request) {
    // Validate customer KYC
    Customer customer = customerService.getCustomer(request.getCustomerId());
    if (!customer.isKycVerified()) {
      throw new ValidationException("Customer KYC not verified");
    }
    
    // Create account
    Account account = new Account();
    account.setAccountId(UUID.randomUUID());
    account.setCustomerId(request.getCustomerId());
    account.setAccountType(request.getAccountType());
    account.setCurrency(request.getCurrency());
    account.setStatus(AccountStatus.ACTIVE);
    account.setBalance(BigDecimal.ZERO);
    account.setAvailableBalance(BigDecimal.ZERO);
    account.setCreatedAt(LocalDateTime.now());
    
    return accountRepository.save(account);
  }

  public BalanceDTO getBalance(UUID accountId) {
    Account account = getAccount(accountId);
    
    return BalanceDTO.builder()
        .accountId(accountId)
        .ledgerBalance(account.getBalance())
        .availableBalance(account.getAvailableBalance())
        .holds(calculateHolds(accountId))
        .timestamp(LocalDateTime.now())
        .build();
  }

  private BigDecimal calculateHolds(UUID accountId) {
    // Query pending transactions
    List<Hold> holds = holdRepository.findByAccountIdAndStatus(
        accountId, HoldStatus.PENDING);
    return holds.stream()
        .map(Hold::getAmount)
        .reduce(BigDecimal.ZERO, BigDecimal::add);
  }
}
```

### Transaction Service

```java
@Service
@Transactional
public class TransactionService {
  
  @Autowired
  private TransactionRepository transactionRepository;
  
  @Autowired
  private AccountService accountService;
  
  @Autowired
  private KafkaTemplate<String, TransactionEvent> kafkaTemplate;
  
  @Autowired
  private FraudService fraudService;

  public Transaction processTransaction(TransactionRequest request) {
    // 1. Validate accounts
    Account fromAccount = accountService.getAccount(request.getFromAccountId());
    Account toAccount = accountService.getAccount(request.getToAccountId());
    
    // 2. Check balance
    if (fromAccount.getAvailableBalance().compareTo(request.getAmount()) < 0) {
      throw new InsufficientFundsException("Insufficient available balance");
    }
    
    // 3. Fraud check
    FraudScore fraudScore = fraudService.score(request);
    if (fraudScore.getScore() > 70) {
      // Require challenge or decline
      throw new TransactionDeclinedException("Transaction declined by fraud system");
    }
    
    // 4. Create transaction record (PENDING)
    Transaction transaction = new Transaction();
    transaction.setTransactionId(UUID.randomUUID());
    transaction.setFromAccountId(request.getFromAccountId());
    transaction.setToAccountId(request.getToAccountId());
    transaction.setAmount(request.getAmount());
    transaction.setType(request.getType());
    transaction.setStatus(TransactionStatus.PENDING);
    transaction.setTimestamp(LocalDateTime.now());
    
    Transaction saved = transactionRepository.save(transaction);
    
    // 5. Place hold on from_account
    Hold hold = new Hold();
    hold.setHoldId(UUID.randomUUID());
    hold.setAccountId(request.getFromAccountId());
    hold.setAmount(request.getAmount());
    hold.setTransactionId(saved.getTransactionId());
    hold.setStatus(HoldStatus.PENDING);
    holdRepository.save(hold);
    
    // Update available balance
    fromAccount.setAvailableBalance(
        fromAccount.getAvailableBalance().subtract(request.getAmount()));
    accountService.saveAccount(fromAccount);
    
    // 6. Publish to Kafka for settlement
    TransactionEvent event = new TransactionEvent(saved);
    kafkaTemplate.send("transactions-settlement", event);
    
    return saved;
  }
}
```

### General Ledger Service

```java
@Service
public class GeneralLedgerService {
  
  @Autowired
  private GeneralLedgerRepository glRepository;
  
  @Autowired
  private AccountService accountService;

  @Transactional
  public void postTransaction(Transaction transaction) {
    // Double-entry accounting:
    // Debit from_account, credit to_account
    
    LocalDate postingDate = LocalDate.now();
    
    // Debit entry (from account)
    GeneralLedgerEntry debitEntry = new GeneralLedgerEntry();
    debitEntry.setEntryId(UUID.randomUUID());
    debitEntry.setAccountNumber(getAccountNumber(transaction.getFromAccountId()));
    debitEntry.setDebitAmount(transaction.getAmount());
    debitEntry.setCreditAmount(BigDecimal.ZERO);
    debitEntry.setTransactionId(transaction.getTransactionId());
    debitEntry.setPostingDate(postingDate);
    glRepository.save(debitEntry);
    
    // Credit entry (to account)
    GeneralLedgerEntry creditEntry = new GeneralLedgerEntry();
    creditEntry.setEntryId(UUID.randomUUID());
    creditEntry.setAccountNumber(getAccountNumber(transaction.getToAccountId()));
    creditEntry.setDebitAmount(BigDecimal.ZERO);
    creditEntry.setCreditAmount(transaction.getAmount());
    creditEntry.setTransactionId(transaction.getTransactionId());
    creditEntry.setPostingDate(postingDate);
    glRepository.save(creditEntry);
    
    // Update account balances
    Account fromAccount = accountService.getAccount(transaction.getFromAccountId());
    Account toAccount = accountService.getAccount(transaction.getToAccountId());
    
    fromAccount.setBalance(fromAccount.getBalance().subtract(transaction.getAmount()));
    toAccount.setBalance(toAccount.getBalance().add(transaction.getAmount()));
    
    accountService.saveAccount(fromAccount);
    accountService.saveAccount(toAccount);
  }

  public GeneralLedgerReport generateTrialBalance(LocalDate date) {
    List<GeneralLedgerEntry> entries = glRepository.findByPostingDate(date);
    
    // Group by account and sum debits/credits
    Map<String, BigDecimal[]> balances = new HashMap<>();
    for (GeneralLedgerEntry entry : entries) {
      String account = entry.getAccountNumber();
      balances.putIfAbsent(account, new BigDecimal[]{BigDecimal.ZERO, BigDecimal.ZERO});
      balances.get(account)[0] = balances.get(account)[0].add(entry.getDebitAmount());
      balances.get(account)[1] = balances.get(account)[1].add(entry.getCreditAmount());
    }
    
    // Total debits and credits must be equal
    BigDecimal totalDebits = balances.values().stream()
        .map(b -> b[0])
        .reduce(BigDecimal.ZERO, BigDecimal::add);
    BigDecimal totalCredits = balances.values().stream()
        .map(b -> b[1])
        .reduce(BigDecimal.ZERO, BigDecimal::add);
    
    if (totalDebits.compareTo(totalCredits) != 0) {
      throw new ReconciliationException("Trial balance does not balance");
    }
    
    return new GeneralLedgerReport(balances, totalDebits, totalCredits);
  }
}
```

### Settlement Batch Job

```java
@Component
public class SettlementBatchJob {
  
  @Autowired
  private TransactionRepository transactionRepository;
  
  @Autowired
  private GeneralLedgerService glService;
  
  @Autowired
  private SettlementRepository settlementRepository;

  @Scheduled(fixedDelay = 60000) // Run every 60 seconds
  public void settlePendingTransactions() {
    // Get all pending transactions from last 24 hours
    LocalDateTime cutoff = LocalDateTime.now().minusDays(1);
    List<Transaction> pending = transactionRepository
        .findByStatusAndTimestampAfter(TransactionStatus.PENDING, cutoff);
    
    for (Transaction transaction : pending) {
      try {
        // Post to general ledger
        glService.postTransaction(transaction);
        
        // Update transaction status
        transaction.setStatus(TransactionStatus.POSTED);
        transactionRepository.save(transaction);
        
        // Record settlement
        Settlement settlement = new Settlement();
        settlement.setSettlementId(UUID.randomUUID());
        settlement.setTransactionId(transaction.getTransactionId());
        settlement.setSettlementDate(LocalDate.now());
        settlement.setAmount(transaction.getAmount());
        settlement.setStatus(SettlementStatus.COMPLETED);
        settlementRepository.save(settlement);
        
      } catch (Exception e) {
        // Log and continue with next transaction
        log.error("Settlement failed for transaction: " + transaction.getTransactionId(), e);
        transaction.setStatus(TransactionStatus.FAILED);
        transactionRepository.save(transaction);
      }
    }
  }
}
```

## Monitoring & Observability

```java
@Component
public class TransactionMetrics {
  
  private final MeterRegistry meterRegistry;

  public TransactionMetrics(MeterRegistry meterRegistry) {
    this.meterRegistry = meterRegistry;
  }

  public void recordTransaction(Transaction transaction) {
    meterRegistry.counter("transactions.total", 
        "type", transaction.getType().toString(),
        "status", transaction.getStatus().toString()).increment();
    
    meterRegistry.gauge("accounts.balance",
        transaction.getFromAccountId().toString(),
        accountService.getAccount(transaction.getFromAccountId()).getBalance().doubleValue());
  }

  public void recordLatency(long durationMs) {
    meterRegistry.timer("transaction.latency.ms").record(durationMs, TimeUnit.MILLISECONDS);
  }
}
```

## Key SLAs

- Account creation: <100ms
- Balance inquiry: <50ms (p99)
- Transaction processing: <500ms (p99)
- Settlement confirmation: <1s
- System availability: 99.99%

## Deployment Considerations

- Multi-region replication
- Read replicas for reporting
- Connection pooling (50-100 connections per service)
- Prepared statement caching
- Database partition by customer_id for horizontal scaling

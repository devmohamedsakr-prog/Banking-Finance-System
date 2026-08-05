# Banking-System: Best Practices & Standards

**Version:** 2.0 | **Status:** Production | **Updated:** 2026-08-05

---

## Development Best Practices

### Code Standards

**Go:**
- Use `gofmt` for formatting
- Coverage target: >80%
- Concurrent safe: Always consider goroutines
- Error handling: Explicit nil checks

**Python:**
- Use `black` + `isort` for formatting
- Coverage target: >80%
- Type hints: 100% coverage
- Async: Use `asyncio` for I/O

**Java:**
- Use Spring Boot best practices
- Coverage target: >80%
- Immutability: Prefer final classes
- Dependency injection: All autowired

### API Design

**REST Principles:**
- Resource-oriented (nouns, not verbs)
- Proper HTTP methods (GET/POST/PUT/DELETE)
- Idempotent operations (retry-safe)
- Versioning: URL path (/v1/, /v2/)

**Response Format:**
```json
{
  "success": true,
  "data": { ... },
  "timestamp": "2024-01-15T10:30:00Z",
  "request_id": "req_123"
}

Error Response:
{
  "success": false,
  "error": {
    "code": "INSUFFICIENT_FUNDS",
    "message": "Account balance too low",
    "details": "Required: $1000, Available: $500"
  },
  "timestamp": "2024-01-15T10:30:00Z",
  "request_id": "req_123"
}
```

---

## Security Best Practices

### Input Validation

```
Rule 1: Never trust user input
Rule 2: Validate at API boundary
Rule 3: Type check + range check
Rule 4: Sanitize before storage

Example:
amount = int(request.amount)
if amount <= 0 or amount > 1000000:
    return {"error": "Invalid amount"}
```

### Password Security

- Minimum: 12 characters
- Complexity: Mixed case + numbers + symbols
- Hashing: bcrypt (cost factor 12+)
- No password reuse: Last 5 passwords

### API Key Security

- Rotate: Every 90 days
- Storage: Environment variables only
- Transmission: TLS 1.3+ only
- Logging: Never log full keys

### Data Privacy

- PII: Encrypted at rest
- Card numbers: Never store (tokenize)
- Audit logging: All access
- Retention: Delete after 7 years

---

## Performance Best Practices

### Caching Strategy

**L1 Cache (In-Memory):**
- TTL: 5-60 seconds
- Use: Hot data (balance, rates)
- Size: Limited by RAM

**L2 Cache (Redis):**
- TTL: 1-5 minutes
- Use: User profiles, merchants
- Size: 100GB+

**L3 Cache (CDN):**
- TTL: 1-24 hours
- Use: Static assets, public data
- Size: Unlimited

### Query Optimization

- Use indexes (strategic)
- EXPLAIN ANALYZE every query
- Avoid N+1 queries
- Batch operations when possible

### Connection Pooling

```
Database connections: 500 pool size
- Min connections: 10
- Max connections: 500
- Idle timeout: 300 seconds
- Queue timeout: 30 seconds
```

---

## Reliability Best Practices

### Circuit Breaker Pattern

```
if external_service_failures > threshold:
    open_circuit()  # Fast fail
    return cached_response()

After 30 seconds:
    half_open_circuit()  # Try again
    if success:
        close_circuit()
    else:
        open_circuit()  # Fail again
```

### Retry Logic

```
Retry Strategy:
- Retriable: Timeouts, 5xx errors
- Not retriable: 4xx errors, validation
- Exponential backoff: 100ms → 1s → 10s
- Max retries: 3
- Max wait: 30 seconds
```

### Rate Limiting (Self)

```
Per-service rate limit:
- Payment: 1M TPS max
- Wallet: 10M TPS max
- Settlement: 100K TPS max

If exceeding:
1. Alert ops
2. Start rejecting requests
3. Auto-scale if possible
4. Activate fallback (cache)
```

---

## Testing Best Practices

### Unit Tests

```python
def test_payment_success():
    # Arrange
    payment = Payment(amount=100, currency="USD")
    
    # Act
    result = process_payment(payment)
    
    # Assert
    assert result.status == "completed"
    assert result.amount == 100
```

### Integration Tests

- Use Docker containers
- Real databases (test instances)
- End-to-end flows
- 1000+ test cases

### Load Testing

- k6 or JMeter
- Ramp: 0 → 100K → 1M users
- Duration: 30 minutes
- Assert: Latency <100ms p99

---

## Operational Best Practices

### Monitoring

**Golden Signals (4):**
1. Latency (p50, p95, p99)
2. Traffic (requests/sec)
3. Errors (rate, types)
4. Saturation (CPU, memory)

### On-Call

- Rotation: Weekly
- Alerting: SMS + Slack + email
- Escalation: Manager after 30 min
- Runbook: Documented for each alert

### Incident Response

```
Severity P1 (Critical):
- Response time: <5 min
- Resolution target: <1 hour
- Post-mortem: Within 24 hours

Severity P2 (Major):
- Response time: <30 min
- Resolution target: <4 hours
- Post-mortem: Within 5 days
```

### Change Management

- Code review: 2 approvals
- Deployment window: Business hours
- Rollback plan: Always documented
- Communication: Slack notification

---

## Documentation Best Practices

### API Documentation

- OpenAPI/Swagger spec
- Examples for each endpoint
- Error codes documented
- Rate limits documented

### Runbooks

- Troubleshooting guide
- Step-by-step procedures
- Common issues + solutions
- Escalation path

### Architecture Documentation

- Diagrams (C4 model)
- Technology choices + rationale
- Trade-offs documented
- Update: Quarterly

---

## Code Review Checklist

- [ ] Code style follows standards
- [ ] Tests written (>80% coverage)
- [ ] No hardcoded values
- [ ] No secrets in code
- [ ] Error handling complete
- [ ] Logging appropriate
- [ ] Performance considered
- [ ] Security reviewed
- [ ] Documentation updated
- [ ] JIRA ticket linked

---

## Release Checklist

- [ ] All tests passing
- [ ] Code review approved
- [ ] Performance tested
- [ ] Security scanned
- [ ] Documentation updated
- [ ] Changelog updated
- [ ] Version bumped
- [ ] Release notes drafted
- [ ] Runbook prepared
- [ ] On-call briefed

---

## Status

✅ Production-ready standards | Enterprise-grade | Continuously improved

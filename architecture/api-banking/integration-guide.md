# API Banking - Integration Guide

## API Authentication

```
OAuth 2.0 Flow:

1. Developer Registration
   - Create app on developer portal
   - Get Client ID and Client Secret
   - Configure redirect URIs

2. Client Credentials Grant (Server-to-server)
   POST /oauth/token
   {
     "client_id": "...",
     "client_secret": "...",
     "grant_type": "client_credentials",
     "scope": "payments:write accounts:read"
   }
   
   Response:
   {
     "access_token": "eyJhbGc...",
     "token_type": "Bearer",
     "expires_in": 3600
   }

3. Authorization Code Grant (User-initiated)
   - Redirect to /oauth/authorize
   - User grants permission
   - Redirect back with code
   - Exchange code for token
```

## Rate Limiting

```
Token Bucket Algorithm:

Tier 1 (Free):
- 100 requests/minute
- Burst: 200
- Quota: 10K/month

Tier 2 (Pro):
- 1000 requests/minute
- Burst: 2000
- Quota: 1M/month

Tier 3 (Enterprise):
- Unlimited requests/minute
- Custom burst limits
- Dedicated support

Response Headers:
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 847
X-RateLimit-Reset: 1693598400
```

## Webhook Integration

```
Event-Driven Architecture:

Supported Events:
- payment.created
- payment.completed
- payment.failed
- account.created
- transfer.settled

Webhook Delivery:

POST https://customer.example.com/webhook

Payload:
{
  "event_id": "evt_123...",
  "event_type": "payment.completed",
  "timestamp": "2024-01-15T10:30:00Z",
  "data": {
    "payment_id": "pay_456...",
    "amount": 1000,
    "currency": "USD",
    "status": "completed"
  }
}

Retry Logic:
- Immediate (0s)
- After 5 minutes
- After 30 minutes
- After 2 hours
- After 24 hours
- Then give up

Signature Verification:
X-Signature-256: sha256=...

verify_webhook(body, secret, signature)
```

## Error Handling

```
HTTP Status Codes:

200 OK: Success
201 Created: Resource created
400 Bad Request: Invalid input
401 Unauthorized: Missing/invalid token
403 Forbidden: Insufficient permissions
404 Not Found: Resource doesn't exist
429 Too Many Requests: Rate limit exceeded
500 Internal Server Error: Server error
503 Service Unavailable: Maintenance

Error Response:
{
  "error": {
    "code": "INSUFFICIENT_FUNDS",
    "message": "Account balance too low",
    "details": "Required: $1000, Available: $500"
  }
}
```

## Sandbox Environment

```
Testing:

Sandbox URL: https://sandbox.bankapi.io
Production URL: https://api.bankapi.io

Test Cards:
- 4242424242424242: Success
- 4000000000000002: Decline
- 4111111111111111: 3D Secure challenge

Test Scenarios:
- Create test account
- Make test payment
- Simulate webhook
- Test error cases
```

## Status
✅ Production-ready | 1000+ API partners

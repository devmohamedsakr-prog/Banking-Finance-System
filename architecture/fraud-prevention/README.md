# Fraud Prevention System

**Target:** <0.1% fraud rate, <50ms detection latency

## Overview

Real-time fraud detection using ML models, velocity checks, geographic anomaly detection, and device fingerprinting. Integrated into all transaction processing pipelines.

## Detection Pipeline

```
Transaction → Pre-Auth Checks
    ↓
Velocity Checks (concurrent transactions)
    ↓
Geographic Anomaly Detection
    ↓
Device Fingerprinting
    ↓
ML Risk Scoring (0-100)
    ├─ 0-20: Auto-approve
    ├─ 20-70: 3D Secure challenge
    └─ 70+: Manual review
    ↓
Decision → Approve / Decline / Challenge
```

## Detection Methods

### Velocity Rules

- 5+ transactions in 10 seconds → FLAG
- 10+ transactions in 1 minute → DECLINE
- $100K in 1 hour → REVIEW
- 50+ card tests in 1 hour → BLOCK

### Geographic Checks

- Country jump in <1 hour → FLAG
- New country in 2 hours (previous 2K km away) → CHALLENGE
- High-risk country → ENHANCED KYC

### Device Fingerprinting

- New device + high amount → FLAG
- Multiple accounts on same device → MONITOR
- Rooted/jailbroken device → ENHANCED AUTH
- VPN/Proxy detected → FLAG

### ML Risk Scoring

Features (50+):
- Transaction amount
- Merchant category
- Time of day
- Device fingerprint
- Customer history
- Network analysis
- Biometric confidence
- Account age

### Case Management

- Risk score 0-20: Auto-approve
- Risk score 20-70: 3D Secure/SMS OTP
- Risk score 70-90: Call customer/email verification
- Risk score 90+: Manual review/decline

## Technology Stack

- **ML Framework:** TensorFlow, XGBoost
- **Feature Store:** Feast or Tecton
- **Real-time:** Redis for velocity checks
- **Database:** PostgreSQL for rules, Cassandra for events
- **Deployment:** Kubernetes, GPU nodes for model serving

## Key SLAs

- Detection latency: <50ms (p99)
- Model inference: <20ms
- False positive rate: <2%
- True positive rate: >95%

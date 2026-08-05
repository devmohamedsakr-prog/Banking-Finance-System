# PCI DSS 4.0 - Compliance Checklist

## 12 Core Requirements - Implementation Status

### Requirement 1: Firewall Configuration
- [ ] Document firewall architecture
- [ ] Define firewall rules (explicit allow/deny)
- [ ] Test firewall rules quarterly
- [ ] Network segmentation documented
- [ ] Cardholder Data Environment (CDE) isolated

### Requirement 2: Remove Default Passwords  
- [ ] Change all vendor defaults (devices, software)
- [ ] Document all changes
- [ ] Remove unnecessary services
- [ ] Disable unused protocols
- [ ] Configuration review process

### Requirement 3: Protect Stored Data
- [ ] No full card numbers stored
- [ ] Tokenization implemented (80% cards)
- [ ] Encryption (AES-256) for remaining
- [ ] Key rotation every 90 days
- [ ] Key access restricted

### Requirement 4: Protect Data in Transit
- [ ] TLS 1.2+ for all communications
- [ ] Certificate validation enabled
- [ ] End-to-end encryption
- [ ] Weak ciphers disabled
- [ ] Quarterly cipher testing

### Requirement 5: Protect Against Malware
- [ ] Antivirus on all systems (100%)
- [ ] Automatic updates enabled
- [ ] Malware scan weekly
- [ ] Quarantine procedures
- [ ] Incident log maintained

### Requirement 6: Secure Development
- [ ] Security code review process
- [ ] Secure SDLC procedures
- [ ] Testing in dev/test (not prod)
- [ ] Change management documented
- [ ] Separation of environments

### Requirement 7: Restrict Access by Business Need
- [ ] Role-based access control (RBAC)
- [ ] Principle of least privilege
- [ ] Default deny policy
- [ ] Access matrix documented
- [ ] Access review quarterly

### Requirement 8: Assign Unique User IDs
- [ ] All system access tracked
- [ ] Unique IDs for humans and systems
- [ ] Inactive IDs disabled (>90 days)
- [ ] Shared accounts prohibited
- [ ] User ID audit trail

### Requirement 9: Restrict Physical Access
- [ ] Badge access system
- [ ] Surveillance cameras
- [ ] Visitor logs maintained
- [ ] Server room locks
- [ ] Badge reader audit trail

### Requirement 10: Track and Monitor Access
- [ ] Centralized logging
- [ ] All access to cardholder data logged
- [ ] User ID + timestamp logged
- [ ] Successful and failed access
- [ ] Log retention (at least 1 year)

### Requirement 11: Test Security Systems
- [ ] Annual penetration test
- [ ] Quarterly vulnerability scan
- [ ] Remediation of findings
- [ ] Re-test after fixes
- [ ] Written test procedures

### Requirement 12: Maintain Security Policy
- [ ] Written information security policy
- [ ] Policy covers all 12 requirements
- [ ] Annual review and updates
- [ ] Employee security training
- [ ] Incident response plan

## Compliance Audit Process

```
Annual Audit Cycle:

Q1: Planning
- Define audit scope
- Assign QSA (Qualified Security Assessor)
- Resource allocation

Q2-Q3: Testing
- Onsite assessment
- Control verification
- Interview personnel
- Test systems

Q4: Report
- Findings documented
- Action plan created
- Management review
- Submit to acquiring bank

Ongoing:
- Quarterly scans
- Annual retesting
- Maintain compliance
```

## Status
✅ Implementation required | Mandatory for all card processors

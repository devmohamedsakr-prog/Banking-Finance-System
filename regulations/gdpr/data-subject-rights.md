# GDPR - Data Subject Rights Implementation

## 6 Core Rights

### 1. Right to Access (Article 15)
- Customer can request copy of personal data
- Timeline: Within 30 days (extendable 60 days)
- Format: Electronic or paper
- Cost: Free (1 copy/year), additional copies €5-25

Process:
```
1. Customer submits request (form on website)
2. Verify identity (email confirmation)
3. Locate all data
4. Compile data export (CSV/JSON)
5. Send to customer within 30 days
```

### 2. Right to Rectification (Article 16)
- Correct inaccurate data
- Complete incomplete data
- Timeline: Immediate
- Example: Update address

### 3. Right to Erasure (Article 17)
- "Right to be forgotten"
- Delete personal data
- Exceptions: Legal obligations, contract performance
- Cannot delete for 7 years (regulatory requirement)

### 4. Right to Restrict Processing (Article 18)
- Stop processing data (but keep it)
- Timeline: Immediate
- Use case: Disputed accuracy
- Resume processing when resolved

### 5. Right to Data Portability (Article 20)
- Export data in structured, machine-readable format
- JSON/CSV format
- Can transfer to another provider
- Timeline: 30 days

### 6. Right to Object (Article 21)
- Stop marketing communications
- Remove from email lists
- Timeline: Immediate
- "Do Not Contact" list

## Implementation

```python
# Customer Data Export API

def export_customer_data(customer_id):
    """Generate GDPR data export (Right to Access)"""
    
    data = {
        "personal_info": {
            "name": customer.name,
            "email": customer.email,
            "phone": customer.phone,
            "address": customer.address
        },
        "transactions": get_all_transactions(customer_id),
        "communications": get_all_emails(customer_id),
        "preferences": customer.preferences,
        "login_history": get_login_history(customer_id)
    }
    
    # Export as JSON
    export = json.dumps(data, indent=2)
    
    # Encrypt and send
    encrypted = encrypt_aes256(export)
    send_to_customer_email(customer_id, encrypted)
    
    # Log the request
    log_gdpr_request(customer_id, "DATA_EXPORT", now())
    
    return {"status": "SENT", "timestamp": now()}
```

## Data Processing Agreement

```
When processing EU customer data:

1. Have a DPA (Data Processing Agreement)
2. Ensure data is in EU or has adequacy decision
3. Use Standard Contractual Clauses (SCCs)
4. Include data processing terms:
   - Purpose (banking services)
   - Duration (as long as customer)
   - Scope (personal data for KYC)
   - Type of data (name, SSN, address)
   - Sub-processors (Plaid, etc.)
   - Security measures
```

## Compliance Checklist

- [ ] Privacy policy available in plain language
- [ ] Consent obtained before processing
- [ ] Data Processing Agreements with vendors
- [ ] Data retention policy documented
- [ ] Breach notification procedure (72 hours)
- [ ] DPIA (Data Protection Impact Assessment) for high-risk
- [ ] Incident response team assigned
- [ ] GDPR privacy training for all staff
- [ ] Annual compliance audit
- [ ] Register with data protection authority

## Penalties

- Fines up to €20M or 4% annual revenue (whichever higher)
- Examples: GDPR violations, €60-€746M fines

## Status
✅ Mandatory | EU customers required

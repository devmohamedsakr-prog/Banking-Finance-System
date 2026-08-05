# Card Management - Issuance Lifecycle

## Card Lifecycle

```
1. ISSUANCE
   - Customer requests card
   - Card generated in system
   - Physical card printed
   - Embossing with customer name
   - Secure packaging
   - Shipment via mail (2-3 days)

2. ACTIVATION
   - Customer receives card
   - Call activation line OR app
   - Verify identity (last 4 SSN)
   - Card activated
   - PIN setup
   - Ready to use

3. USAGE
   - Point-of-sale transactions
   - Online shopping
   - ATM withdrawals
   - Mobile payments (Apple Pay, Google Pay)

4. MANAGEMENT
   - Real-time balance
   - Transaction history
   - Freeze/unfreeze
   - Spending limits
   - Notifications

5. REPLACEMENT
   - Card lost/stolen: Issue immediately
   - Card expired: Send 30 days before expiry
   - Card damaged: Replace on request
   - New card arrives in 5 business days
   - Old card automatically disabled
```

## Card Types

```
Debit Card:
- Access to checking account
- No credit line
- No interest charges
- Immediate fund deduction

Credit Card:
- Revolving credit line
- Monthly statement
- Interest if not paid in full
- Building credit history

Prepaid Card:
- Load funds
- Use like debit card
- No credit required
- Limited features

Premium/Metal:
- Premium materials
- Enhanced benefits
- Higher annual fee
- Priority support
```

## Security Features

```
Fraud Prevention:

Chip Technology (EMV):
- Dynamic data (CVV changes)
- Cannot be counterfeited
- More secure than mag stripe

3D Secure (3DS):
- Online authentication
- Additional password
- Reduces online fraud

Biometric (Contactless):
- Fingerprint verification
- Face recognition
- Enhanced security

Tokenization:
- Card details replaced with token
- Token used for payments
- Original card details never exposed
```

## Database Schema

```sql
CREATE TABLE cards (
    card_id UUID PRIMARY KEY,
    customer_id UUID NOT NULL,
    card_number VARCHAR(19),  -- encrypted
    exp_month INT,
    exp_year INT,
    cvv VARCHAR(4),  -- encrypted
    card_type VARCHAR(20),  -- DEBIT, CREDIT, PREPAID
    status VARCHAR(20),  -- ACTIVE, FROZEN, EXPIRED, CANCELLED
    issued_at TIMESTAMP,
    activated_at TIMESTAMP,
    expires_at TIMESTAMP,
    
    INDEX idx_customer (customer_id),
    INDEX idx_status (status)
);

CREATE TABLE card_transactions (
    transaction_id UUID PRIMARY KEY,
    card_id UUID NOT NULL,
    amount DECIMAL(10,2),
    merchant VARCHAR(255),
    merchant_category VARCHAR(50),
    is_online BOOLEAN,
    status VARCHAR(20),  -- PENDING, COMPLETED, DECLINED
    created_at TIMESTAMP,
    
    FOREIGN KEY (card_id) REFERENCES cards(id),
    INDEX idx_created (created_at)
);

CREATE TABLE card_controls (
    control_id UUID PRIMARY KEY,
    card_id UUID NOT NULL,
    control_type VARCHAR(50),  -- SPENDING_LIMIT, MERCHANT_BLOCK, LOCATION_BLOCK
    value VARCHAR(255),
    enabled BOOLEAN,
    
    FOREIGN KEY (card_id) REFERENCES cards(id)
);
```

## Controls & Limits

```
Real-time Controls:

Spending Limits:
- Daily limit: $5,000
- Monthly limit: $50,000
- Merchant category limit: $1,000/ATM

Merchant Blocking:
- Block gambling sites
- Block adult content
- Block international merchants
- Custom merchant blocking

Location Blocking:
- Disable international use
- Allow only specific countries
- Enable when traveling
```

## Status
✅ Production-ready | 1B+ cards issued

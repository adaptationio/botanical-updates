# Data Encryption Standards

## Overview

This document outlines the encryption standards and practices for protecting patient health information (PHI) in the Botaniqal medical booking system, ensuring HIPAA compliance and Australian healthcare data protection requirements.

## Encryption Requirements

### Compliance Standards
- **HIPAA**: 45 CFR § 164.312(a)(2)(iv) and § 164.312(e)(2)(ii)
- **Australian Privacy Act**: APP 11 - Security of personal information
- **PCI DSS**: For payment card data
- **ISO 27001**: Information security management

## Encryption at Rest

### Database Encryption

#### PostgreSQL Transparent Data Encryption (TDE)
```yaml
Implementation: AWS RDS with encryption enabled
Algorithm: AES-256
Key Management: AWS KMS
Key Rotation: Annual automatic rotation
```

#### Application-Level Encryption
Sensitive fields encrypted before database storage:

```javascript
// Fields requiring application-level encryption
const ENCRYPTED_FIELDS = [
  'patients.medicare_number',
  'patients.health_fund_details',
  'form_submissions.form_data',
  'consultation_notes.subjective',
  'consultation_notes.objective',
  'consultation_notes.assessment',
  'consultation_notes.plan'
];
```

### File Storage Encryption

#### S3 Bucket Configuration
```yaml
Encryption: SSE-S3 (Server-Side Encryption)
Algorithm: AES-256
Additional: Bucket policies enforce encryption
Access: IAM roles with least privilege
```

#### Document Encryption Process
```
1. Client uploads document
2. Lambda function validates file
3. Encrypt with patient-specific key
4. Store in S3 with metadata
5. Log access in audit trail
```

### Backup Encryption

```yaml
Database Backups:
  Type: Automated RDS snapshots
  Encryption: Same KMS key as primary database
  Retention: 30 days encrypted storage

Application Backups:
  Type: S3 lifecycle policies
  Encryption: SSE-KMS with separate backup key
  Cross-Region: Encrypted replication to Sydney/Melbourne
```

## Encryption in Transit

### TLS Configuration

#### Minimum Requirements
```nginx
# Nginx SSL Configuration
ssl_protocols TLSv1.2 TLSv1.3;
ssl_ciphers 'ECDHE-RSA-AES128-GCM-SHA256:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-RSA-CHACHA20-POLY1305';
ssl_prefer_server_ciphers on;
ssl_session_timeout 1d;
ssl_session_cache shared:SSL:50m;
ssl_stapling on;
ssl_stapling_verify on;
```

#### Certificate Management
- **Provider**: AWS Certificate Manager
- **Type**: Wildcard certificate for *.botaniqal.com.au
- **Renewal**: Automatic via ACM
- **HSTS**: Enabled with 1-year max-age

### API Security

#### Request Encryption
```javascript
// All API requests must use HTTPS
const apiClient = axios.create({
  baseURL: 'https://api.botaniqal.com.au',
  timeout: 30000,
  headers: {
    'Content-Type': 'application/json',
    'X-API-Version': '1.0'
  },
  httpsAgent: new https.Agent({
    rejectUnauthorized: true,
    minVersion: 'TLSv1.2'
  })
});
```

#### Response Encryption
- All responses over HTTPS
- Sensitive data additionally encrypted in payload
- No PHI in URL parameters or headers

### Webhook Security

```javascript
// Webhook payload encryption
const encryptWebhookPayload = (data) => {
  const cipher = crypto.createCipheriv(
    'aes-256-gcm',
    Buffer.from(WEBHOOK_ENCRYPTION_KEY, 'hex'),
    crypto.randomBytes(16)
  );
  
  const encrypted = Buffer.concat([
    cipher.update(JSON.stringify(data), 'utf8'),
    cipher.final()
  ]);
  
  return {
    data: encrypted.toString('base64'),
    authTag: cipher.getAuthTag().toString('base64'),
    iv: iv.toString('base64')
  };
};
```

## Key Management

### AWS Key Management Service (KMS)

#### Key Hierarchy
```
Root Key (KMS Customer Master Key)
├── Database Encryption Key (DEK)
├── Application Encryption Key
├── Backup Encryption Key
└── File Storage Encryption Key
```

#### Key Policies
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Enable IAM User Permissions",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::ACCOUNT:root"
      },
      "Action": "kms:*",
      "Resource": "*"
    },
    {
      "Sid": "Allow use of the key for encryption",
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::ACCOUNT:role/BotaniqalAppRole",
          "arn:aws:iam::ACCOUNT:role/BotaniqalLambdaRole"
        ]
      },
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:GenerateDataKey"
      ],
      "Resource": "*"
    }
  ]
}
```

### Key Rotation

#### Automatic Rotation Schedule
- **KMS Keys**: Annual rotation
- **Application Keys**: Quarterly rotation
- **API Keys**: Monthly rotation
- **Session Keys**: Per session

#### Manual Key Rotation Process
```bash
# 1. Generate new key version
aws kms create-key-version --key-id $KEY_ID

# 2. Update application configuration
kubectl set secret encryption-keys --from-literal=new-key=$NEW_KEY

# 3. Re-encrypt existing data (background job)
npm run rotate-encryption-keys --batch-size=1000

# 4. Verify and cleanup old keys
npm run verify-key-rotation
```

## Application-Level Encryption

### Patient Data Encryption

```javascript
class PatientDataEncryption {
  constructor() {
    this.algorithm = 'aes-256-gcm';
    this.keyDerivation = 'pbkdf2';
    this.iterations = 100000;
  }

  encryptField(plaintext, patientId) {
    // Derive patient-specific key
    const salt = crypto.createHash('sha256')
      .update(patientId)
      .digest();
    
    const key = crypto.pbkdf2Sync(
      process.env.MASTER_ENCRYPTION_KEY,
      salt,
      this.iterations,
      32,
      'sha256'
    );

    // Encrypt data
    const iv = crypto.randomBytes(16);
    const cipher = crypto.createCipheriv(this.algorithm, key, iv);
    
    const encrypted = Buffer.concat([
      cipher.update(plaintext, 'utf8'),
      cipher.final()
    ]);

    return {
      encrypted: encrypted.toString('base64'),
      iv: iv.toString('base64'),
      authTag: cipher.getAuthTag().toString('base64')
    };
  }

  decryptField(encryptedData, patientId) {
    // Similar process for decryption
    // Includes integrity verification via authTag
  }
}
```

### Form Data Encryption

```javascript
// Encrypt form submissions before storage
const encryptFormSubmission = async (formData, patientId) => {
  const sensitiveFields = [
    'medicareNumber',
    'healthFundNumber',
    'medicalHistory',
    'medications',
    'allergies'
  ];

  const encryptedData = { ...formData };
  
  for (const field of sensitiveFields) {
    if (encryptedData[field]) {
      encryptedData[field] = await encrypt(
        encryptedData[field],
        `${patientId}-${field}`
      );
    }
  }

  return encryptedData;
};
```

## Encryption for Specific Services

### Telehealth Video Encryption
- **Protocol**: SRTP (Secure Real-time Transport Protocol)
- **Key Exchange**: DTLS-SRTP
- **Video Codec**: VP8/VP9 with encryption
- **End-to-End**: Optional E2EE for sensitive consultations

### Email Encryption
```yaml
Patient Communications:
  Transport: TLS 1.2+ required
  Content: S/MIME for PHI attachments
  Storage: Encrypted at rest in mail servers

Automated Emails:
  Service: AWS SES with TLS
  Templates: No PHI in email body
  Links: Secure portal access only
```

### Calendar Integration
```javascript
// Calendly webhook data handling
const processCalendlyWebhook = (encryptedPayload) => {
  // Decrypt webhook payload
  const decrypted = decryptWebhookData(encryptedPayload);
  
  // Remove PHI before logging
  const sanitized = removePHI(decrypted);
  logger.info('Calendly event received', sanitized);
  
  // Process with encrypted storage
  return storeBookingData(decrypted);
};
```

## Monitoring and Compliance

### Encryption Monitoring
```yaml
CloudWatch Metrics:
  - Encryption failures
  - Key usage patterns
  - TLS handshake failures
  - Certificate expiry warnings

Alerts:
  - Failed decryption attempts > 5/minute
  - Expired certificates
  - Key rotation failures
  - Weak cipher usage attempts
```

### Audit Requirements
```sql
-- Encryption audit log entries
INSERT INTO audit_log (
  action,
  resource_type,
  details,
  user_id,
  timestamp
) VALUES (
  'DECRYPT_PHI',
  'patient_record',
  '{"field": "medical_history", "reason": "consultation"}',
  :practitioner_id,
  NOW()
);
```

## Incident Response

### Encryption Key Compromise
1. **Immediate Actions**
   - Revoke compromised key
   - Generate new keys
   - Alert security team
   
2. **Assessment**
   - Identify affected data
   - Determine exposure window
   - Document access logs

3. **Remediation**
   - Re-encrypt affected data
   - Update all systems
   - Notify compliance officer

4. **Prevention**
   - Review key access policies
   - Enhance monitoring
   - Update security training

## Development Guidelines

### Secure Coding Practices
```javascript
// DO: Use established encryption libraries
const crypto = require('crypto');
const encrypted = crypto.publicEncrypt(publicKey, Buffer.from(data));

// DON'T: Implement custom encryption
// const encrypted = customXOR(data, key); // NEVER DO THIS

// DO: Clear sensitive data from memory
let sensitiveData = getPatientData();
processData(sensitiveData);
sensitiveData = null; // Clear reference

// DO: Use constant-time comparison for secrets
const timingSafeEqual = crypto.timingSafeEqual(
  Buffer.from(providedToken),
  Buffer.from(storedToken)
);
```

### Testing Encryption
```javascript
describe('Encryption Tests', () => {
  test('Patient data encryption', async () => {
    const testData = 'Sensitive medical information';
    const encrypted = await encryptPatientData(testData);
    
    expect(encrypted).not.toBe(testData);
    expect(encrypted.authTag).toBeDefined();
    
    const decrypted = await decryptPatientData(encrypted);
    expect(decrypted).toBe(testData);
  });

  test('Encryption key rotation', async () => {
    const oldKey = await getCurrentKey();
    await rotateEncryptionKey();
    const newKey = await getCurrentKey();
    
    expect(newKey).not.toBe(oldKey);
    // Verify old data still accessible
    const oldData = await decryptWithKey(encryptedData, oldKey);
    expect(oldData).toBeDefined();
  });
});
```

## Compliance Checklist

- [ ] All PHI encrypted at rest (AES-256)
- [ ] All transmissions use TLS 1.2+
- [ ] Key rotation implemented and tested
- [ ] Encryption monitoring active
- [ ] Audit logging for all decrypt operations
- [ ] Incident response plan documented
- [ ] Regular encryption testing
- [ ] Staff training on encryption practices
- [ ] Third-party BAAs include encryption requirements
- [ ] Annual encryption audit scheduled

---

*Last Updated: [Current Date]*
*Version: 1.0.0*
*Next Review: [Annual]*
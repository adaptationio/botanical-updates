# Database Schema Documentation

## Overview

This document defines the database structure for the Botaniqal medical booking system. The schema is designed to support:
- Multi-practitioner medical bookings
- Patient health records (HIPAA compliant)
- Dynamic form submissions
- Audit trails and compliance tracking
- Service-based pricing and appointments

## Database Engine

- **Primary**: PostgreSQL 14.x
- **Replication**: Read replicas for reporting
- **Backup**: Daily automated snapshots with 30-day retention

## Core Tables

### 1. patients

Stores patient demographic and contact information.

```sql
CREATE TABLE patients (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    -- Basic Information
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    date_of_birth DATE NOT NULL,
    gender VARCHAR(20),
    
    -- Contact Information
    email VARCHAR(255) UNIQUE NOT NULL,
    phone VARCHAR(20) NOT NULL,
    phone_country_code VARCHAR(5) DEFAULT '+61',
    
    -- Address
    address_line1 VARCHAR(255),
    address_line2 VARCHAR(255),
    suburb VARCHAR(100),
    state VARCHAR(50),
    postcode VARCHAR(10),
    country VARCHAR(100) DEFAULT 'Australia',
    
    -- Account Information
    password_hash VARCHAR(255),
    email_verified BOOLEAN DEFAULT FALSE,
    phone_verified BOOLEAN DEFAULT FALSE,
    
    -- Preferences
    timezone VARCHAR(50) DEFAULT 'Australia/Sydney',
    preferred_language VARCHAR(10) DEFAULT 'en',
    communication_preferences JSONB DEFAULT '{"email": true, "sms": true}',
    
    -- Metadata
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    deleted_at TIMESTAMP WITH TIME ZONE,
    
    -- Indexes
    INDEX idx_patients_email (email),
    INDEX idx_patients_phone (phone),
    INDEX idx_patients_name (last_name, first_name)
);
```

### 2. practitioners

Stores healthcare provider information.

```sql
CREATE TABLE practitioners (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    -- Basic Information
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    title VARCHAR(50), -- Dr., RN, etc.
    
    -- Professional Information
    practitioner_type VARCHAR(50) NOT NULL, -- 'doctor', 'nurse', 'consultant', 'coach'
    specializations TEXT[],
    registration_number VARCHAR(100),
    registration_body VARCHAR(100),
    
    -- Availability
    calendly_url VARCHAR(255),
    office365_calendar_id VARCHAR(255),
    consultation_types JSONB, -- Types of appointments they offer
    
    -- Location
    practice_locations JSONB, -- Multiple locations support
    primary_location VARCHAR(50), -- 'telehealth', 'melbourne', etc.
    
    -- Status
    is_active BOOLEAN DEFAULT TRUE,
    accepting_new_patients BOOLEAN DEFAULT TRUE,
    
    -- Metadata
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    -- Indexes
    INDEX idx_practitioners_type (practitioner_type),
    INDEX idx_practitioners_active (is_active)
);
```

### 3. services

Defines available medical services and pricing.

```sql
CREATE TABLE services (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    -- Service Information
    service_code VARCHAR(50) UNIQUE NOT NULL,
    service_name VARCHAR(200) NOT NULL,
    service_category VARCHAR(100) NOT NULL, -- 'consultation', 'alternative-medicine', 'gaps', 'weight-loss'
    description TEXT,
    
    -- Pricing
    initial_consultation_price DECIMAL(10, 2),
    initial_consultation_duration INTEGER, -- minutes
    followup_price DECIMAL(10, 2),
    followup_duration INTEGER, -- minutes
    
    -- Availability
    is_active BOOLEAN DEFAULT TRUE,
    available_online BOOLEAN DEFAULT TRUE,
    available_in_person BOOLEAN DEFAULT FALSE,
    
    -- Requirements
    requires_referral BOOLEAN DEFAULT FALSE,
    requires_initial_consultation BOOLEAN DEFAULT TRUE,
    min_age_requirement INTEGER,
    
    -- Metadata
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    -- Indexes
    INDEX idx_services_category (service_category),
    INDEX idx_services_active (is_active)
);
```

### 4. bookings

Central booking table for all appointments.

```sql
CREATE TABLE bookings (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    -- Relationships
    patient_id UUID NOT NULL REFERENCES patients(id),
    practitioner_id UUID NOT NULL REFERENCES practitioners(id),
    service_id UUID NOT NULL REFERENCES services(id),
    
    -- Booking Details
    booking_reference VARCHAR(20) UNIQUE NOT NULL,
    appointment_type VARCHAR(50) NOT NULL, -- 'initial', 'followup'
    scheduled_start TIMESTAMP WITH TIME ZONE NOT NULL,
    scheduled_end TIMESTAMP WITH TIME ZONE NOT NULL,
    actual_start TIMESTAMP WITH TIME ZONE,
    actual_end TIMESTAMP WITH TIME ZONE,
    
    -- Status
    status VARCHAR(50) NOT NULL DEFAULT 'pending', -- 'pending', 'confirmed', 'completed', 'cancelled', 'no-show'
    cancellation_reason TEXT,
    cancelled_by VARCHAR(50), -- 'patient', 'practitioner', 'system'
    cancelled_at TIMESTAMP WITH TIME ZONE,
    
    -- Payment
    payment_status VARCHAR(50) DEFAULT 'pending', -- 'pending', 'paid', 'refunded', 'failed'
    payment_method VARCHAR(50), -- 'online', 'phone', 'insurance'
    amount_charged DECIMAL(10, 2),
    amount_paid DECIMAL(10, 2),
    
    -- Source
    booking_source VARCHAR(50) NOT NULL, -- 'online', 'phone', 'walk-in', 'referral'
    created_by_user_id UUID, -- For phone bookings
    referral_source VARCHAR(100),
    
    -- Integration IDs
    calendly_event_id VARCHAR(255),
    medirecords_appointment_id VARCHAR(255),
    stripe_payment_intent_id VARCHAR(255),
    
    -- Meeting Details
    meeting_url VARCHAR(500), -- For telehealth
    meeting_password VARCHAR(50),
    
    -- Notes
    admin_notes TEXT,
    practitioner_notes TEXT,
    
    -- Metadata
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    -- Indexes
    INDEX idx_bookings_patient (patient_id),
    INDEX idx_bookings_practitioner (practitioner_id),
    INDEX idx_bookings_scheduled (scheduled_start),
    INDEX idx_bookings_status (status),
    INDEX idx_bookings_reference (booking_reference)
);
```

### 5. form_submissions

Stores all form data (intake, triage, etc.).

```sql
CREATE TABLE form_submissions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    -- Relationships
    patient_id UUID REFERENCES patients(id),
    booking_id UUID REFERENCES bookings(id),
    
    -- Form Information
    form_type VARCHAR(100) NOT NULL, -- 'mini_intake', 'full_intake', 'triage', 'consultation_notes'
    form_version VARCHAR(20) NOT NULL,
    
    -- Submission Data
    form_data JSONB NOT NULL, -- Encrypted in application layer
    
    -- Consent
    consent_given BOOLEAN DEFAULT FALSE,
    consent_timestamp TIMESTAMP WITH TIME ZONE,
    consent_ip_address INET,
    
    -- Processing
    is_processed BOOLEAN DEFAULT FALSE,
    processed_at TIMESTAMP WITH TIME ZONE,
    processing_notes TEXT,
    
    -- Source
    submission_source VARCHAR(50), -- 'jotform', 'internal', 'api'
    external_form_id VARCHAR(255), -- JotForm ID, etc.
    
    -- Metadata
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    -- Indexes
    INDEX idx_form_submissions_patient (patient_id),
    INDEX idx_form_submissions_booking (booking_id),
    INDEX idx_form_submissions_type (form_type),
    INDEX idx_form_submissions_processed (is_processed)
);
```

### 6. consultation_notes

Clinical notes from consultations (separate for security).

```sql
CREATE TABLE consultation_notes (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    -- Relationships
    booking_id UUID NOT NULL REFERENCES bookings(id),
    practitioner_id UUID NOT NULL REFERENCES practitioners(id),
    patient_id UUID NOT NULL REFERENCES patients(id),
    
    -- Note Content
    chief_complaint TEXT,
    subjective TEXT, -- SOAP format
    objective TEXT,
    assessment TEXT,
    plan TEXT,
    
    -- Recommendations
    referrals JSONB,
    prescriptions JSONB,
    follow_up_required BOOLEAN DEFAULT FALSE,
    follow_up_timeframe VARCHAR(100),
    
    -- Attachments
    attachments JSONB, -- References to S3 objects
    
    -- Sign-off
    signed_by_practitioner BOOLEAN DEFAULT FALSE,
    signed_at TIMESTAMP WITH TIME ZONE,
    
    -- Metadata
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    -- Indexes
    INDEX idx_consultation_notes_booking (booking_id),
    INDEX idx_consultation_notes_patient (patient_id)
);
```

### 7. audit_log

Comprehensive audit trail for HIPAA compliance.

```sql
CREATE TABLE audit_log (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    -- User Information
    user_id UUID,
    user_type VARCHAR(50), -- 'patient', 'practitioner', 'admin', 'system'
    ip_address INET,
    user_agent TEXT,
    
    -- Action Details
    action VARCHAR(100) NOT NULL, -- 'view', 'create', 'update', 'delete', 'export', 'print'
    resource_type VARCHAR(100) NOT NULL, -- Table name
    resource_id UUID,
    
    -- Changes
    old_values JSONB,
    new_values JSONB,
    
    -- Context
    request_id UUID,
    session_id VARCHAR(255),
    
    -- Timestamp
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    -- Indexes
    INDEX idx_audit_log_user (user_id),
    INDEX idx_audit_log_resource (resource_type, resource_id),
    INDEX idx_audit_log_timestamp (created_at),
    INDEX idx_audit_log_action (action)
);
```

### 8. practitioner_services

Junction table for practitioner-service relationships.

```sql
CREATE TABLE practitioner_services (
    practitioner_id UUID NOT NULL REFERENCES practitioners(id),
    service_id UUID NOT NULL REFERENCES services(id),
    
    -- Custom Pricing (overrides service defaults)
    custom_initial_price DECIMAL(10, 2),
    custom_followup_price DECIMAL(10, 2),
    
    -- Availability
    is_active BOOLEAN DEFAULT TRUE,
    max_daily_appointments INTEGER,
    
    -- Metadata
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    PRIMARY KEY (practitioner_id, service_id),
    INDEX idx_practitioner_services_service (service_id)
);
```

### 9. availability_schedules

Practitioner availability patterns.

```sql
CREATE TABLE availability_schedules (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    practitioner_id UUID NOT NULL REFERENCES practitioners(id),
    
    -- Schedule Pattern
    day_of_week INTEGER NOT NULL, -- 0 = Sunday, 6 = Saturday
    start_time TIME NOT NULL,
    end_time TIME NOT NULL,
    
    -- Service Restrictions
    service_ids UUID[], -- Null means all services
    
    -- Location
    location VARCHAR(100), -- 'telehealth', 'melbourne', etc.
    
    -- Valid Period
    effective_from DATE NOT NULL,
    effective_until DATE,
    
    -- Status
    is_active BOOLEAN DEFAULT TRUE,
    
    -- Metadata
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    
    -- Indexes
    INDEX idx_availability_practitioner (practitioner_id),
    INDEX idx_availability_day (day_of_week),
    INDEX idx_availability_active (is_active)
);
```

### 10. payment_transactions

Payment record keeping.

```sql
CREATE TABLE payment_transactions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    -- Relationships
    booking_id UUID NOT NULL REFERENCES bookings(id),
    patient_id UUID NOT NULL REFERENCES patients(id),
    
    -- Transaction Details
    transaction_type VARCHAR(50) NOT NULL, -- 'payment', 'refund', 'partial_refund'
    amount DECIMAL(10, 2) NOT NULL,
    currency VARCHAR(3) DEFAULT 'AUD',
    
    -- Payment Method
    payment_method VARCHAR(50) NOT NULL, -- 'card', 'bank_transfer', 'cash', 'insurance'
    payment_provider VARCHAR(50), -- 'stripe', 'manual', etc.
    
    -- External References
    provider_transaction_id VARCHAR(255),
    provider_customer_id VARCHAR(255),
    
    -- Status
    status VARCHAR(50) NOT NULL, -- 'pending', 'processing', 'succeeded', 'failed'
    failure_reason TEXT,
    
    -- Metadata
    metadata JSONB,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP,
    processed_at TIMESTAMP WITH TIME ZONE,
    
    -- Indexes
    INDEX idx_payment_transactions_booking (booking_id),
    INDEX idx_payment_transactions_patient (patient_id),
    INDEX idx_payment_transactions_status (status)
);
```

## Views

### patient_booking_history

Comprehensive view of patient bookings.

```sql
CREATE VIEW patient_booking_history AS
SELECT 
    b.id as booking_id,
    b.booking_reference,
    b.scheduled_start,
    b.status,
    p.first_name || ' ' || p.last_name as patient_name,
    pr.first_name || ' ' || pr.last_name as practitioner_name,
    s.service_name,
    b.appointment_type,
    b.amount_charged,
    b.payment_status
FROM bookings b
JOIN patients p ON b.patient_id = p.id
JOIN practitioners pr ON b.practitioner_id = pr.id
JOIN services s ON b.service_id = s.id
WHERE p.deleted_at IS NULL
ORDER BY b.scheduled_start DESC;
```

### daily_booking_summary

Admin dashboard summary.

```sql
CREATE VIEW daily_booking_summary AS
SELECT 
    DATE(scheduled_start) as booking_date,
    COUNT(*) as total_bookings,
    COUNT(CASE WHEN status = 'completed' THEN 1 END) as completed,
    COUNT(CASE WHEN status = 'cancelled' THEN 1 END) as cancelled,
    COUNT(CASE WHEN status = 'no-show' THEN 1 END) as no_shows,
    SUM(amount_paid) as revenue
FROM bookings
GROUP BY DATE(scheduled_start)
ORDER BY booking_date DESC;
```

## Security Considerations

1. **Encryption at Rest**
   - All tables containing PHI use PostgreSQL's transparent data encryption
   - Backup files are encrypted using AES-256

2. **Encryption in Transit**
   - All connections use SSL/TLS
   - Minimum TLS version: 1.2

3. **Access Control**
   - Row-level security enabled for patient data
   - Separate read-only user for reporting
   - Audit log cannot be modified or deleted

4. **Data Retention**
   - Patient data: 7 years minimum (Australian healthcare requirement)
   - Audit logs: 3 years minimum
   - Soft deletes for patient records

## Backup and Recovery

1. **Backup Schedule**
   - Full backup: Daily at 2 AM AEST
   - Incremental backup: Every 4 hours
   - Transaction logs: Continuous archiving

2. **Recovery Objectives**
   - RPO (Recovery Point Objective): 4 hours
   - RTO (Recovery Time Objective): 2 hours

3. **Testing**
   - Monthly restore tests
   - Quarterly disaster recovery drills

## Performance Optimization

1. **Indexes**
   - All foreign keys indexed
   - Composite indexes for common queries
   - Partial indexes for status fields

2. **Partitioning**
   - Audit_log partitioned by month
   - Bookings partitioned by year

3. **Connection Pooling**
   - Maximum connections: 100
   - Pool size: 20
   - Timeout: 30 seconds

## Migration Strategy

1. **Version Control**
   - All schema changes in migration files
   - Rollback scripts for each migration
   - Schema version tracked in database

2. **Deployment Process**
   - Test migrations on staging first
   - Backup before production migration
   - Monitor performance after changes

---

*Last Updated: [Current Date]*
*Version: 1.0.0*
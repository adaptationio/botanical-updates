# Pricing Changes Summary

## Current System Pricing (What exists now)
- **Initial Consultation**: $89 (15 minutes)
- **Follow-up Consultation**: $69 (10 minutes)
- **Single Provider**: Dr. Dia only
- **Payment Method**: Online via Calendly/Stripe

## New System Pricing (What needs to be implemented)

### Free Triage Layer (NEW)
- **Free Initial Consultation**: $0 (20 minutes)
- **Purpose**: Assessment and practitioner matching
- **No payment collection**

### Alternative Medicine Services
| Service Type | Initial | Follow-up | Notes |
|--------------|---------|-----------|-------|
| **Telehealth** | $119 (15 min) | $79 (10 min) | +$30/$10 increase |
| **In-Person (Melbourne)** | $119 (20 min) | $79 (15 min) | New service option |

### GAPS Diet Coaching (NEW SERVICE)
- **Initial**: $195 (60 minutes)
- **Follow-up**: $79 (15 minutes)

## Key Pricing Changes

1. **Price Increase**: 
   - Initial: $89 → $119 (+33.7%)
   - Follow-up: $69 → $79 (+14.5%)

2. **Free Entry Point**:
   - NEW: Free 20-minute consultation
   - Reduces barrier to entry
   - Allows proper assessment before payment

3. **Service Differentiation**:
   - In-person appointments get extra time (20/15 min vs 15/10 min)
   - GAPS coaching has longer initial session (60 min)

## Implementation Notes

### Calendly Configuration
```yaml
Current:
  - Initial Consultation: $89
  - Follow-up: $69
  
New Setup:
  - Triage Calendar: FREE (no Stripe integration)
  - Alt Medicine Initial: $119 (collected via phone)
  - Alt Medicine Follow-up: $79 (collected via phone)
  - GAPS Initial: $195 (collected via phone)
  - GAPS Follow-up: $79 (collected via phone)
```

### Payment Collection Change
- **Current**: Automated via Calendly Stripe integration
- **New**: Manual collection over phone by consultant
- **Reason**: Allows triage before payment commitment

### Database Updates Required
```sql
-- Add service pricing table
CREATE TABLE service_pricing (
    service_type VARCHAR(50),
    appointment_type VARCHAR(20), -- 'initial' or 'followup'
    price DECIMAL(10, 2),
    duration_minutes INTEGER,
    effective_date DATE
);

-- Insert new pricing
INSERT INTO service_pricing VALUES
('alternative_medicine_telehealth', 'initial', 119.00, 15, CURRENT_DATE),
('alternative_medicine_telehealth', 'followup', 79.00, 10, CURRENT_DATE),
('alternative_medicine_inperson', 'initial', 119.00, 20, CURRENT_DATE),
('alternative_medicine_inperson', 'followup', 79.00, 15, CURRENT_DATE),
('gaps_coaching', 'initial', 195.00, 60, CURRENT_DATE),
('gaps_coaching', 'followup', 79.00, 15, CURRENT_DATE),
('triage_consultation', 'initial', 0.00, 20, CURRENT_DATE);
```

---

**Summary**: The system is moving from a simple $89/$69 pricing model to a more complex structure with free triage, multiple service types, and higher pricing tiers ($119/$79 for standard services, $195/$79 for GAPS).
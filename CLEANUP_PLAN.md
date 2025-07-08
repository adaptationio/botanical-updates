# Botanical Updates - Cleanup Action Plan

## Priority 1: Critical Content Issues 🚨

### 1. Remove Plant E-commerce Content
**Issue**: User experience flows are for a plant shopping website, not medical booking system.

**Files to Delete or Replace**:
```
❌ /flowcharts/user-experience/current/user-flow.md
❌ /flowcharts/user-experience/new-update/user-flow.md  
❌ /flowcharts/user-experience/proposed/user-flow.md
```

**Current Content (WRONG)**:
- Browse Plants
- Plant Details Page
- Shopping Cart
- Plant Categories
- Order Confirmation

**What Should Replace It**:
- Find a Practitioner
- Book Consultation
- Complete Health Assessment
- Payment for Medical Services
- Appointment Confirmation

**Action**: Either DELETE these files entirely OR create new medical booking user flows.

### 2. Fix Remaining Pricing Inconsistencies
**Issue**: Some files still show incorrect pricing.

**Files to Check and Fix**:
- [ ] `/flowcharts/booking/current/comprehensive-booking-flow.md` - May show old $89/$69 pricing
- [ ] `/flowcharts/admin/current/booking-admin-flow.md` - Verify pricing references
- [ ] `/flowcharts/tech/current/booking-tech-flow.md` - Check payment processing amounts

**Correct Pricing to Use Everywhere**:
```
Free Consultation: $0 (20 min) - Consultant
Alternative Medicine Initial: $119 (15 min)
Alternative Medicine Follow-up: $79 (10 min)
GAPS Initial: $195 (60 min)
GAPS Follow-up: $79 (15 min)
In-Person Melbourne: Same prices (20/15 min)
```

## Priority 2: Unrealistic Content ⚠️

### 3. Remove or Relabel "Proposed" Sci-Fi Features
**Issue**: Proposed sections contain unrealistic technology that confuses the actual implementation.

**Files with Unrealistic Content**:
```
/flowcharts/admin/proposed/admin-flow.md
- Contains: "VR meeting rooms", "AI that negotiates with suppliers", "90% automation"
- Action: DELETE or rename to "admin-flow-future-vision-2035.md"

/flowcharts/tech/proposed/tech-flow.md
- Contains: "Quantum-ready algorithms", "Blockchain integration", "Zero-latency global network"
- Action: DELETE or rename to "tech-flow-moonshot-ideas.md"

/flowcharts/user-experience/proposed/user-flow.md
- Contains: "AR/VR plant preview", "AI companion", "Metaverse integration"
- Action: DELETE (it's also about plants, not medical)
```

**Recommendation**: DELETE all proposed directories entirely. They add confusion without value.

### 4. Complete or Remove Placeholder Content
**Issue**: Several files have "To be detailed..." or "awaiting specifications"

**Files to Fix**:
```
/flowcharts/booking/proposed/patient-booking-flow.md
- Current: Just says "To be detailed based on future vision discussions"
- Action: DELETE this empty file

/flowcharts/admin/new-update/booking-admin-flow.md
- Check if still says "awaiting specifications"
- Action: Either complete it or remove the reference from README
```

## Priority 3: Structural Cleanup 🔧

### 5. Consolidate Service Information
**Issue**: Inconsistent practitioner counts and service status across files.

**Create Single Source of Truth**:
Create `/SERVICES_AND_PRICING.md` with:
```markdown
# Official Services and Pricing

## Current Practitioners (6 Total)
1. Consultant - Free initial assessments
2. Doctor 1 - Telehealth
3. Doctor 2 - Telehealth  
4. Dr. Shivani - In-person Melbourne
5. Nurse Practitioner - Telehealth
6. GAPS Coach - Nutrition

## Active Services
- Alternative Medicine (Telehealth & In-person)
- GAPS Diet Coaching

## Coming Soon Services
- Weight Loss Program (Launch TBD)
- Online Counseling (Launch TBD)
- Equine Therapy (Launch TBD)

## Pricing (Current as of July 2025)
[Insert pricing table]
```

Then update all files to reference this single source.

### 6. Rename Confusing Directories
**Issue**: "proposed" vs "variations" is confusing.

**Current Structure** (Confusing):
```
/flowcharts/
├── booking/
│   ├── current/
│   ├── new-update/
│   ├── proposed/        ← What is this?
│   └── analysis/
└── variations/          ← How is this different from proposed?
    ├── coming-soon/
    └── fully-operational/
```

**Suggested New Structure** (Clear):
```
/flowcharts/
├── booking/
│   ├── current/         # What exists today
│   ├── new-update/      # Ready to deploy
│   └── analysis/        # Supporting docs
└── future-options/      # Rename variations
    ├── option-1-gradual/   # Was coming-soon
    └── option-2-complete/  # Was fully-operational
```

## Priority 4: Missing Critical Documentation 📋

### 7. Create Minimum Viable Documentation
**Issue**: Missing security, API, and deployment docs for a healthcare system.

**Create These Essential Files**:
```
/docs/security/
├── HIPAA_COMPLIANCE_CHECKLIST.md
├── DATA_ENCRYPTION.md
└── ACCESS_CONTROL.md

/docs/technical/
├── API_ENDPOINTS.md
├── DATABASE_SCHEMA.md
└── DEPLOYMENT_GUIDE.md

/docs/integration/
├── CALENDLY_SETUP.md
├── MEDIRECORDS_CONFIG.md
└── STRIPE_INTEGRATION.md
```

## Quick Wins (Do These First) ✅

1. **Delete all plant shop files** (user-experience directory)
2. **Delete all empty/placeholder files**
3. **Delete or archive all "proposed" directories**
4. **Create SERVICES_AND_PRICING.md as single source of truth**
5. **Fix any remaining $89/$69 pricing to $119/$79 and $195/$79**

## Verification Checklist

After cleanup, verify:
- [ ] No plant/e-commerce references remain
- [ ] All pricing shows $119/$79 for Alt Med, $195/$79 for GAPS
- [ ] Practitioner count is consistent (6 current)
- [ ] No sci-fi features in main documentation
- [ ] No empty placeholder files
- [ ] Clear distinction between current/new-update/future-options

## Time Estimate

- Quick Wins: 1-2 hours
- Full Cleanup: 4-6 hours
- Adding Missing Docs: 8-12 hours

## Decision Points for Team

1. **Keep or delete** user experience flows? (Currently about plants)
2. **Keep or delete** all "proposed" content? (Currently sci-fi)
3. **Prioritize missing docs** - which are needed for launch?
4. **Rename directories** for clarity?

## Final Note

The booking flow documentation is excellent and doesn't need changes beyond pricing fixes. The cleanup is mainly removing unrelated content and adding missing technical documentation. Focus on the medical booking system, remove the plant shop content, and this will be production-ready documentation.
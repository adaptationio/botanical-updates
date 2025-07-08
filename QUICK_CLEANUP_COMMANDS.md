# Quick Cleanup Commands

## 🚨 REVIEW BEFORE RUNNING - These commands will DELETE files

### 1. Remove All Plant Shop User Experience Flows
```bash
# Preview what will be deleted
ls -la flowcharts/user-experience/*/

# DELETE plant shop flows (if confirmed they're not needed)
rm -rf flowcharts/user-experience/
```

### 2. Remove All Empty/Proposed Directories
```bash
# Find and list empty or proposed files
find flowcharts -name "*proposed*" -type f
find flowcharts -type f -exec grep -l "To be detailed" {} \;

# DELETE all proposed directories (if confirmed)
find flowcharts -type d -name "proposed" -exec rm -rf {} \;

# DELETE specific empty file
rm flowcharts/booking/proposed/patient-booking-flow.md
```

### 3. Fix Remaining Pricing Issues
```bash
# Find files with old pricing
grep -r "\$89" flowcharts/ --include="*.md"
grep -r "\$69" flowcharts/ --include="*.md"

# These files likely need manual pricing updates:
# - flowcharts/booking/current/comprehensive-booking-flow.md
# - flowcharts/booking/current/patient-booking-flow.md (already fixed)
# - flowcharts/admin/current/booking-admin-flow.md
```

### 4. Create Services and Pricing Reference
```bash
# Create authoritative pricing document
cat > SERVICES_AND_PRICING.md << 'EOF'
# Official Services and Pricing Reference

## Current Services & Pricing (July 2025)

| Service | Initial Consult | Follow-up | Provider |
|---------|----------------|-----------|----------|
| Free Consultation | FREE (20 min) | N/A | Consultant |
| Alternative Medicine | $119 (15 min) | $79 (10 min) | Telehealth Doctors/Nurse |
| Alternative Medicine | $119 (20 min) | $79 (15 min) | Dr. Shivani (Melbourne) |
| GAPS Diet Coaching | $195 (60 min) | $79 (15 min) | GAPS Coach |
| Weight Loss | TBD | TBD | Coming Soon |
| Online Counseling | TBD | TBD | Coming Soon |
| Equine Therapy | TBD | TBD | Coming Soon |

## Current Practitioners (6)
1. Consultant - Free initial assessments
2. Doctor 1 - Telehealth alternative medicine
3. Doctor 2 - Telehealth alternative medicine
4. Dr. Shivani - In-person Melbourne clinic
5. Nurse Practitioner - Telehealth alternative medicine
6. GAPS Coach - Nutrition and GAPS diet

## Future Practitioners (When Fully Operational: 8)
7. Counselor - Online mental health services
8. Equine Therapist - Equine-assisted therapy
EOF
```

### 5. Quick Directory Structure Check
```bash
# See current structure
tree flowcharts -d -L 2

# Count total files
find flowcharts -name "*.md" -type f | wc -l

# List all proposed/future vision files
find flowcharts -name "*proposed*" -o -name "*future*" | sort
```

### 6. Verify No Plant References Remain
```bash
# Search for plant-related terms
grep -r -i "plant\|garden\|botanical\|flower\|seed" flowcharts/ --include="*.md" | grep -v "flowcharts/user-experience"

# Search for e-commerce terms that shouldn't be in medical booking
grep -r -i "cart\|product catalog\|inventory" flowcharts/ --include="*.md"
```

### 7. Create Missing Directories for Documentation
```bash
# Create structure for missing docs
mkdir -p docs/{security,technical,integration}

# Create placeholder files
touch docs/security/{HIPAA_COMPLIANCE.md,DATA_ENCRYPTION.md,ACCESS_CONTROL.md}
touch docs/technical/{API_ENDPOINTS.md,DATABASE_SCHEMA.md,DEPLOYMENT_GUIDE.md}
touch docs/integration/{CALENDLY_SETUP.md,MEDIRECORDS_CONFIG.md,STRIPE_INTEGRATION.md}
```

## Recommended Cleanup Order

1. **First**: Run command #4 to create SERVICES_AND_PRICING.md
2. **Second**: Review plant shop content with command #6
3. **Third**: If confirmed, delete plant shop flows with command #1
4. **Fourth**: Find and remove proposed/empty files with command #2
5. **Fifth**: Fix any remaining pricing issues found with command #3
6. **Last**: Create documentation structure with command #7

## Safety Check Before Major Deletions

```bash
# Create a backup before deleting
tar -czf botanical_backup_$(date +%Y%m%d).tar.gz flowcharts/

# Or use git to ensure you can recover
git status  # Make sure everything is committed first
```

## After Cleanup Verification

```bash
# Verify no old pricing remains
grep -r "\$89\|\$69" flowcharts/ --include="*.md" | grep -v "79"

# Verify structure is clean
tree flowcharts -d

# Check for any remaining plant references
grep -r -i "plant" flowcharts/ --include="*.md"
```
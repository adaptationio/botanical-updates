# Visual Cleanup Guide - What to Delete vs Keep

## 🗑️ DELETE These Files (Plant Shop Content)

### ❌ User Experience Directory - ALL FILES
```
flowcharts/user-experience/
├── current/user-flow.md         ❌ DELETE (Browse Plants, Shopping Cart)
├── new-update/user-flow.md      ❌ DELETE (AI Plant Recommendations)  
└── proposed/user-flow.md        ❌ DELETE (AR/VR Plant Preview)
```

**Why Delete?** These are for an e-commerce plant shop, NOT medical booking!

Example of WRONG content:
```mermaid
flowchart TD
    Start --> HomePage[Landing Page]
    HomePage --> Browse[Browse Plants] ❌
    Browse --> PlantDetail[Plant Details] ❌
    PlantDetail --> Cart[Shopping Cart] ❌
```

## 🗑️ DELETE These Files (Unrealistic Sci-Fi)

### ❌ All "Proposed" Directories
```
flowcharts/
├── admin/proposed/admin-flow.md      ❌ DELETE (VR meetings, 90% automation)
├── booking/proposed/                 ❌ DELETE (empty placeholder)
├── tech/proposed/tech-flow.md        ❌ DELETE (quantum computing, blockchain)
└── user-experience/proposed/         ❌ DELETE (also plant content)
```

Example of UNREALISTIC content to remove:
- "Quantum-ready algorithms"
- "Blockchain patient records"
- "AI negotiates with suppliers"
- "Holographic consultations"
- "Neural interface booking"

## ✅ KEEP These Files (Medical Booking - Excellent!)

### ✅ Booking Flows - ALL EXCELLENT
```
flowcharts/booking/
├── current/
│   ├── patient-booking-flow.md      ✅ KEEP (just fix pricing)
│   └── comprehensive-booking-flow.md ✅ KEEP (needs pricing check)
├── new-update/
│   ├── patient-booking-flow.md      ✅ KEEP (excellent!)
│   └── comprehensive-booking-flow.md ✅ KEEP (excellent!)
└── analysis/
    └── improvements-summary.md       ✅ KEEP (good analysis)
```

### ✅ Practitioner Workflows - KEEP ALL
```
flowcharts/
├── doctor/
│   ├── current/appointment-flow.md   ✅ KEEP
│   └── new-update/appointment-flow.md ✅ KEEP
├── consultant/
│   └── new-update/triage-flow.md     ✅ KEEP
└── admin/
    ├── current/booking-admin-flow.md  ✅ KEEP (check pricing)
    └── new-update/booking-admin-flow.md ✅ KEEP
```

### ✅ Technical Architecture - KEEP ALL
```
flowcharts/tech/
├── current/
│   ├── booking-tech-flow.md          ✅ KEEP
│   └── tech-flow.md                  ✅ KEEP
└── new-update/
    ├── booking-tech-flow.md          ✅ KEEP
    └── tech-flow.md                  ✅ KEEP
```

### ✅ Variations - KEEP (Rename for Clarity)
```
flowcharts/variations/
├── coming-soon/                      ✅ KEEP (rename to "option-1-gradual")
├── fully-operational/                ✅ KEEP (rename to "option-2-complete")
└── service-funnels/                  ✅ KEEP (good marketing examples)
```

## 🔧 FIX These Issues in Kept Files

### 1. Pricing Updates Needed
Look for these OLD prices and update:
- ❌ $89 → ✅ $119 (Alternative Medicine Initial)
- ❌ $69 → ✅ $79 (Alternative Medicine Follow-up)
- ❌ $69 → ✅ $195 (GAPS Initial) 
- ❌ $69 → ✅ $79 (GAPS Follow-up)

### 2. Duration Updates Needed
- ❌ 30 min → ✅ 15 min (Alt Med Initial)
- ❌ 30 min → ✅ 10 min (Alt Med Follow-up)
- ❌ 45 min → ✅ 60 min (GAPS Initial)
- ❌ 45 min → ✅ 15 min (GAPS Follow-up)

### 3. Practitioner Count
- ❌ "5 practitioners" → ✅ "6 practitioners (including Dr. Shivani)"
- ❌ "7 practitioners" → ✅ "8 practitioners when fully operational"

## 📁 Final Clean Structure

After cleanup, you should have:
```
botanical_updates/
├── SERVICES_AND_PRICING.md          # NEW - Single source of truth
├── REVIEW_SUMMARY.md                # KEEP - Review findings
├── CLEANUP_PLAN.md                  # KEEP - This cleanup guide
├── README.md                        # KEEP - Update after cleanup
├── flowcharts/
│   ├── booking/                     # KEEP ALL (fix pricing)
│   ├── doctor/                      # KEEP ALL
│   ├── consultant/                  # KEEP ALL
│   ├── admin/                       # KEEP most (delete proposed)
│   ├── tech/                        # KEEP most (delete proposed)
│   └── future-options/              # RENAME from variations
└── docs/                            # CREATE NEW
    ├── security/                    # Needed for healthcare
    ├── technical/                   # API and deployment
    └── integration/                 # Third-party setup
```

## Decision Matrix

| Content Type | Action | Reason |
|--------------|--------|--------|
| Plant shop flows | DELETE | Wrong business entirely |
| Medical booking flows | KEEP | Core documentation |
| Sci-fi proposed features | DELETE | Unrealistic, confusing |
| Current pricing $89/$69 | UPDATE | Wrong prices |
| Correct pricing $119/$79 | KEEP | Accurate |
| Empty placeholder files | DELETE | No value |
| Technical architecture | KEEP | Well documented |
| Service variations | KEEP | Good options |

## Quick Visual Test

After cleanup, search for these terms should return ZERO results:
- 🌱 "plant", "botanical", "garden"
- 🛒 "shopping cart", "product catalog"  
- 🚀 "quantum", "blockchain", "VR meeting"
- 💰 "$89", "$69" (except in follow-up $79)

These terms SHOULD appear:
- 🏥 "medical", "consultation", "practitioner"
- 💵 "$119", "$79", "$195"
- ⏱️ "15 min", "10 min", "60 min", "20 min"
- 👥 "6 practitioners", "consultant", "triage"
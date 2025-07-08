# Botanical Updates - Comprehensive Flow Charts

> **📋 Note**: A comprehensive review has been completed. See [REVIEW_SUMMARY.md](./REVIEW_SUMMARY.md) for findings and action items.
> 
> **🧹 Cleanup Required**: See [CLEANUP_PLAN.md](./CLEANUP_PLAN.md) for detailed cleanup instructions and [CLEANUP_VISUAL_GUIDE.md](./CLEANUP_VISUAL_GUIDE.md) for what to delete vs keep.

## Project Overview

This project contains comprehensive flow charts documenting the Botaniqal medical booking system architecture, including patient flows, practitioner workflows, technical architecture, and service expansion options.

## Directory Structure

```
botanical_updates/
├── SERVICES_AND_PRICING.md     # Official pricing and service reference
├── flowcharts/
│   ├── 1-current-system/       # 📍 WHAT EXISTS TODAY
│   │   ├── admin/              # Current admin workflows
│   │   ├── doctor/             # Dr. Dia's current flow
│   │   ├── tech/               # Current technical architecture
│   │   ├── booking/            # Current booking process
│   │   └── README.md           # Guide to current system
│   │
│   ├── 2-new-update/           # 🚀 WHAT WE'RE BUILDING
│   │   ├── admin/              # Enhanced admin features
│   │   ├── doctor/             # Multi-practitioner flow
│   │   ├── consultant/         # NEW: Triage workflow
│   │   ├── tech/               # New architecture
│   │   ├── booking/            # New booking with triage
│   │   └── README.md           # Guide to new system
│   │
│   ├── 3-future-options/       # 💡 FUTURE POSSIBILITIES
│   │   ├── coming-soon/        # Phased service expansion
│   │   ├── fully-operational/  # All services active
│   │   ├── service-funnels/    # Marketing strategies
│   │   └── README.md           # Guide to variations
│   │
│   └── _analysis/              # 📊 COMPARISONS & REPORTS
│       ├── improvements-summary.md
│       ├── consistency-check-report.md
│       ├── comparison-matrix.md
│       └── README.md
│
└── docs/
    ├── IMPLEMENTATION_CHANGES.md  # What needs to be built
    ├── FLOW_COMPARISON.md        # Current vs New comparison
    ├── security/                 # HIPAA compliance docs
    └── technical/                # API and deployment guides
```

## Flow Chart Navigation

### 🔍 Quick Navigation

1. **[Current System](./flowcharts/1-current-system/)** - What exists today (single practitioner, direct booking)
2. **[New Update](./flowcharts/2-new-update/)** - What we're building (multi-practitioner, free triage)
3. **[Future Options](./flowcharts/3-future-options/)** - Potential expansions (new services)
4. **[Analysis](./flowcharts/_analysis/)** - Comparisons and improvements

### 🔧 Admin Flows

#### Current System
- [Admin Flow](./flowcharts/1-current-system/admin/admin-flow.md) - Basic CRUD operations
- [Booking Admin Flow](./flowcharts/1-current-system/admin/booking-admin-flow.md) - Manual calendar sync

#### New Update  
- [Admin Flow](./flowcharts/2-new-update/admin/admin-flow.md) - Role-based access, analytics
- [Booking Admin Flow](./flowcharts/2-new-update/admin/booking-admin-flow.md) - Multi-practitioner management


### 👨‍⚕️ Doctor Workflow

#### Current System
- [Appointment Flow](./flowcharts/1-current-system/doctor/appointment-flow.md) - Dr. Dia's consultation process

#### New Update
- [Appointment Flow](./flowcharts/2-new-update/doctor/appointment-flow.md) - Multi-practitioner collaboration

### 🏥 Consultant Workflow

#### New Update (NEW ROLE)
- [Triage Flow](./flowcharts/2-new-update/consultant/triage-flow.md) - Free consultation and assessment

### 💻 Technical Architecture

#### Current System
- [Tech Flow](./flowcharts/1-current-system/tech/tech-flow.md) - Monolithic architecture
- [Booking Tech Flow](./flowcharts/1-current-system/tech/booking-tech-flow.md) - Calendly + MediRecords

#### New Update
- [Tech Flow](./flowcharts/2-new-update/tech/tech-flow.md) - Microservices architecture
- [Booking Tech Flow](./flowcharts/2-new-update/tech/booking-tech-flow.md) - Multi-calendar orchestration

### 📅 Booking Flows

#### Current System
- [Comprehensive Flow](./flowcharts/1-current-system/booking/comprehensive-booking-flow.md) - End-to-end system
- [Patient Flow](./flowcharts/1-current-system/booking/patient-booking-flow.md) - Patient journey

#### New Update
- [Comprehensive Flow](./flowcharts/2-new-update/booking/comprehensive-booking-flow.md) - 5-stakeholder system
- [Patient Flow](./flowcharts/2-new-update/booking/patient-booking-flow.md) - Triage-first journey

#### Analysis
- [Improvements Summary](./flowcharts/_analysis/improvements-summary.md) - Pain points and solutions

## Key Improvements in New Update (Ready for Deployment)

### Patient Experience
- Free initial consultation with triage
- Multiple practitioner options
- Phone booking support
- Combined calendar for easy follow-ups

### Admin Efficiency
- Multi-calendar synchronization
- Automated form processing
- Phone booking tracking
- Enhanced reporting

### Technical Architecture
- Multiple Calendly integrations
- Dynamic form support
- Webhook automation
- Scalable microservices

## Viewing the Flowcharts

All flowcharts are created using Mermaid markdown syntax. To view them:

1. **GitHub**: GitHub automatically renders Mermaid diagrams in markdown files
2. **VS Code**: Install the "Markdown Preview Mermaid Support" extension
3. **Online**: Use [Mermaid Live Editor](https://mermaid.live)
4. **Other Tools**: Most modern markdown viewers support Mermaid

## Current Pricing Structure

### Active Services (New Update)
- **Free Initial Consultation**: 20 minutes with consultant (triage assessment)
- **Alternative Medicine**: 
  - Initial: $119 (15 minutes) - Telehealth
  - Follow-up: $79 (10 minutes) - Telehealth
  - In-Person (Melbourne): $119/$79 (20/15 minutes)
- **GAPS Diet Coaching**:
  - Initial: $195 (60 minutes)
  - Follow-up: $79 (15 minutes)

## Service Expansion Options

### 🚀 Future Service Possibilities
Explore potential expansions beyond the current Alternative Medicine and GAPS Coaching:

#### [Service Overview](./flowcharts/_analysis/service-expansion-overview.md)
- Weight Loss Program (Ready to launch)
- Counseling Services (Planned)
- Equine Therapy (Planned)

### Implementation Variations

#### Option 1: [Coming Soon](./flowcharts/3-future-options/coming-soon/)
- Weight loss launches first, others on waitlist
- Lower risk, gradual expansion
- 4-6 weeks implementation

#### Option 2: [Fully Operational](./flowcharts/3-future-options/fully-operational/)
- All 5 services active simultaneously
- 7+ practitioners total
- 8-12 weeks implementation

#### [Service Funnels](./flowcharts/3-future-options/service-funnels/)
- Marketing strategies for each service
- Conversion optimization approaches

### Analysis Documents
- [Comparison Matrix](./flowcharts/_analysis/comparison-matrix.md) - Compare implementation options
- [Improvements Summary](./flowcharts/_analysis/improvements-summary.md) - Current vs new analysis

## Implementation Priority

1. **Phase 1**: Deploy "New Update" features
   - Free consultations with triage
   - Multi-practitioner support
   - Enhanced booking system

2. **Phase 2**: Service Expansion (Choose Variation)
   - Launch weight loss program
   - Implement waitlist or full launch
   - Service-specific funnels

3. **Phase 3**: Future Enhancements
   - Advanced analytics
   - Integrated care packages
   - Automated care planning

## Using These Flowcharts

These diagrams serve multiple purposes:
- **Development Planning**: Guide implementation priorities
- **Stakeholder Communication**: Visual representation of improvements
- **Architecture Documentation**: Technical reference
- **Progress Tracking**: Compare current vs planned state

## Next Steps

1. Review all flowcharts with stakeholders
2. Prioritize features for implementation
3. Create detailed technical specifications
4. Begin phased implementation
5. Track progress against these blueprints
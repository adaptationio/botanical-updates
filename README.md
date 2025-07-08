# Botanical Updates - Comprehensive Flow Charts

> **📋 Note**: A comprehensive review has been completed. See [REVIEW_SUMMARY.md](./REVIEW_SUMMARY.md) for findings and action items.
> 
> **🧹 Cleanup Required**: See [CLEANUP_PLAN.md](./CLEANUP_PLAN.md) for detailed cleanup instructions and [CLEANUP_VISUAL_GUIDE.md](./CLEANUP_VISUAL_GUIDE.md) for what to delete vs keep.

## Project Overview

This project contains comprehensive flow charts documenting the Botaniqal medical booking system architecture, including patient flows, practitioner workflows, technical architecture, and service expansion options.

## Directory Structure

```
botanical_updates/
├── SERVICES_AND_PRICING.md # Official pricing and service reference
├── REVIEW_SUMMARY.md       # Comprehensive review findings
├── flowcharts/
│   ├── admin/
│   │   ├── current/        # Current admin flow
│   │   └── new-update/     # Ready for deployment
│   ├── doctor/
│   │   ├── current/        # Doctor appointment workflow
│   │   └── new-update/     # Enhanced multi-practitioner flow
│   ├── consultant/
│   │   └── new-update/     # Consultant triage workflow
│   ├── tech/
│   │   ├── current/        # Current architecture
│   │   └── new-update/     # Ready for deployment
│   ├── booking/
│   │   ├── current/        # Current booking flow
│   │   ├── new-update/     # Ready for deployment
│   │   └── analysis/       # Pain points and improvements
│   └── variations/
│       ├── coming-soon/    # Weight loss active, others waitlist
│       ├── fully-operational/ # All 5 services active
│       └── service-funnels/   # Marketing funnel designs
└── docs/
    ├── security/           # HIPAA compliance and security
    ├── technical/          # API and deployment docs
    └── integration/        # Third-party integrations
```

## Flow Chart Navigation

### 🔧 Admin Flows

#### [Current Admin Flow](./flowcharts/admin/current/admin-flow.md)
- Basic CRUD operations
- Manual processes
- Limited reporting

#### [Current Booking Admin Flow](./flowcharts/admin/current/booking-admin-flow.md)
- Manual availability sync (MediRecords ↔ Calendly)
- Email-based booking monitoring
- Manual intake form processing
- LLM clinical notes generation

#### [New Update Booking Admin Flow](./flowcharts/admin/new-update/booking-admin-flow.md)
- Multi-practitioner calendar management
- Enhanced form processing
- Phone booking tracking

#### [New Update Admin Flow](./flowcharts/admin/new-update/admin-flow.md)
- Role-based access control
- Bulk operations
- Advanced analytics
- Marketing automation


### 👨‍⚕️ Doctor Workflow

#### [Current Doctor Appointment Flow](./flowcharts/doctor/current/appointment-flow.md)
- Pre-appointment preparation
- Patient consultation process
- SafeScript integration
- eRx prescription system
- Follow-up coordination

#### [New Update Doctor Flow](./flowcharts/doctor/new-update/appointment-flow.md)
- Triage handover integration
- Multi-practitioner referrals
- Team collaboration features
- Enhanced documentation

### 🏥 Consultant Workflow

#### [New Update Consultant Triage Flow](./flowcharts/consultant/new-update/triage-flow.md)
- Free initial consultations
- Dynamic triage assessment
- Practitioner matching
- Phone booking process with payment collection
- Handover to healthcare team

### 💻 Technical Architecture Flows

#### [Current Tech Flow](./flowcharts/tech/current/tech-flow.md)
- Monolithic PHP architecture
- MySQL database
- Basic caching
- Manual scaling

#### [Current Booking Tech Flow](./flowcharts/tech/current/booking-tech-flow.md)
- Calendly + Office 365 integration
- AWS Lambda serverless processing
- MediRecords EHR integration
- Multi-step webhook architecture

#### [New Update Booking Tech Flow](./flowcharts/tech/new-update/booking-tech-flow.md)
- *Ready for deployment - awaiting specifications*

#### [New Update Tech Flow](./flowcharts/tech/new-update/tech-flow.md)
- Microservices architecture
- Kubernetes orchestration
- PostgreSQL with replicas
- CI/CD automation

#### [Proposed Tech Flow](./flowcharts/tech/proposed/tech-flow.md)
- Edge computing with AI
- Quantum-ready algorithms
- Blockchain integration
- Zero-latency global network

### 📅 Booking Flow (Botaniqal Specific)

#### [Current Comprehensive Booking System Flow](./flowcharts/booking/current/comprehensive-booking-flow.md)
- Complete view across all stakeholders (current system)
- Shows integration points between systems
- Identifies manual processes and pain points
- Maps full journey from booking to completion

#### [New Update Comprehensive Booking System Flow](./flowcharts/booking/new-update/comprehensive-booking-flow.md)
- Enhanced multi-stakeholder view with 5 user types
- Free initial consultation with consultant triage
- Multi-practitioner booking system
- Phone booking and payment integration
- Dynamic forms and improved handover process

#### [Current Patient Booking Flow](./flowcharts/booking/current/patient-booking-flow.md)
- Two-step process (booking + intake)
- Single provider (Dr. Dia)
- Basic eligibility screening
- Calendly integration

#### [New Update Patient Booking Flow](./flowcharts/booking/new-update/patient-booking-flow.md)
- Free initial consultation with consultant
- Triage to appropriate practitioner
- Phone booking after assessment
- Combined calendar for follow-ups
- Multiple practitioners (2 telehealth doctors, Dr Shivani in-person, nurse, GAPS coach, consultant)

#### [Booking Analysis](./flowcharts/booking/analysis/improvements-summary.md)
- Pain points identification
- Implementation roadmap
- Expected outcomes
- Technical requirements

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

## Service Expansion Variations

### 🚀 New Service Offerings
The team is evaluating expansion into additional services beyond Alternative Medicine and GAPS Coaching:

#### [Service Expansion Overview](./flowcharts/variations/service-expansion-overview.md)
- Weight Loss Program (Ready to launch)
- Counseling Services (Planned)
- Equine Therapy (Planned)
- Service-specific funnels and pricing

#### Variation 1: Coming Soon
- **[Patient Booking Flow](./flowcharts/variations/coming-soon/patient-booking-flow.md)**: Weight loss active, others on waitlist
- **[Technical Architecture](./flowcharts/variations/coming-soon/technical-architecture.md)**: Multi-service support
- **[Admin Workflow](./flowcharts/variations/coming-soon/admin-workflow.md)**: Service-level management
- **Benefits**: Lower risk, gradual expansion, market testing
- **Timeline**: 4-6 weeks implementation

#### Variation 2: Fully Operational
- **[Patient Booking Flow](./flowcharts/variations/fully-operational/patient-booking-flow.md)**: All 5 services active
- **Practitioners**: 7 total (adding counselor and equine therapist)
- **Appointment Types**: 15+ variations
- **Benefits**: Complete offering, integrated care, maximum revenue
- **Timeline**: 8-12 weeks implementation

#### [Comparison Matrix](./flowcharts/variations/comparison-matrix.md)
- Side-by-side comparison of variations
- Revenue projections
- Risk assessment
- Implementation phases
- Decision framework

### Marketing Funnels
#### [Weight Loss Funnel Example](./flowcharts/variations/service-funnels/weight-loss-funnel.md)
- Landing page structure
- Conversion optimization
- Trust building elements
- Email nurture sequences

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
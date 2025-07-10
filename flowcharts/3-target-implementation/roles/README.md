# Role-Based Flows - Target Implementation (Archive)

## Overview
This directory contains the original role-specific flowcharts for the target implementation. These have now been distributed into the specific implementation variations (Coming Soon and Fully Operational) for better organization. Each flow is tailored to show how different roles interact with the expanded multi-service system in our target implementation.

## 🎭 Role-Based Documentation

### 1. [Admin Dashboard Flow](./admin-dashboard-flow.md)
**For: Administrators, Managers, Business Owners**

Key Features:
- Multi-service dashboard overview
- Active vs waitlisted service management
- Service-specific analytics and metrics
- Practitioner utilization tracking
- Waitlist conversion tools
- Revenue analysis by service line

### 2. [Consultant Triage Flow](./consultant-triage-flow.md)
**For: Consultants, GAPS Coach (performing consultations)**

Key Features:
- Multi-service assessment process
- Active service routing
- Waitlist capture for coming soon services
- Dynamic form selection
- Service combination recommendations
- Mixed active/waitlist booking

### 3. [Doctor Appointment Flow](./doctor-appointment-flow.md)
**For: Doctors, Nurse Practitioners**

Key Features:
- Multi-service appointment management
- Cross-service referral capabilities
- Integrated care planning
- Service-specific protocols
- Waitlist referral process
- Care team coordination

### 4. [Technical Architecture Flow](./tech-architecture-flow.md)
**For: Developers, System Architects, DevOps**

Key Features:
- Microservices architecture
- Multi-calendar integration
- Waitlist infrastructure
- Service configuration management
- Event-driven system design
- Scalability considerations

## 🔄 Service Status Overview

### Active Services (Fully Functional)
1. **Alternative Medicine** - $119/$79, 15-20 min slots
2. **GAPS Diet Coaching** - $195/$79, 60/15 min slots
3. **Weight Loss Program** - Pricing TBD, launching soon

### Waitlisted Services (Coming Soon)
4. **Counseling Services** - Online mental health support
5. **Equine Therapy** - Alternative therapy with horses

## 📊 Key Differences from Current System

### System Complexity
| Aspect | Ready-to-Deploy (2-new-update) | Target Implementation |
|--------|----------------------|----------------|
| Services | 2 (Alt Med, GAPS) | 5 (+ Weight, Counseling, Equine) |
| Practitioners | 6 | 8+ |
| Booking Paths | Simple triage | Multi-service coordination |
| Waitlist | None | Integrated capture & nurture |
| Analytics | Basic | Service-level insights |

### Role Evolution

**Admin Role:**
- From: Single service monitoring
- To: Multi-service portfolio management with waitlist analytics

**Consultant Role:**
- From: Simple triage to 2 services
- To: Complex assessment across 5 services with waitlist handling

**Doctor Role:**
- From: Alternative medicine focus
- To: Multi-service delivery with cross-referral capabilities

**Tech Role:**
- From: Basic integration
- To: Scalable microservices with dynamic service configuration

## 🚀 Implementation Approach

### Phase 1: Coming Soon Variation
- Enable Weight Loss alongside existing services
- Implement waitlist capture for Counseling & Equine
- Basic multi-service support

### Phase 2: Fully Operational
- Activate all 5 services
- Full practitioner team (8+)
- Advanced analytics and automation
- Integrated care pathways

## 💡 Navigation Tips

### For Business Planning
1. Start with [Admin Dashboard Flow](./admin-dashboard-flow.md)
2. Review service management capabilities
3. Understand waitlist conversion strategy

### For Operations Training
1. Review [Consultant Triage Flow](./consultant-triage-flow.md)
2. Study multi-service assessment
3. Practice waitlist capture scenarios

### For Clinical Teams
1. Focus on [Doctor Appointment Flow](./doctor-appointment-flow.md)
2. Understand cross-service referrals
3. Review integrated care planning

### For Technical Implementation
1. Study [Technical Architecture Flow](./tech-architecture-flow.md)
2. Plan microservices deployment
3. Design waitlist infrastructure

## 📈 Success Metrics

### Key Performance Indicators
- **Service Utilization**: Track across all 5 services
- **Waitlist Conversion**: Measure interest to booking rate
- **Cross-Service Referrals**: Monitor integrated care adoption
- **Practitioner Efficiency**: Multi-service scheduling optimization

### Quality Measures
- Patient satisfaction by service
- Care coordination effectiveness
- Waitlist engagement rates
- System performance metrics

---

*These role-based flows represent the target implementation we are actively designing. They build upon the ready-to-deploy system in [2-new-update](../../2-new-update/) and detail the full multi-service platform we're building toward. The flows have been copied into both [coming-soon](../coming-soon/) and [fully-operational](../fully-operational/) variations for implementation planning.*
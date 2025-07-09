# Fully Operational Flowcharts - Navigation Guide

## 📋 Overview
The fully operational variation represents the complete vision with all 5 services active and 8 practitioners. Due to the complexity, the flows have been broken into focused, digestible documents.

## 🗺️ Document Map

### Core Documents

1. **[patient-booking-flow.md](./patient-booking-flow.md)** ⭐
   - Main entry point with links to all sub-flows
   - System overview and key features
   - Complete service and practitioner listings

2. **[patient-booking-overview.md](./patient-booking-overview.md)** 
   - Simplified 20-node master flowchart
   - High-level system view
   - Quick reference tables

### Detailed Flow Documents

3. **[new-patient-journey.md](./new-patient-journey.md)**
   - First-time patient experience
   - Free consultation process
   - Dynamic intake forms
   - Service matching logic

4. **[returning-patient-flow.md](./returning-patient-flow.md)**
   - Follow-up booking process
   - Combined calendar views
   - Self-service booking
   - Online payment flow

5. **[service-triage-process.md](./service-triage-process.md)**
   - Consultant assessment logic
   - Service matching criteria
   - Practitioner assignment
   - Multi-service planning

6. **[phone-booking-process.md](./phone-booking-process.md)**
   - Post-triage booking steps
   - Payment processing
   - Multi-service coordination
   - Confirmation process

### Legacy Documents

7. **[admin-dashboard-flow.md](./admin-dashboard-flow.md)**
   - Administrative functions
   - Reporting capabilities
   - System management

8. **[technical-architecture.md](./technical-architecture.md)**
   - System integration points
   - Technical requirements
   - Architecture overview

## 🎯 Where to Start

### For Business Stakeholders
1. Start with **[patient-booking-overview.md](./patient-booking-overview.md)**
2. Review service portfolio in **[patient-booking-flow.md](./patient-booking-flow.md)**
3. Understand patient experience via journey documents

### For Technical Teams
1. Review **[technical-architecture.md](./technical-architecture.md)**
2. Study detailed flows for integration points
3. Check **[phone-booking-process.md](./phone-booking-process.md)** for payment integration

### For Operations Staff
1. Focus on **[service-triage-process.md](./service-triage-process.md)**
2. Understand **[phone-booking-process.md](./phone-booking-process.md)**
3. Review admin functions in dashboard flow

## 📊 Quick Stats

- **Services**: 5 (Alt Med, GAPS, Weight Loss, Counseling, Equine)
- **Practitioners**: 8 (including consultant role)
- **Booking Paths**: 2 (New patient triage, Returning self-service)
- **Appointment Types**: 15+
- **Payment Options**: Multiple (CC, HSA/FSA, Payment plans)

## 🔄 System Complexity

The fully operational system is broken into multiple documents because:
- Single diagram had 180+ lines with 100+ nodes
- Multiple overlapping pathways created visual confusion
- Different audiences need different levels of detail
- Maintenance is easier with modular documentation

## 💡 Key Innovations

1. **Dynamic Intake Forms** - Consultant selects appropriate form during call
2. **Combined Calendars** - Shows only relevant practitioners per service
3. **Multi-Service Support** - Integrated care planning and booking
4. **Smart Triage** - Matches patients to optimal practitioner
5. **Flexible Pathways** - Phone booking for new, online for returning

---

*These flowcharts represent the complete vision for the booking system. For current implementation status, see the [2-new-update](../../2-new-update/) variation.*
# Alternative Medicine Funnel

## Overview
This flowchart maps the patient journey from discovering alternative medicine options to booking their consultation.

## Main Funnel Flow

```mermaid
graph TD
    Start([Traffic Sources]) --> Landing[Alternative Medicine<br/>Landing Page]
    
    Landing --> Hero[Hero: Natural Healthcare<br/>That Works]
    Hero --> Choice[Choose Your Preference<br/>✓ Telehealth Nationwide<br/>✓ In-Person Melbourne]
    
    Choice --> Benefits[Key Benefits<br/>✓ Holistic Approach<br/>✓ Natural Treatments<br/>✓ Personalized Care<br/>✓ Evidence-Based]
    
    Benefits --> Practitioners[Meet Our Practitioners<br/>Dr. Shivani & Team<br/>Combined 50+ Years Experience]
    
    Practitioners --> Conditions[Conditions We Treat<br/>Chronic Pain • Digestive Issues<br/>Hormonal Balance • Stress<br/>Immune Support • More]
    
    Conditions --> Pricing[Transparent Pricing<br/>Initial: $119 (15-20 min)<br/>Follow-up: $79 (10-15 min)<br/>FREE 20-min Consultation]
    
    Pricing --> CTA1{Primary Action}
    CTA1 -->|Book Free Consult| Booking[Enter Booking System]
    CTA1 -->|View Services| Services[Service Details]
    CTA1 -->|See Reviews| Reviews[Patient Reviews]
    
    Services --> Modalities[Treatment Options<br/>Herbal Medicine • Acupuncture<br/>Nutritional Therapy • Lifestyle]
    Modalities --> CTA2[Book Consultation]
    
    Reviews --> Testimonials[Success Stories<br/>Video Testimonials<br/>Google Reviews: 4.8★]
    Testimonials --> CTA3[Start Your Journey]
    
    CTA2 --> Booking
    CTA3 --> Booking
    
    Booking --> Success([Free Consultation<br/>Scheduled])
    
    CTA1 -->|Not Ready| Resources[Free Resources<br/>Alternative Medicine Guide]
    Resources --> EmailCapture[Email Signup]
    EmailCapture --> Nurture[Educational<br/>Email Series]
    
    classDef primary fill:#e8f5e9,stroke:#4caf50,stroke-width:3px
    classDef choice fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px
    classDef cta fill:#fff8e1,stroke:#f57c00,stroke-width:3px
    
    class Landing,Hero,Benefits primary
    class Choice,Practitioners choice
    class CTA1,CTA2,CTA3,Pricing cta
```

## Conversion Elements

### 1. Landing Page Structure
- **Hero Message**: Emphasize natural, effective healthcare
- **Choice Architecture**: Clear telehealth vs in-person options
- **Practitioner Profiles**: Build trust with credentials
- **Condition List**: Help patients self-identify

### 2. Traffic Sources
- Google Search (alternative medicine + location)
- Health directories and listings
- Referrals from GPs
- Social media wellness communities
- Content marketing (health articles)

### 3. Trust Building Elements
| Element | Purpose |
|---------|---------|
| Practitioner photos & bios | Personal connection |
| Years of experience | Credibility |
| Treatment modalities | Professional expertise |
| Patient testimonials | Social proof |
| Google reviews integration | Third-party validation |

### 4. Service Differentiators
- **Flexibility**: Both telehealth and in-person options
- **Accessibility**: Dr. Shivani available for Melbourne patients
- **Integration**: Works alongside conventional medicine
- **Personalization**: Tailored treatment plans
- **Free Consultation**: Risk-free first step

### 5. Email Nurture Campaign
**Lead Magnet**: "Introduction to Alternative Medicine" Guide
1. **Day 0**: Welcome & guide delivery
2. **Day 3**: What is alternative medicine?
3. **Day 5**: Success story: chronic pain relief
4. **Day 7**: Common misconceptions addressed
5. **Day 10**: Practitioner spotlight
6. **Day 14**: Special offer for new patients
7. **Day 21**: How to prepare for your consultation
8. **Ongoing**: Monthly wellness tips

### 6. Objection Handling
| Objection | Response |
|-----------|----------|
| "Is it scientific?" | Evidence-based natural therapies |
| "Will it work for me?" | Free consultation to assess suitability |
| "Is it covered by insurance?" | Private health rebates available |
| "How is it different from my GP?" | Complementary approach, more time per visit |

## Unique Selling Points

### Dr. Shivani's Dual Availability
- **Telehealth**: Convenient nationwide access
- **In-Person**: Melbourne clinic for hands-on treatments
- **Flexibility**: Switch between modes as needed
- **Continuity**: Same practitioner regardless of mode

### Holistic Integration
- Works with existing medical treatments
- Addresses root causes, not just symptoms
- Lifestyle and dietary guidance included
- Long-term wellness focus

## Tracking Metrics
- Telehealth vs in-person preference
- Condition-based conversion rates
- Practitioner preference impact
- Free consultation to paid conversion
- Average patient lifetime value

## Integration with Booking System
- Service: "Alternative Medicine" pre-selected
- Practitioner choice available after triage
- Location preference captured early
- Consultant notes patient's specific needs
- Smooth handoff to chosen practitioner
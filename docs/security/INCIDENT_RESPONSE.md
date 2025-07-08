# Incident Response Plan

## Overview

This document outlines the incident response procedures for the Botaniqal medical booking system, with specific focus on security breaches involving Protected Health Information (PHI) and compliance with HIPAA breach notification requirements.

## Incident Classification

### Severity Levels

| Level | Description | Response Time | Examples |
|-------|-------------|---------------|----------|
| **Critical (P1)** | Active data breach, system compromise, PHI exposure | Immediate (< 15 min) | Ransomware, database breach, mass PHI exposure |
| **High (P2)** | Potential data breach, security vulnerability | 1 hour | Suspicious access patterns, authentication bypass |
| **Medium (P3)** | Security concern, compliance issue | 4 hours | Failed security scan, policy violation |
| **Low (P4)** | Minor security issue | 24 hours | Single failed login, minor misconfiguration |

### Incident Types

1. **Data Breach**
   - Unauthorized PHI access
   - Data exfiltration
   - Lost/stolen devices
   - Accidental disclosure

2. **System Compromise**
   - Malware infection
   - Unauthorized system access
   - Account takeover
   - Insider threat

3. **Service Disruption**
   - DDoS attack
   - Ransomware
   - System failure
   - Third-party service outage

4. **Compliance Violation**
   - Unauthorized data sharing
   - Improper data handling
   - Access control failure
   - Audit trail gaps

## Response Team Structure

### Core Incident Response Team

```yaml
Incident Commander:
  Primary: Chief Technology Officer
  Backup: Head of Engineering
  Responsibilities: Overall incident coordination

Security Lead:
  Primary: Security Engineer
  Backup: Senior DevOps Engineer
  Responsibilities: Technical investigation and containment

Compliance Officer:
  Primary: HIPAA Compliance Officer
  Backup: Legal Counsel
  Responsibilities: Regulatory compliance and reporting

Communications Lead:
  Primary: VP of Customer Success
  Backup: Marketing Director
  Responsibilities: Internal/external communications

Operations Lead:
  Primary: DevOps Manager
  Backup: Senior SRE
  Responsibilities: System recovery and monitoring
```

### Extended Team (as needed)

- Legal Counsel
- External Security Consultants
- Law Enforcement Liaison
- Public Relations
- Customer Support Manager

## Response Procedures

### Phase 1: Detection and Analysis (0-30 minutes)

#### 1.1 Initial Detection
```javascript
// Automated detection triggers
const DETECTION_RULES = {
  bruteForce: {
    threshold: 10,
    window: '5 minutes',
    action: 'alert_security'
  },
  dataExfiltration: {
    threshold: '100MB',
    unusual_hours: true,
    action: 'block_and_alert'
  },
  privilegedAccess: {
    after_hours: true,
    unusual_location: true,
    action: 'require_mfa_and_alert'
  }
};
```

#### 1.2 Initial Assessment Checklist
- [ ] Confirm incident is real (not false positive)
- [ ] Determine incident type and severity
- [ ] Identify affected systems and data
- [ ] Assess ongoing threat
- [ ] Document initial findings
- [ ] Activate response team if P1/P2

#### 1.3 Evidence Collection
```bash
# Immediate evidence collection script
#!/bin/bash

INCIDENT_ID=$(date +%Y%m%d_%H%M%S)
EVIDENCE_DIR="/secure/incidents/${INCIDENT_ID}"

mkdir -p ${EVIDENCE_DIR}/{logs,snapshots,network}

# Capture system state
ps aux > ${EVIDENCE_DIR}/snapshots/processes.txt
netstat -an > ${EVIDENCE_DIR}/snapshots/connections.txt
w > ${EVIDENCE_DIR}/snapshots/logged_users.txt

# Preserve logs
cp -r /var/log/* ${EVIDENCE_DIR}/logs/
docker logs $(docker ps -q) > ${EVIDENCE_DIR}/logs/docker.log

# Capture network traffic (if authorized)
tcpdump -i any -w ${EVIDENCE_DIR}/network/capture.pcap -c 10000

# Create timeline
echo "Incident detected: $(date)" > ${EVIDENCE_DIR}/timeline.txt
```

### Phase 2: Containment (30 minutes - 2 hours)

#### 2.1 Short-term Containment
```javascript
// Immediate containment actions
async function containIncident(incidentType, affectedResources) {
  switch(incidentType) {
    case 'ACCOUNT_COMPROMISE':
      await disableUserAccount(affectedResources.userId);
      await terminateAllSessions(affectedResources.userId);
      await revokeAllTokens(affectedResources.userId);
      break;
      
    case 'DATA_BREACH':
      await blockIPAddresses(affectedResources.suspiciousIPs);
      await disableAffectedAPIs(affectedResources.endpoints);
      await enableReadOnlyMode();
      break;
      
    case 'MALWARE':
      await isolateAffectedSystems(affectedResources.servers);
      await blockMaliciousConnections(affectedResources.iocs);
      break;
  }
  
  // Log all actions
  await auditLog.logContainmentActions(incidentType, affectedResources);
}
```

#### 2.2 System Isolation Procedures
1. **Network Isolation**
   ```bash
   # Isolate affected systems
   iptables -I INPUT -s <affected_ip> -j DROP
   iptables -I OUTPUT -d <affected_ip> -j DROP
   
   # Update security groups
   aws ec2 modify-instance-attribute --instance-id <instance> --no-source-dest-check
   ```

2. **Account Isolation**
   ```sql
   -- Disable compromised accounts
   UPDATE users 
   SET status = 'suspended',
       suspension_reason = 'Security incident',
       suspended_at = NOW()
   WHERE id IN (<affected_user_ids>);
   
   -- Revoke all sessions
   DELETE FROM sessions WHERE user_id IN (<affected_user_ids>);
   ```

### Phase 3: Eradication (2-24 hours)

#### 3.1 Root Cause Analysis
```javascript
const performRootCauseAnalysis = async (incidentId) => {
  const timeline = await buildIncidentTimeline(incidentId);
  const affectedSystems = await identifyAffectedSystems(incidentId);
  const vulnerabilities = await scanForVulnerabilities(affectedSystems);
  
  return {
    initialVector: determineAttackVector(timeline),
    scope: calculateImpactScope(affectedSystems),
    dataExposed: identifyExposedData(timeline, affectedSystems),
    rootCause: analyzeRootCause(vulnerabilities, timeline)
  };
};
```

#### 3.2 Threat Removal
- Remove malware/backdoors
- Close vulnerabilities
- Update security rules
- Reset compromised credentials
- Patch affected systems

### Phase 4: Recovery (1-7 days)

#### 4.1 System Restoration
```yaml
Recovery Priority:
  1. Authentication services
  2. Database integrity
  3. API functionality
  4. User interface
  5. Third-party integrations
  6. Analytics and reporting
```

#### 4.2 Validation Checklist
- [ ] All systems patched and updated
- [ ] Security configurations verified
- [ ] Monitoring enhanced for affected areas
- [ ] Backup integrity confirmed
- [ ] Access controls reviewed
- [ ] Incident-specific alerts created

### Phase 5: Post-Incident (7-30 days)

#### 5.1 Lessons Learned Meeting
**Agenda Template:**
1. Incident timeline review
2. What went well
3. What needs improvement
4. Action items
5. Policy/procedure updates
6. Training needs

#### 5.2 Report Generation
```markdown
# Incident Report Template

## Executive Summary
- Incident Type: [Data Breach/System Compromise/etc]
- Severity: [P1-P4]
- Duration: [Start time - End time]
- Impact: [Number of affected users/records]
- Root Cause: [Brief description]

## Timeline
[Detailed chronological events]

## Impact Assessment
- Systems Affected: [List]
- Data Exposed: [Type and volume]
- Users Impacted: [Number and categories]
- Business Impact: [Downtime, revenue, reputation]

## Response Actions
[Detailed actions taken during each phase]

## Root Cause Analysis
[Technical details of how incident occurred]

## Remediation
- Immediate fixes: [List]
- Long-term improvements: [List]
- Policy changes: [List]

## Recommendations
[Specific recommendations to prevent recurrence]
```

## HIPAA Breach Notification

### Breach Assessment

```javascript
const assessBreach = async (incident) => {
  // HIPAA breach definition criteria
  const breachCriteria = {
    // 1. Was PHI accessed or disclosed?
    phiInvolved: await checkPHIInvolvement(incident),
    
    // 2. Was it an authorized person?
    unauthorizedAccess: await checkAuthorizationStatus(incident),
    
    // 3. Was risk assessment performed?
    riskAssessment: await performRiskAssessment(incident)
  };
  
  // Four-factor risk assessment
  const riskFactors = {
    // 1. Nature and extent of PHI
    dataTypes: await identifyPHITypes(incident),
    recordCount: await countAffectedRecords(incident),
    
    // 2. Unauthorized person who accessed
    accessorIdentity: await identifyAccessor(incident),
    accessorIntent: await assessIntent(incident),
    
    // 3. Whether PHI was acquired/viewed
    dataAcquired: await checkDataAcquisition(incident),
    
    // 4. Mitigation factors
    mitigationSteps: await documentMitigation(incident)
  };
  
  return {
    isReportableBreach: determineReportability(breachCriteria, riskFactors),
    riskLevel: calculateRiskLevel(riskFactors),
    notificationRequired: determineNotificationRequirements(riskFactors)
  };
};
```

### Notification Timeline

| Notification Type | Timeline | Recipients |
|------------------|----------|------------|
| **Internal** | Immediate | Response team, executives |
| **Individual** | 60 days | Affected patients |
| **HHS** | 60 days | Office for Civil Rights |
| **Media** | 60 days | If >500 individuals |
| **State** | Varies | State health departments |

### Notification Templates

#### Patient Notification
```
Dear [Patient Name],

We are writing to inform you of a recent security incident that may have affected your protected health information.

What Happened:
[Brief description of incident]

Information Involved:
[Types of information potentially affected]

What We Are Doing:
[Steps taken to address the incident]

What You Should Do:
[Recommended actions for the patient]

For More Information:
Contact our privacy office at privacy@botaniqal.com.au or 1800-XXX-XXX

We sincerely apologize for any inconvenience this may cause.

Sincerely,
[Privacy Officer Name]
Botaniqal Privacy Officer
```

## Communication Protocols

### Internal Communications

```yaml
Immediate (0-1 hour):
  - Executive team
  - Legal counsel
  - Response team members

Within 4 hours:
  - All staff (high-level summary)
  - Board of directors (if P1)
  
Within 24 hours:
  - Detailed update to all stakeholders
  - Third-party vendors (if affected)
```

### External Communications

```yaml
Patients:
  - Method: Secure email + postal mail
  - Timeline: Within 60 days
  - Content: Approved by legal

Media (if required):
  - Spokesperson: CEO or designated
  - Key messages: Pre-approved
  - Timeline: Coordinated with legal

Regulators:
  - HHS OCR: Within 60 days
  - State authorities: Per state requirements
  - Include: Incident details, affected count, remediation
```

## Specific Incident Playbooks

### Playbook: Ransomware Attack

```bash
#!/bin/bash
# Ransomware Response Playbook

# 1. Immediate Actions (0-15 min)
- Disconnect affected systems from network
- Activate incident response team
- Begin evidence collection
- Check backup integrity

# 2. Assessment (15-60 min)
- Identify ransomware variant
- Determine infection scope
- Assess data criticality
- Review recovery options

# 3. Containment (1-4 hours)
- Isolate all potentially affected systems
- Block C&C communications
- Preserve forensic evidence
- Implement emergency access procedures

# 4. Decision Point (4-8 hours)
if [[ backup_available && restoration_time < tolerance ]]; then
  proceed_with_restoration
else
  evaluate_negotiation_options
fi

# 5. Recovery (1-7 days)
- Restore from clean backups
- Rebuild affected systems
- Implement additional controls
- Conduct thorough testing
```

### Playbook: Insider Threat

```javascript
const insiderThreatResponse = {
  immediate: {
    disableAccess: ['VPN', 'email', 'applications', 'physical'],
    preserveEvidence: ['emails', 'files', 'logs', 'device'],
    involveHR: true,
    involeLegal: true
  },
  
  investigation: {
    scope: ['access_patterns', 'data_accessed', 'external_transfers'],
    interviews: ['supervisor', 'teammates', 'IT_staff'],
    forensics: ['workstation', 'cloud_storage', 'mobile_devices']
  },
  
  containment: {
    changePasswords: ['shared_accounts', 'service_accounts'],
    reviewAccess: ['similar_roles', 'elevated_privileges'],
    monitorActivity: ['data_movement', 'unusual_patterns']
  }
};
```

## Testing and Maintenance

### Tabletop Exercises

**Quarterly Scenarios:**
1. Q1: Data breach with PHI exposure
2. Q2: Ransomware attack
3. Q3: Insider threat
4. Q4: Third-party compromise

### Plan Updates

- **Monthly**: Contact information
- **Quarterly**: Procedures and playbooks
- **Annually**: Complete plan review
- **After Incident**: Lessons learned integration

## Key Contacts

### Internal Contacts

| Role | Primary | Backup | Contact |
|------|---------|--------|---------|
| Incident Commander | CTO | Head of Eng | [Secure contact] |
| Security Lead | Security Eng | Senior DevOps | [Secure contact] |
| Legal Counsel | General Counsel | External Firm | [Secure contact] |
| PR Lead | VP Marketing | Director Comms | [Secure contact] |

### External Contacts

| Organization | Purpose | Contact | Available |
|--------------|---------|---------|-----------|
| Incident Response Firm | Forensics | [Contact] | 24/7 |
| Cyber Insurance | Claims | [Contact] | 24/7 |
| Law Enforcement | Cybercrime | [Contact] | Business hours |
| HHS OCR | Breach reporting | [Contact] | Business hours |

## Metrics and Improvement

### Key Metrics
- Mean Time to Detect (MTTD)
- Mean Time to Respond (MTTR)
- Mean Time to Contain (MTTC)
- Mean Time to Recover (MTTR)
- Number of incidents by type
- Lessons learned implemented

### Continuous Improvement
```javascript
const improveIncidentResponse = {
  collect: ['metrics', 'feedback', 'industry_trends'],
  analyze: ['response_times', 'effectiveness', 'gaps'],
  improve: ['procedures', 'tools', 'training'],
  measure: ['before_after', 'benchmarks', 'compliance']
};
```

---

*Last Updated: [Current Date]*
*Version: 1.0.0*
*Next Review: [Quarterly]*
*Classification: Confidential*
# Access Control Matrix

## Overview

This document defines the Role-Based Access Control (RBAC) matrix for the Botaniqal medical booking system, ensuring appropriate access levels for all user types while maintaining HIPAA compliance and patient privacy.

## User Roles

### 1. **Patient**
- Self-registered users
- Access to own health records only
- Can book appointments and view history

### 2. **Consultant**
- Triage specialists
- Access to patient data during consultation
- Limited to active consultations

### 3. **Practitioner**
- Healthcare providers (doctors, nurses, coaches)
- Access to assigned patient records
- Full medical record access for their patients

### 4. **Admin**
- System administrators
- Manage practitioners and services
- Limited PHI access for support

### 5. **Super Admin**
- Full system access
- Compliance and audit functions
- Emergency access protocols

### 6. **System**
- Automated processes
- Integration services
- Audit and logging

## Access Control Matrix

### Patient Data Access

| Resource | Patient | Consultant | Practitioner | Admin | Super Admin | System |
|----------|---------|------------|--------------|--------|-------------|---------|
| **Own Profile** | RW | - | - | R | RW | R |
| **Own Medical History** | R | - | - | - | R* | - |
| **Own Appointments** | RW | - | - | R | RW | R |
| **Own Forms** | RW | - | - | - | R* | W |
| **Own Payment Info** | R | - | - | - | R* | R |
| **Other Patient Data** | - | - | - | - | R* | - |

*R* = Read with audit logging and justification required

### Appointment Management

| Action | Patient | Consultant | Practitioner | Admin | Super Admin | System |
|--------|---------|------------|--------------|--------|-------------|---------|
| **Book Appointment** | ✓ | ✓ | - | ✓ | ✓ | ✓ |
| **Cancel Own Appointment** | ✓ | - | ✓ | ✓ | ✓ | ✓ |
| **Cancel Any Appointment** | - | - | - | ✓ | ✓ | - |
| **View Schedule** | Own | Assigned | Own | All | All | All |
| **Modify Schedule** | - | - | Own | All | All | ✓ |
| **Phone Booking** | - | ✓ | - | ✓ | ✓ | - |

### Clinical Notes Access

| Action | Patient | Consultant | Practitioner | Admin | Super Admin | System |
|--------|---------|------------|--------------|--------|-------------|---------|
| **Create Notes** | - | ✓ | ✓ | - | - | - |
| **Read Own Notes** | Summary | ✓ | ✓ | - | ✓* | - |
| **Read Patient Notes** | - | Active Only | Assigned | - | ✓* | - |
| **Edit Notes** | - | Own<24h | Own<48h | - | - | - |
| **Delete Notes** | - | - | - | - | ✓* | - |

### System Configuration

| Action | Patient | Consultant | Practitioner | Admin | Super Admin | System |
|--------|---------|------------|--------------|--------|-------------|---------|
| **Manage Services** | - | - | - | ✓ | ✓ | - |
| **Manage Practitioners** | - | - | - | ✓ | ✓ | - |
| **Configure Integrations** | - | - | - | ✓ | ✓ | ✓ |
| **View Audit Logs** | - | - | - | ✓ | ✓ | - |
| **Export Data** | Own | - | Assigned | - | ✓ | ✓ |
| **System Settings** | - | - | - | ✓ | ✓ | - |

## Permission Definitions

### Read (R)
- View data in UI
- Retrieve via API
- Include in reports

### Write (W)
- Create new records
- Update existing data
- Submit forms

### Delete (D)
- Soft delete (archive)
- Hard delete (with approval)
- Bulk operations

### Execute (X)
- Run reports
- Trigger workflows
- Execute system functions

## Implementation Details

### Role Assignment

```sql
-- User roles table
CREATE TABLE user_roles (
    user_id UUID REFERENCES users(id),
    role_id UUID REFERENCES roles(id),
    assigned_at TIMESTAMP DEFAULT NOW(),
    assigned_by UUID REFERENCES users(id),
    expires_at TIMESTAMP,
    PRIMARY KEY (user_id, role_id)
);

-- Role permissions table
CREATE TABLE role_permissions (
    role_id UUID REFERENCES roles(id),
    resource VARCHAR(100),
    action VARCHAR(50),
    conditions JSONB,
    PRIMARY KEY (role_id, resource, action)
);
```

### Permission Checking

```javascript
class AccessControl {
  async checkPermission(userId, resource, action, resourceId = null) {
    // Get user roles
    const roles = await getUserRoles(userId);
    
    // Check each role for permission
    for (const role of roles) {
      const permission = await getPermission(role.id, resource, action);
      
      if (permission) {
        // Check conditions if any
        if (permission.conditions) {
          const allowed = await evaluateConditions(
            permission.conditions,
            userId,
            resourceId
          );
          if (allowed) return true;
        } else {
          return true;
        }
      }
    }
    
    // Log access denial
    await logAccessDenial(userId, resource, action, resourceId);
    return false;
  }
}
```

### Conditional Access

```javascript
// Example: Practitioners can only access their assigned patients
const practitionerPatientAccess = {
  conditions: {
    type: 'sql',
    query: `
      SELECT 1 FROM bookings
      WHERE patient_id = :resourceId
      AND practitioner_id = :userId
      AND status IN ('confirmed', 'completed')
      LIMIT 1
    `
  }
};

// Example: Time-based access for consultants
const consultantTemporaryAccess = {
  conditions: {
    type: 'function',
    name: 'checkActiveConsultation',
    params: ['userId', 'patientId'],
    maxDuration: 3600 // 1 hour
  }
};
```

## Special Access Scenarios

### Break-Glass Access

For emergency situations requiring immediate access:

```javascript
const breakGlassAccess = async (userId, patientId, reason) => {
  // Require additional authentication
  const verified = await verifyBreakGlassAuth(userId);
  if (!verified) throw new Error('Break-glass authentication failed');
  
  // Log the access with reason
  await audit.logBreakGlass({
    userId,
    patientId,
    reason,
    timestamp: new Date(),
    ipAddress: getClientIP()
  });
  
  // Grant temporary elevated access
  await grantTemporaryAccess(userId, patientId, {
    duration: 3600, // 1 hour
    permissions: ['read', 'write'],
    requiresSupervision: true
  });
  
  // Notify compliance team
  await notifyCompliance({
    event: 'break_glass_access',
    user: userId,
    patient: patientId,
    reason
  });
};
```

### Delegation of Access

```javascript
// Practitioners can delegate access during leave
const delegateAccess = async (fromUserId, toUserId, patients, duration) => {
  // Verify delegation is allowed
  const canDelegate = await checkDelegationRights(fromUserId, toUserId);
  if (!canDelegate) throw new Error('Delegation not permitted');
  
  // Create delegation record
  const delegation = await createDelegation({
    from: fromUserId,
    to: toUserId,
    patients,
    startDate: new Date(),
    endDate: addDays(new Date(), duration),
    approved: false
  });
  
  // Require admin approval
  await requestAdminApproval(delegation);
};
```

### Patient Consent Management

```javascript
const patientConsentMatrix = {
  generalAccess: {
    practitioners: true,
    consultants: 'during_consultation',
    researchers: false,
    family: false
  },
  
  dataSharing: {
    referrals: 'explicit_consent',
    insurance: 'explicit_consent',
    research: 'opt_in',
    marketing: 'opt_in'
  },
  
  recordTypes: {
    demographics: 'default_consent',
    medical_history: 'default_consent',
    mental_health: 'explicit_consent',
    substance_use: 'explicit_consent'
  }
};
```

## API Access Control

### JWT Token Structure

```json
{
  "sub": "user_uuid",
  "roles": ["patient"],
  "permissions": ["read:own_profile", "write:appointments"],
  "sessionId": "session_uuid",
  "exp": 1234567890,
  "iat": 1234567890,
  "restrictions": {
    "ip": "203.0.113.0/24",
    "mfa": true
  }
}
```

### API Endpoint Protection

```javascript
// Middleware for role-based access
const requireRole = (roles) => {
  return async (req, res, next) => {
    const userRoles = req.user.roles || [];
    const hasRole = roles.some(role => userRoles.includes(role));
    
    if (!hasRole) {
      await logUnauthorizedAccess(req);
      return res.status(403).json({
        error: 'Insufficient permissions'
      });
    }
    
    next();
  };
};

// Usage in routes
router.get('/api/admin/practitioners',
  authenticate,
  requireRole(['admin', 'super_admin']),
  getPractitioners
);

router.get('/api/patients/:id/records',
  authenticate,
  checkPatientAccess,
  getPatientRecords
);
```

## Frontend Access Control

### Component-Level Permissions

```jsx
// Permission-based component rendering
const ProtectedComponent = ({ required, children }) => {
  const { permissions } = useAuth();
  
  if (!hasPermission(permissions, required)) {
    return <AccessDenied />;
  }
  
  return children;
};

// Usage
<ProtectedComponent required="admin:read_audit_logs">
  <AuditLogViewer />
</ProtectedComponent>
```

### Route Protection

```javascript
const protectedRoutes = {
  '/admin': ['admin', 'super_admin'],
  '/practitioner': ['practitioner', 'admin', 'super_admin'],
  '/consultant': ['consultant', 'admin', 'super_admin'],
  '/patient': ['patient', 'practitioner', 'consultant', 'admin', 'super_admin']
};
```

## Audit Trail Requirements

### Access Log Schema

```sql
CREATE TABLE access_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id UUID NOT NULL,
    resource_type VARCHAR(100) NOT NULL,
    resource_id UUID,
    action VARCHAR(50) NOT NULL,
    result VARCHAR(20) NOT NULL, -- 'allowed', 'denied', 'error'
    reason TEXT,
    ip_address INET,
    user_agent TEXT,
    session_id UUID,
    timestamp TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
    
    -- Indexes for compliance queries
    INDEX idx_access_user_time (user_id, timestamp),
    INDEX idx_access_resource (resource_type, resource_id),
    INDEX idx_access_denied (result) WHERE result = 'denied'
);
```

### Compliance Reporting

```sql
-- Monthly access report
SELECT 
    u.email,
    r.role_name,
    COUNT(*) as access_count,
    COUNT(DISTINCT al.resource_id) as unique_patients,
    SUM(CASE WHEN al.result = 'denied' THEN 1 ELSE 0 END) as denied_count
FROM access_logs al
JOIN users u ON al.user_id = u.id
JOIN user_roles ur ON u.id = ur.user_id
JOIN roles r ON ur.role_id = r.id
WHERE al.timestamp >= DATE_TRUNC('month', CURRENT_DATE)
GROUP BY u.email, r.role_name
ORDER BY access_count DESC;
```

## Security Best Practices

### Principle of Least Privilege
- Users get minimum required access
- Regular access reviews (quarterly)
- Automatic permission expiry
- Just-in-time access for elevated privileges

### Separation of Duties
- No single user can complete critical workflows alone
- Dual approval for sensitive operations
- Segregated admin roles

### Defense in Depth
- Multiple authorization checks
- Database-level row security
- API gateway policies
- Frontend restrictions

## Testing Access Control

```javascript
describe('Access Control Tests', () => {
  test('Patient can only access own records', async () => {
    const patient = await createTestPatient();
    const otherPatient = await createTestPatient();
    
    const canAccessOwn = await checkAccess(
      patient.id,
      'patient_record',
      'read',
      patient.id
    );
    expect(canAccessOwn).toBe(true);
    
    const canAccessOther = await checkAccess(
      patient.id,
      'patient_record',
      'read',
      otherPatient.id
    );
    expect(canAccessOther).toBe(false);
  });

  test('Break-glass access is logged', async () => {
    const admin = await createTestAdmin();
    const patient = await createTestPatient();
    
    await breakGlassAccess(admin.id, patient.id, 'Emergency');
    
    const logs = await getAuditLogs({
      userId: admin.id,
      action: 'break_glass'
    });
    
    expect(logs).toHaveLength(1);
    expect(logs[0].reason).toBe('Emergency');
  });
});
```

---

*Last Updated: [Current Date]*
*Version: 1.0.0*
*Review Schedule: Quarterly*
# Deployment Guide

## Prerequisites

- AWS Account with appropriate permissions
- Node.js 18.x or higher
- Docker installed locally
- Access to production secrets

## Environment Setup

### 1. Environment Variables

Create `.env.production` with:

```bash
# API Keys
CALENDLY_API_KEY=
MEDIRECORDS_API_KEY=
STRIPE_SECRET_KEY=
OFFICE365_CLIENT_ID=
OFFICE365_CLIENT_SECRET=

# AWS Configuration
AWS_REGION=ap-southeast-2
AWS_LAMBDA_FUNCTION_NAME=botaniqal-booking-processor

# Database
DATABASE_URL=

# Security
JWT_SECRET=
ENCRYPTION_KEY=

# Webhooks
CALENDLY_WEBHOOK_SECRET=
JOTFORM_WEBHOOK_SECRET=
```

### 2. AWS Resources Setup

Required AWS services:
- Lambda Functions
- API Gateway
- RDS (for database)
- S3 (for document storage)
- CloudWatch (for logging)
- Secrets Manager

## Deployment Steps

### 1. Build Application

```bash
# Install dependencies
npm install

# Run tests
npm test

# Build production bundle
npm run build
```

### 2. Deploy Lambda Functions

```bash
# Package Lambda functions
npm run package:lambda

# Deploy to AWS
npm run deploy:lambda

# Verify deployment
npm run test:lambda
```

### 3. Deploy API Gateway

```bash
# Deploy API Gateway configuration
aws apigateway create-deployment \
  --rest-api-id <api-id> \
  --stage-name production
```

### 4. Database Migration

```bash
# Run database migrations
npm run migrate:production

# Verify database
npm run db:verify
```

### 5. Configure Webhooks

Configure webhook endpoints in external services:

1. **Calendly**:
   - URL: `https://api.botaniqal.com.au/webhooks/calendly`
   - Events: booking.created, booking.cancelled

2. **Office 365**:
   - URL: `https://api.botaniqal.com.au/webhooks/office365`
   - Subscription type: Calendar events

3. **JotForm**:
   - URL: `https://api.botaniqal.com.au/webhooks/jotform`
   - Trigger: On form submission

### 6. Frontend Deployment

```bash
# Build frontend
npm run build:frontend

# Deploy to hosting (e.g., Vercel, Netlify)
npm run deploy:frontend
```

## Health Checks

### 1. API Health Check
```bash
curl https://api.botaniqal.com.au/health
```

Expected response:
```json
{
  "status": "healthy",
  "version": "1.0.0",
  "services": {
    "database": "connected",
    "calendly": "connected",
    "medirecords": "connected"
  }
}
```

### 2. Integration Tests
```bash
npm run test:integration:production
```

### 3. Monitoring Setup

Configure CloudWatch alarms for:
- API response time > 1s
- Error rate > 1%
- Lambda function failures
- Database connection failures

## Rollback Procedure

### Quick Rollback (< 5 minutes)

```bash
# Revert Lambda to previous version
aws lambda update-alias \
  --function-name botaniqal-booking-processor \
  --name production \
  --function-version <previous-version>

# Revert API Gateway
aws apigateway update-stage \
  --rest-api-id <api-id> \
  --stage-name production \
  --patch-operations op=replace,path=/deploymentId,value=<previous-deployment>
```

### Database Rollback

```bash
# Restore from snapshot
npm run db:restore --snapshot=<snapshot-id>

# Run rollback migrations
npm run migrate:rollback
```

## Production Checklist

- [ ] All environment variables set
- [ ] SSL certificates configured
- [ ] Database backups scheduled
- [ ] Monitoring alerts configured
- [ ] Error tracking enabled (e.g., Sentry)
- [ ] Rate limiting configured
- [ ] CORS settings reviewed
- [ ] Security headers enabled
- [ ] Logging configured
- [ ] Backup procedures tested

## Troubleshooting

### Common Issues

1. **Lambda timeout errors**
   - Increase timeout in Lambda configuration
   - Check for inefficient database queries

2. **Webhook failures**
   - Verify webhook secrets
   - Check CloudWatch logs for errors
   - Ensure proper CORS configuration

3. **Database connection errors**
   - Check security group settings
   - Verify connection pooling configuration
   - Monitor RDS metrics

### Debug Mode

Enable debug logging:
```bash
export DEBUG=botaniqal:*
npm run start:debug
```

## Maintenance

### Regular Tasks

- **Daily**: Check error logs
- **Weekly**: Review performance metrics
- **Monthly**: Update dependencies
- **Quarterly**: Security audit

### Backup Schedule

- **Database**: Daily automated snapshots
- **Document storage**: Daily S3 backup
- **Configuration**: Version controlled in Git

---
*Note: Update this guide with specific values for your deployment.*
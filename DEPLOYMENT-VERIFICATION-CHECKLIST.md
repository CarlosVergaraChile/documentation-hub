# DEPLOYMENT VERIFICATION CHECKLIST

## IMMEDIATE CHECKS (First 5 minutes)

### Health & Connectivity
- [ ] API health endpoint 200 OK
- [ ] All service status checks pass
- [ ] Database connectivity verified
- [ ] Cache layer operational
- [ ] Message queue connected

### Core Functionality  
- [ ] Authentication working
- [ ] API endpoints responding
- [ ] User sessions creating
- [ ] Data retrieval operations
- [ ] Logging operational

## SMOKE TESTS (First 15 minutes)

### User Flows
- [ ] User registration
- [ ] User login
- [ ] Dashboard loading
- [ ] Data export
- [ ] Transactions processing

### API Tests
- [ ] GET requests 200
- [ ] POST creating resources
- [ ] PUT updating resources
- [ ] DELETE removing resources
- [ ] Error responses proper

### Database
- [ ] Data persisting
- [ ] Transactions completing
- [ ] No data corruption
- [ ] Replication lag <1s

## PERFORMANCE (First 30 minutes)

### Latency
- [ ] p50 <100ms
- [ ] p95 <300ms
- [ ] p99 <500ms
- [ ] Page load <2s

### Resources
- [ ] CPU <70%
- [ ] Memory <80%
- [ ] Disk <85%
- [ ] Network <60%

## SECURITY (First 60 minutes)

### Auth & Access
- [ ] JWT tokens valid
- [ ] Unauthorized requests rejected
- [ ] RBAC working
- [ ] Rate limiting enforced

### Data Protection
- [ ] HTTPS/TLS enabled
- [ ] Certificates valid
- [ ] Sensitive data encrypted
- [ ] No secrets in logs

### Vulnerabilities
- [ ] Security headers present
- [ ] CORS configured
- [ ] CSRF protection active
- [ ] SQL injection prevention

## INTEGRATION (First 2 hours)

### Service Communication
- [ ] Service-to-service calls
- [ ] Event bus messaging
- [ ] Message queue processing
- [ ] Async workflows

### External Integration
- [ ] Third-party API calls
- [ ] Webhooks delivering
- [ ] Email notifications
- [ ] Slack notifications

### Data Consistency
- [ ] Data synchronized
- [ ] Cache invalidation
- [ ] Replicas in sync
- [ ] Message delivery

## PRODUCTION MONITORING (Ongoing)

### Errors
- [ ] Error rate <0.1%
- [ ] No spikes
- [ ] 5xx <10/hour
- [ ] 4xx expected levels

### Business
- [ ] Conversion rate normal
- [ ] User engagement healthy
- [ ] Revenue processing
- [ ] KPIs in range

### Infrastructure
- [ ] Pods healthy
- [ ] No resource spikes
- [ ] Network stable
- [ ] Backups completing

## ROLLBACK TRIGGER

Any critical check failure -> Rollback immediately

```bash
git revert <commit>
git push origin main
kubectl rollout undo deployment/[service]
```

## SIGN-OFF

Verified: _______ Time: _______
Approved: _______ Time: _______
Notes: ________________________

Version 1.0 - January 9, 2026

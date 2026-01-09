# OPERATIONAL RUNBOOK 2026 - Quick Reference

## Daily Operations

### 1. Morning Health Check (8:00 AM)
- Check monitoring dashboard
- Review error rates
- Verify all services green
- Alert if incidents detected

### 2. Incident Response (By Severity)

P1 (Critical, <5 min SLA)
- All services down
- Data corruption
- Security breach

P2 (High, <30 min SLA)
- Single service down
- Performance degradation

P3 (Medium, <4 hours SLA)
- Minor feature broken
- Documentation issues

P4 (Low, <24 hours SLA)
- Feature requests
- Minor bugs

### 3. Deployment

Pre-deployment:
1. All tests pass
2. Code review approved
3. Staging verified
4. Smoke tests green

Production deployment:
1. Create git tag
2. GitHub Actions deploys
3. Monitor metrics
4. Verify health check

Rollback:
```
git revert <commit>
git push origin main
```

### 4. Database Operations

Backup:
```
aws rds create-db-snapshot --db-instance-identifier prod-db
```

Restore:
```
aws rds restore-db-instance-from-db-snapshot --db-snapshot-identifier backup
```

### 5. Scaling

Horizontal scaling:
```
kubectl scale deployment api-gateway --replicas=10
```

Vertical scaling:
```
kubectl edit deployment api-gateway
# Update CPU/memory resources
```

### 6. Communication During Incidents

- Critical: Slack #critical-alerts + SMS + Phone
- High: Slack #incidents + Email
- Medium: Slack #alerts + Ticket
- Low: Ticket only

### 7. Emergency Contacts

On-Call: #oncall channel
Engineering Lead: [contact info]
Operations: [contact info]
Security: [contact info]

---

Last Updated: January 9, 2026
Owner: @operations-team
Review Date: April 9, 2026

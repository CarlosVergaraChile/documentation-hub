# Deployment Guide - CarlosVergaraChile Ecosystem

## Introducción
Guía completa para deployments en los 25 repositorios. Soporta staging y producción.

## 1. Pre-deployment Checklist

### Antes de cualquier deployment
- [ ] Tests pasan 100%
- [ ] Coverage >= 80%
- [ ] Linting sin errores
- [ ] Security scan sin issues críticos
- [ ] CHANGELOG.md actualizado
- [ ] Documentación actualizada
- [ ] No hay TODO/FIXME en código
- [ ] Dependencias actualizadas

## 2. Staging Deployment

### 2.1 Triggers Automáticos
- Push a `develop` branch
- Pull Request merge a `develop`
- Manual trigger desde GitHub Actions

### 2.2 Staging Workflow

```bash
# 1. Build
docker build -t registry.example.com/app:dev-$(git rev-parse --short HEAD) .

# 2. Push to registry
docker push registry.example.com/app:dev-$(git rev-parse --short HEAD)

# 3. Update deployment
kubectl set image deployment/app-staging \
  app=registry.example.com/app:dev-$(git rev-parse --short HEAD) \
  -n staging

# 4. Monitor rollout
kubectl rollout status deployment/app-staging -n staging
```

### 2.3 Testing en Staging
- Smoke tests: 5 minutos
- Integration tests: 15 minutos
- Performance tests: 10 minutos
- Security scan: 5 minutos

### 2.4 Rollback (si es necesario)
```bash
kubectl rollout undo deployment/app-staging -n staging
kubectl rollout history deployment/app-staging -n staging
```

## 3. Production Deployment

### 3.1 Requisitos
- Staging deployment exitoso (>= 24 horas)
- Approval de 2 mantenedores
- Change log completado
- No hay incidents abiertos

### 3.2 Release Process

```bash
# 1. Create release tag
git tag -a v1.2.3 -m "Release 1.2.3"
git push origin v1.2.3

# 2. GitHub Actions triggers automatically:
#    - Build production image
#    - Create GitHub Release
#    - Deploy to production (canary)

# 3. Monitor canary (10% traffic)
kubectl describe deployment app-prod -n production
watch kubectl get pods -n production

# 4. Promote to 100% (after 1 hour if healthy)
kubectl patch deployment app-prod -n production \
  -p '{"spec":{"replicas":3}}'
```

### 3.3 Production Rollout Strategy

**Canary Deployment** (primeros 10% de usuarios)
- Monitoreo de errores: < 0.1%
- Latencia: < 200ms (p95)
- CPU: < 60%
- Memoria: < 70%

**Time-based Promotion**
- Canary: 1-2 horas
- 25%: 30 minutos
- 50%: 30 minutos
- 75%: 30 minutos
- 100%: Final

## 4. Database Migrations

### 4.1 Pre-migration
- Backup completo
- Rehearsal en staging
- Estimación de tiempo
- Plan de rollback

### 4.2 Migration Steps

```bash
# 1. Deploy new code (backward compatible)
git tag v1.2.3-pre-migration
git push origin v1.2.3-pre-migration

# 2. Wait for deployment (5 minutes)

# 3. Run migration
kubectl exec -it pod/app-prod-xxx -- \
  python -m alembic upgrade head

# 4. Verify migration
psql -h db.prod -U admin -d mydb -c "SELECT version();"

# 5. Deploy final code
git tag v1.2.3
git push origin v1.2.3
```

### 4.3 Rollback Migration

```bash
# 1. Downgrade database
kubectl exec -it pod/app-prod-xxx -- \
  python -m alembic downgrade -1

# 2. Redeploy previous version
kubectl rollout undo deployment/app-prod -n production
```

## 5. Monitoring Post-Deployment

### 5.1 Health Checks
- API ping: `GET /health`
- Database: `SELECT 1`
- Cache: Redis ping
- Message queue: Connection alive

### 5.2 Key Metrics
- Error rate: < 0.1%
- P95 latency: < 200ms
- P99 latency: < 500ms
- Throughput: > 1000 req/s
- CPU: < 70%
- Memory: < 75%

### 5.3 Alerting
```yaml
alerts:
  - ErrorRateHigh: > 1%
  - LatencyHigh: P95 > 300ms
  - PodCrashing: Restarts > 3/hour
  - DiskSpace: < 10% free
  - CPUHigh: > 80%
```

## 6. Deployment Logs

### 6.1 Ubicaciones
- GitHub Actions: `Actions` tab
- Kubernetes: `kubectl logs -f pod/XXX`
- Application: CloudWatch/ELK
- Database: Query logs

### 6.2 Log Analysis
```bash
# Check recent deployments
kubectl rollout history deployment/app-prod -n production

# View logs
kubectl logs -f deployment/app-prod -n production

# Check pod status
kubectl describe pod/app-prod-xxx -n production
```

## 7. Incident Response

### 7.1 If Deployment Fails
1. Pause promocion (automatico)
2. Examine logs
3. Rollback: `kubectl rollout undo deployment/app-prod`
4. Investigate root cause
5. Fix and re-deploy

### 7.2 If Performance Degrades
1. Check metrics
2. Compare against baseline
3. Investigate: New dependencies? Queries? Traffic?
4. If critical: Rollback
5. Optimize and re-deploy

## 8. Deployment Checklist por Capa

### Capa 1 - Data
- [ ] Database schema validated
- [ ] Migration tested
- [ ] Indexes created
- [ ] Backup verified

### Capa 2 - Logic
- [ ] ML models optimized
- [ ] Curriculum loaded
- [ ] Analytics tables ready
- [ ] Cache warmed

### Capa 3 - Services
- [ ] API gateway configured
- [ ] Auth service updated
- [ ] Rate limits set
- [ ] Notifications working

### Capa 4 - Frontend
- [ ] CSS bundles optimized
- [ ] JavaScript minified
- [ ] Assets cached
- [ ] CDN propagated

## 9. Post-Deployment

### 24 horas después
- Monitor error logs
- Check business metrics
- Validate user experience
- Review performance trends

### 1 semana después
- Performance analysis
- User feedback
- Business impact
- Identify improvements

## 10. Useful Commands

```bash
# Deployment status
kubectl get deployments -n production
kubectl describe deployment app-prod -n production

# Rollout history
kubectl rollout history deployment/app-prod -n production

# View logs
kubectl logs -f deployment/app-prod -n production --all-containers

# Scale deployment
kubectl scale deployment app-prod --replicas=5 -n production

# Execute command in pod
kubectl exec -it pod/app-prod-xxx -- /bin/bash

# Port forward
kubectl port-forward pod/app-prod-xxx 8080:8080 -n production
```

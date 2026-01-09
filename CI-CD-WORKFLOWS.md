# CI/CD Workflows - Ecosistema CarlosVergaraChile

## Visión General
Pipelines de CI/CD automatizados para todos los 25 repositorios usando GitHub Actions.

## 1. Pipelines Estándar

### 1.1 Pull Request Workflow
**Triggers**: `pull_request` on `develop`, `main`

```yaml
name: PR Checks
on:
  pull_request:
    branches: [develop, main]

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Lint
        run: |
          pip install ruff  # Python
          ruff check .

  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Run Tests
        run: pytest --cov=. --cov-report=xml
      - name: Upload Coverage
        uses: codecov/codecov-action@v3

  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Security Scan
        run: pip install bandit && bandit -r .
```

### 1.2 Build & Deploy to Staging
**Triggers**: `push` to `develop`

```yaml
name: Build Staging
on:
  push:
    branches: [develop]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build Image
        run: |
          docker build -t ${{ secrets.REGISTRY_URL }}/app:dev-${{ github.sha }} .
          docker push ${{ secrets.REGISTRY_URL }}/app:dev-${{ github.sha }}

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Staging
        run: |
          kubectl set image deployment/app-staging \
            app=${{ secrets.REGISTRY_URL }}/app:dev-${{ github.sha }} \
            --record
```

### 1.3 Release & Deploy to Production
**Triggers**: `push` to `main` with tag `v*`

```yaml
name: Release Production
on:
  push:
    branches: [main]
    tags: ['v*']

jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Create Release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: ${{ github.ref }}
          release_name: Release ${{ github.ref }}
          body_path: CHANGELOG.md

  build-production:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build Production Image
        run: |
          docker build -t ${{ secrets.REGISTRY_URL }}/app:${{ github.ref_name }} .
          docker push ${{ secrets.REGISTRY_URL }}/app:${{ github.ref_name }}

  deploy-production:
    needs: build-production
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Production
        run: |
          kubectl set image deployment/app-prod \
            app=${{ secrets.REGISTRY_URL }}/app:${{ github.ref_name }} \
            --record
```

## 2. Workflows Especializados

### 2.1 Python Data Layer
- Lint: `ruff`, `mypy`
- Test: `pytest` with `pytest-cov`
- Build: `pip freeze > requirements.txt`

### 2.2 Go Services
- Lint: `golangci-lint`
- Test: `go test -cover ./...`
- Build: `GOOS=linux go build -o app`

### 2.3 JavaScript Frontend
- Lint: `ESLint`, `Prettier`
- Test: `Jest` with coverage
- Build: `npm run build`

## 3. Deployment Strategies

### 3.1 Canary Deployment
```yaml
# Deploy to 10% of pods, monitor, then 100%
strategy:
  canary:
    steps: 10
    maxUnavailable: 1
    interval: 5m
```

### 3.2 Blue-Green Deployment
```yaml
# Keep two identical production environments
# Switch traffic between them for zero downtime
```

### 3.3 Rolling Deployment
```yaml
strategy:
  rolling:
    maxSurge: 1
    maxUnavailable: 0
```

## 4. Secrets Management

### 4.1 Required Secrets
```
REGISTRY_URL        # Container registry URL
REGISTRY_USERNAME   # Registry credentials
REGISTRY_PASSWORD   # Registry credentials
KUBE_CONFIG         # Kubernetes config
DB_PASSWORD         # Database credentials
API_KEYS            # Third-party API keys
```

### 4.2 Secret Rotation
- Automated rotation: 90 days
- Manual rotation: On demand
- Audit logging: All access logged

## 5. Monitoring & Alerts

### 5.1 Pipeline Metrics
- Build time
- Test coverage
- Deployment frequency
- Mean time to recovery (MTTR)

### 5.2 Alert Rules
```yaml
# Critical alerts
- Build failure
- Test failure
- Deployment failure
- Security scan failure

# Warning alerts
- High memory usage
- Slow tests (> 10 min)
- Coverage drop (< 80%)
```

## 6. Artifact Management

### 6.1 Image Registry
- **URL**: registry.example.com
- **Naming**: `{org}/{repo}:{branch|tag}`
- **Retention**: Keep last 10 images per branch

### 6.2 Release Artifacts
- GitHub Releases: For tagged versions
- Container images: For deployments
- Documentation: Auto-generated

## 7. Troubleshooting

### Common Issues

**Build timeout**
- Increase timeout: `timeout-minutes: 30`
- Optimize dependencies
- Cache layers

**Test failure in CI but not locally**
- Check Node version
- Check environment variables
- Check data state

**Deployment failure**
- Check image exists
- Check cluster access
- Check resource limits

## 8. Best Practices

1. **Fail fast**: Stop on first error
2. **Cache aggressively**: npm, pip, docker
3. **Parallel jobs**: Run independent steps in parallel
4. **Matrix strategy**: Test multiple versions
5. **Notifications**: Slack/Email on failure
6. **Artifacts**: Save logs for debugging
7. **Status checks**: Required for merge

# Ecosystem Standards - CarlosVergaraChile

## Propósito
Estándares consolidados para garantizar consistencia, calidad y mantenibilidad en los 25 repositorios del ecosistema.

## 1. Estándares de Código

### 1.1 Lenguajes Soportados

#### Python
- **Version**: 3.11+
- **Linter**: ruff
- **Formatter**: black
- **Type Checker**: mypy
- **Testing**: pytest
- **Coverage**: > 80%

#### Go
- **Version**: 1.21+
- **Linter**: golangci-lint
- **Formatter**: gofmt
- **Testing**: Go test
- **Coverage**: > 80%

#### JavaScript/TypeScript
- **Version**: Node 18+
- **Linter**: ESLint
- **Formatter**: Prettier
- **Testing**: Jest/Vitest
- **Coverage**: > 80%

### 1.2 Configuración Común

```yaml
# .editorconfig
root = true
[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{py,go,js,ts}]
indent_style = space
indent_size = 4
```

### 1.3 Naming Conventions

- **Files**: snake_case (Python), camelCase (JS)
- **Functions**: snake_case
- **Classes**: PascalCase
- **Constants**: UPPERCASE_WITH_UNDERSCORES

## 2. Estructura de Repositorios

### 2.1 Directorios Base
```
repository/
├── src/              # Source code
├── tests/            # Test files
├── docs/             # Documentation
├── scripts/          # Utility scripts
├── .github/          # GitHub Actions
│   └── workflows/    # CI/CD workflows
├── README.md         # Repository overview
├── CHANGELOG.md      # Version history
├── LICENSE           # License file
└── .gitignore        # Git ignore rules
```

### 2.2 Essential Files

**README.md**
- Project description
- Installation instructions
- Usage examples
- Contributing guidelines
- License information

**CHANGELOG.md**
- Format: Keep a Changelog
- Sections: Added, Changed, Deprecated, Removed, Fixed, Security

**.gitignore**
- Language-specific patterns
- Environment files
- Build artifacts
- IDE configurations

## 3. Git Workflow

### 3.1 Branch Strategy
- **main**: Production code
- **develop**: Integration branch
- **feature/***: New features
- **bugfix/***: Bug fixes
- **hotfix/***: Production hotfixes

### 3.2 Commit Messages
Format: `<type>(<scope>): <subject>`

Types: feat, fix, docs, style, refactor, perf, test, chore

Example: `feat(auth): add OAuth2 support`

### 3.3 Pull Request Process
1. Create feature branch from develop
2. Implement changes with tests
3. Create PR with description
4. Minimum 1 approval required
5. All checks must pass
6. Merge to develop
7. Create release PR to main

## 4. Testing Standards

### 4.1 Test Organization
```
tests/
├── unit/          # Unit tests
├── integration/   # Integration tests
├── e2e/           # End-to-end tests
└── performance/   # Performance tests
```

### 4.2 Naming Conventions
- Test files: `test_*.py` or `*_test.go`
- Test functions: `test_<function_name>`
- Test classes: `Test<ClassName>`

### 4.3 Coverage Requirements
- Minimum: 80%
- Critical paths: 100%
- Excluded: Constants, Config, Auto-generated

## 5. Documentation Standards

### 5.1 Code Documentation
- Docstrings for all public functions
- Type hints in all functions
- Comments for complex logic
- Avoid redundant comments

### 5.2 API Documentation
- OpenAPI/Swagger specs
- Request/Response examples
- Error codes documented
- Rate limits documented

### 5.3 Architecture Documentation
- ADR (Architecture Decision Records)
- Sequence diagrams for flows
- Component diagrams
- Deployment architecture

## 6. Security Standards

### 6.1 Code Security
- No hardcoded secrets
- Input validation
- SQL injection prevention
- CORS properly configured
- CSRF protection enabled

### 6.2 Dependency Management
- Dependabot enabled
- Regular updates checked
- Vulnerable dependencies blocked
- License compliance verified

### 6.3 Secrets Management
- GitHub Secrets for CI/CD
- Environment variables for local
- Rotation policy: 90 days
- Audit logging enabled

## 7. Performance Standards

### 7.1 API Performance
- Response time: < 200ms (p95)
- Throughput: > 1000 req/s
- Memory: < 512MB per instance
- CPU: < 80% under load

### 7.2 Database Performance
- Query time: < 100ms (p95)
- Connection pooling enabled
- Indexes on foreign keys
- Query optimization reviewed

## 8. CI/CD Standards

### 8.1 GitHub Actions Workflows
- Lint check on PR
- Test execution on PR
- Coverage report on PR
- Build artifact creation
- Deployment to staging
- Production deployment

### 8.2 Checks Required
- Tests pass
- Coverage maintained
- Linting passes
- Security scan passes
- No breaking changes

## 9. Versioning

### 9.1 Semantic Versioning
- Format: MAJOR.MINOR.PATCH
- MAJOR: Breaking changes
- MINOR: New features
- PATCH: Bug fixes

### 9.2 Release Tagging
- Tag format: `v{MAJOR}.{MINOR}.{PATCH}`
- Tag on main branch only
- Release notes in GitHub Releases

## 10. Monitoring & Observability

### 10.1 Logging
- Structured logging
- Log levels: DEBUG, INFO, WARNING, ERROR, CRITICAL
- Correlation IDs for tracing
- No sensitive data in logs

### 10.2 Metrics
- Application metrics
- Business metrics
- Infrastructure metrics
- Dashboards for each component

### 10.3 Tracing
- Distributed tracing enabled
- Transaction tracking
- Performance insights
- Error tracking

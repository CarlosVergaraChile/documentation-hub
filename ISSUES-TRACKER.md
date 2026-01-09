# Issues Tracker - Ecosistema CarlosVergaraChile

## Propósito
Central de control y gestión de issues de los 25 repositorios del ecosistema.

## Estado Global - 2026

### Resumen por Capa

| Capa | Repository | Issues | Critical | High | Medium | Low | Status |
|------|------------|--------|----------|------|--------|-----|--------|
| **1. DATOS** | postgres-migration | 3 | 0 | 1 | 1 | 1 | 🟡 Active |
| | elasticsearch-indexer | 5 | 0 | 2 | 2 | 1 | 🟡 Active |
| | redis-cache-layer | 4 | 0 | 1 | 2 | 1 | 🟡 Active |
| | data-validator | 2 | 0 | 0 | 1 | 1 | 🟢 Maintenance |
| **2. LÓGICA** | education-ml | 6 | 1 | 2 | 2 | 1 | 🟡 Active |
| | student-analytics | 4 | 0 | 1 | 2 | 1 | 🟡 Active |
| | curriculum-engine | 3 | 0 | 1 | 1 | 1 | 🟢 Maintenance |
| | assessment-scorer | 2 | 0 | 0 | 1 | 1 | 🟢 Maintenance |
| **3. SERVICIOS** | api-gateway | 5 | 0 | 2 | 2 | 1 | 🟡 Active |
| | auth-service | 4 | 0 | 1 | 2 | 1 | 🟡 Active |
| | notification-service | 3 | 0 | 1 | 1 | 1 | 🟢 Maintenance |
| | payment-service | 2 | 0 | 0 | 1 | 1 | 🟢 Maintenance |
| **4. FRONTEND** | student-dashboard | 7 | 0 | 2 | 3 | 2 | 🟡 Active |
| | instructor-portal | 5 | 0 | 1 | 3 | 1 | 🟡 Active |
| | admin-console | 3 | 0 | 1 | 1 | 1 | 🟢 Maintenance |
| | mobile-app | 4 | 0 | 1 | 2 | 1 | 🟡 Active |
| **HERRAMIENTAS** | dashboard-proyectos | 2 | 0 | 0 | 1 | 1 | 🟢 Maintenance |
| | documentation-hub | 1 | 0 | 0 | 0 | 1 | 🟢 Maintenance |
| | cli-tools | 3 | 0 | 1 | 1 | 1 | 🟢 Maintenance |

**Totales**: 73 Issues | 1 Critical | 19 High | 33 Medium | 20 Low

## Issues Críticos (1)

### [P0] education-ml: Model Training Pipeline Failure
- **Repository**: education-ml
- **Severity**: CRITICAL
- **Status**: IN_PROGRESS
- **Priority**: P0
- **Assigned**: @CarlosVergaraChile
- **Due Date**: 2026-01-15
- **Description**: El pipeline de entrenamiento de modelos ML falla al procesar datasets > 1GB
- **Impact**: Sistema de recomendaciones no funciona para cursos grandes
- **Action Items**:
  - [ ] Optimizar carga de datos (batching)
  - [ ] Implementar streaming para large files
  - [ ] Agregar monitoring de memoria
  - [ ] Testing con datasets de prueba
- **Blocked By**: None
- **Blocks**: auth-service, student-dashboard

## Issues de Alta Prioridad (19)

### Capa 1 - Datos
1. **postgres-migration**: Optimize slow queries en tabla students
   - Status: OPEN | Priority: HIGH | Est: 5d
2. **elasticsearch-indexer**: Index not updating in real-time
   - Status: IN_PROGRESS | Priority: HIGH | Est: 3d

### Capa 2 - Lógica
1. **education-ml**: Add cross-validation to recommendation model
   - Status: OPEN | Priority: HIGH | Est: 8d
2. **student-analytics**: Handle missing data in enrollment reports
   - Status: OPEN | Priority: HIGH | Est: 4d

### Capa 3 - Servicios
1. **api-gateway**: Implement rate limiting
   - Status: OPEN | Priority: HIGH | Est: 6d
2. **api-gateway**: Add request validation middleware
   - Status: IN_PROGRESS | Priority: HIGH | Est: 4d

### Capa 4 - Frontend
1. **student-dashboard**: Performance optimization on large class lists
   - Status: OPEN | Priority: HIGH | Est: 5d
2. **student-dashboard**: Fix accessibility issues (WCAG 2.1)
   - Status: IN_PROGRESS | Priority: HIGH | Est: 3d

## Roadmap por Semana

### Semana 1 (Ene 6-12)
- [ ] Resolver CRITICAL issue en education-ml
- [ ] 5 High priority issues
- [ ] Code review backlog reduction

### Semana 2 (Ene 13-19)
- [ ] Completar fixes en Capa 2
- [ ] Actualizar tests
- [ ] Deploy staging

### Semana 3 (Ene 20-26)
- [ ] Performance testing
- [ ] Security audit
- [ ] Production release

## Etiquetas (Labels)

- **bug**: Defecto en funcionalidad existente
- **feature**: Nueva funcionalidad
- **enhancement**: Mejora a funcionalidad existente
- **documentation**: Falta o incompleta
- **performance**: Optimización de velocidad/memoria
- **security**: Issue de seguridad
- **devops**: Infraestructura/CI-CD
- **breaking-change**: Cambia API pública

## Proceso de Issues

### Creation
1. Seleccionar template (Bug, Feature, etc.)
2. Llenar todos los campos requeridos
3. Asignar labels apropiados
4. Estimar effort en story points

### Triage
1. Revisar descripción
2. Validar reproducibilidad
3. Asignar prioridad
4. Asignar propietario

### Work
1. Crear branch: feature/issue-XXX
2. Implementar con tests
3. Pull Request
4. Code review
5. Merge y close

### Metrics
- Issues creadas/cerradas por semana
- Tiempo promedio de resolución
- Distribución por prioridad
- Distribución por tipo

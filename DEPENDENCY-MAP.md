# DEPENDENCY MAP - Mapa de Integraciones

## Mapeo de Dependencias entre Sistemas
**Última actualización: 9 Enero 2026**

---

## Arquitectura de 4 Capas y Flujos

```
CAPA 4: CONSUL TORÍA
├─ GL Strategic MVP
├─ GL Strategic Web Sites (2)
└─ Máquina Orquestadora IA
          ↓ (requiere datos)
CAPA 3: INTELIGENCIA (AI/Analytics)
├─ NOVIS Executive Dashboard
├─ AI-PR Orchestrator
└─ Analytics Engine
          ↓ (consume eventos)
CAPA 2: OPERACIONES (Orchestration)
├─ COIPO (n8n Workflows)
├─ OAuth2 Auth Layer
└─ Gmail Integration
          ↓ (ejecuta)
CAPA 1: EDUCACIÓN (Core)
├─ SAM v3.0
├─ Material Generator
├─ Test Evaluator
└─ Payment Gateway
          ↓ (escribe logs)
DATA HUB CENTRAL
├─ documentation-hub (logs)
├─ Metrics DB
└─ Central Dashboard
```

---

## Matriz de Dependencias

### SAM (Sistema Asistente de Maestros)
- **Dependencias:**
  - Mercado Pago (payment)
  - COIPO (workflow triggers)
  - NOVIS (analytics)
- **Dependientes:**
  - COIPO (consume events)
  - NOVIS (receives metrics)
  - dashboard-proyectos (reports)
- **Datos:** Usuarios, Cursos, Evaluaciones
- **Eventos:** course.started, evaluation.submitted, payment.completed

### COIPO (Orchestrator)
- **Dependencias:**
  - n8n (workflow engine)
  - OAuth2 (authentication)
  - Gmail API (notifications)
  - SAM (triggering)
- **Dependientes:**
  - NOVIS (workflow status)
  - dashboard-proyectos (monitors)
- **Eventos:** workflow.triggered, workflow.completed, task.assigned

### NOVIS (Executive Dashboard)
- **Dependencias:**
  - SAM (metrics)
  - COIPO (workflow data)
  - AI-PR Orchestrator (PR data)
  - Analytics DB
- **Dependientes:**
  - GL Strategic (reports)
  - dashboard-proyectos (KPIs)
- **Eventos:** metric.updated, alert.raised, report.generated

### AI-PR-Orchestrator
- **Dependencias:**
  - GitHub API
  - ChatGPT, Claude, DeepSeek (AI APIs)
  - COIPO (event triggers)
- **Dependientes:**
  - NOVIS (statistics)
  - dashboard-proyectos (PR metrics)
- **Eventos:** pr.analyzed, review.generated, code.suggestion

### GL-Strategic (Consulting Platform)
- **Dependencias:**
  - NOVIS (data/reports)
  - Máquina Orquestadora IA
  - SAM (course data)
- **Dependientes:**
  - Marketing Teams
  - Clients
- **Events:** project.created, analysis.completed, recommendation.generated

---

## Flujos de Datos Principales

### 1. Flujo de Usuario en SAM
```
Usuario entra en SAM
    ↓
SAM genera/asigna material
    ↓ (evento: course.started)
COIPO recibe evento
    ↓
COIPO envia notificación (Gmail)
    ↓
NOVIS registra evento
    ↓
dashboard-proyectos actualiza KPI
```

### 2. Flujo de Evaluación
```
Estudiante envía evaluación a SAM
    ↓ (evento: evaluation.submitted)
SAM procesa y almacena
    ↓
NOVIS recibe métrica
    ↓
NOVIS genera reporte de cohorte
    ↓
GL Strategic accede para análisis
```

### 3. Flujo de PR Review
```
Pull Request creado
    ↓
AI-PR-Orchestrator activado
    ↓ (múltiples AI models)
Review generado
    ↓
NOVIS registra estadísticas
    ↓
dashboard-proyectos muestra métricas
```

### 4. Flujo de Orquestación
```
COIPO workflow triggered
    ↓
Ejecuta tareas secuenciales
    ↓
Integra con SAM, NOVIS, externos
    ↓ (evento: workflow.completed)
NOVIS actualiza dashboard
    ↓
Notificación a usuarios
```

---

## Integraciones Externas

### Servicios Terceros
- **Mercado Pago:** SAM payment processing
- **n8n:** COIPO workflow execution
- **GitHub API:** AI-PR-Orchestrator PRs
- **Gmail API:** COIPO notifications
- **Multiple AI APIs:**
  - OpenAI (ChatGPT)
  - Anthropic (Claude)
  - DeepSeek
  - Google (Gemini)
  - Groq

### Hosting & Infrastructure
- **Frontend:** Vercel (Netlify alternative)
- **APIs:** Vercel Functions, Railway, AWS
- **Databases:** MongoDB Atlas, Supabase
- **Logs:** Vercel, CloudWatch, ELK Stack

---

## API Contracts (Webhooks)

### SAM → COIPO
```json
POST /coipo/webhooks/sam-events
{
  "event": "course.started",
  "userId": "xxx",
  "courseId": "yyy",
  "timestamp": "2026-01-09T14:00:00Z"
}
```

### COIPO → NOVIS
```json
POST /novis/webhooks/workflow-complete
{
  "event": "workflow.completed",
  "workflowId": "xxx",
  "status": "success",
  "metrics": {"duration": 120, "tasksCompleted": 5}
}
```

### NOVIS → dashboard-proyectos
```json
WS: wss://dashboard.com/ws/kpi-updates
{
  "metric": "active_users",
  "value": 245,
  "timestamp": "2026-01-09T14:05:00Z"
}
```

---

## Dependencias Críticas (Sin Estas NO Funciona)

**Tier 1 - MUY CRÍTICAS:**
1. SAM ↔ COIPO (orquestación)
2. SAM ↔ Payment Gateway (monetización)
3. COIPO ↔ n8n (ejecución)
4. GitHub API ↔ AI-PR-Orchestrator (PRs)

**Tier 2 - IMPORTANTES:**
1. NOVIS ↔ SAM (analytics)
2. NOVIS ↔ COIPO (workflow monitoring)
3. Dashboard ↔ NOVIS (KPIs)

**Tier 3 - OPCIONALES:**
1. GL-Strategic ↔ NOVIS (reports)
2. Máquina Orquestadora ↔ NOVIS (AI insights)

---

## Bottlenecks & Single Points of Failure

🔴 **CRÍTICOS:**
- Mercado Pago down → SAM no procesa pagos
- n8n down → COIPO no ejecuta workflows
- GitHub API down → AI-PR no funciona
- SAM DB down → Todo se cae

🟡 **IMPORTANTES:**
- NOVIS DB down → No hay analytics
- OAuth2 down → No hay autenticación
- Gmail API down → No hay notificaciones

**Mitigación:**
- Implementar Circuit Breakers
- Fallbacks a servicios alternativos
- Caching de datos críticos
- Database replication

---

## Próximos Pasos (Fase 2)

1. **Crear Diagramas Mermaid** de flujos
2. **Implementar Circuit Breakers** en COIPO
3. **Setup de Webhooks** en SAM y NOVIS
4. **Monitoring de Dependencias** en dashboard
5. **Health Check APIs** en todos los sistemas

*Mapa completado el 9 Enero 2026*

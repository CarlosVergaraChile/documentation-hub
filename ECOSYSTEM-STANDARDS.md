# 📜 ECOSYSTEM STANDARDS & ENGINEERING MANIFESTO

**Versión:** 1.0.0 | **Jurisdicción:** Todos los repositorios de CarlosVergaraChile

---

## 🎯 1. Principios Fundamentales

- **Code is Liability**: El mejor código es el que no se escribe. Reutiliza antes de crear.
- **Single Source of Truth**: documentation-hub dicta las reglas.
- **Automate Everything**: Si tienes que hacerlo más de dos veces, automatízalo (CI/CD).

---

## 🏗️ 2. Arquitectura de Backend (Node.js/Express)

Para mantener la escalabilidad y testabilidad, se exige el patrón **Controller-Service-Model**.

### ❌ PROHIBIDO (Fat Controllers)
Meter lógica de negocio, llamadas a DB y respuestas HTTP en un solo archivo.

### ✅ OBLIGATORIO (Separation of Concerns)

- **Controller** (`/controllers`): Solo recibe HTTP (req), valida inputs y responde (res). Llama al Service.
- **Service** (`/services`): Contiene la lógica pura de negocio ("Si usuario existe, enviar email"). Llama al Model.
- **Model** (`/models`): Esquema de base de datos (Mongoose/SQL). Solo habla con la DB.

### Estructura de Carpetas Estándar

```
src/
├── config/         # Variables de entorno y configs (DB, Logger)
├── controllers/    # Manejo de peticiones HTTP
├── services/       # Lógica de negocio
├── models/         # Esquemas de datos
├── routes/         # Definición de endpoints
├── utils/          # Helpers puros (fechas, formatos)
└── app.js          # Entry point
```

---

## 🖥️ 3. Estándares de Código (Code Style)

- **Formateo**: Prettier es obligatorio en raíz (`.prettierrc`). Sin debate sobre puntos y coma.
- **Linting**: ESLint debe pasar sin errores antes de mergear (`npm run lint`).

### Naming Conventions

- **Variables/Funciones**: `camelCase` (ej: `getUserData`)
- **Clases/Componentes**: `PascalCase` (ej: `UserProfile`)
- **Constantes**: `UPPER_SNAKE_CASE` (ej: `MAX_RETRY_ATTEMPTS`)
- **Booleanos**: Prefijos `is`, `has`, `should` (ej: `isActive`)

---

## 🛡️ 4. Seguridad (Security Baseline)

- **Cero Secretos**: NUNCA commitear `.env`, claves API, o credenciales. Usar `dotenv` localmente y GitHub Secrets en producción.
- **Validación de Inputs**: NUNCA confiar en el usuario. Validar `req.body` y `req.params` (usando Joi, Zod o express-validator) antes de procesar.
- **Dependencias**: Prohibido usar librerías con vulnerabilidades críticas (verificadas por Dependabot/npm audit).

---

## 🔀 5. Flujo de Trabajo Git (Git Workflow)

### Ramas

- **`main`**: Producción (Intocable directamente).
- **`dev` o `staging`**: Pruebas.
- **`feat/nombre-feature`**: Para nuevas funcionalidades.
- **`fix/nombre-bug`**: Para correcciones.

### Commits (Conventional Commits)

**Formato:** `tipo(scope): descripción breve`

**Tipos permitidos:**
- `feat`: Nueva funcionalidad.
- `fix`: Corrección de bug.
- `docs`: Cambios en documentación.
- `chore`: Mantenimiento (dependencias, configs).
- `refactor`: Cambio de código que no altera funcionalidad.

**Ejemplo:** `feat(auth): implement JWT login strategy`

---

## 📝 6. Documentación

### README.md
Todo repositorio debe tener un README con:
- Qué hace el proyecto.
- Prerrequisitos (Node version, API Keys necesarias).
- Cómo instalar (`npm install`) y correr (`npm run dev`).

### Código
Comentar el **"Por qué"**, no el **"Qué"**. El código dice qué hace; el comentario explica la decisión compleja.

---

## 🚀 CÓMO APLICAR ESTA CONSTITUCIÓN

1. Ve a tu repositorio `documentation-hub`.
2. Crea un archivo nuevo llamado `ECOSYSTEM-STANDARDS.md`.
3. Pega todo el contenido de arriba (incluyendo los encabezados Markdown).
4. Haz Commit.

Una vez guardado, tu "Gem Auditor" (el workflow que creamos antes) leerá este archivo automáticamente y empezará a juzgar a tus proyectos bajo estas leyes.

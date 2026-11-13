# Política de repositorios y publicación responsable

**Versión:** propuesta v0.1, 12 de noviembre de 2026. Su aplicación a repositorios existentes requiere revisión caso por caso.

## Principio

**Publicar el conocimiento útil; preservar la ventaja comercial y la privacidad.** La visibilidad pública de un repositorio es una decisión diferente de la licencia de uso del código.

### Preferentemente público

- Descripciones de producto, ejemplos sintéticos, capturas sin datos de clientes, documentación de API pública y tutoriales generales.
- Plantillas y librerías **deliberadamente liberadas**, con licencia explícita y revisión de dependencias/derechos.
- Resultados de pruebas que no revelen credenciales, dominios internos, rutas privadas ni datos sensibles.
- Casos de uso genéricos con métodos transparentes, sin prometer resultados no validados.

### Preferentemente privado

- Motores propietarios cuya publicación permita replicar diferenciación comercial, algoritmos estratégicos y configuraciones internas.
- Datos reales de clientes, archivos comerciales no publicados, precios negociados, contratos, prospectos y sus contactos no destinados a difusión.
- Esquemas completos de autorización, auditorías de vulnerabilidades no resueltas y código de seguridad aún no revisado.
- Respaldos de Hostinger, entornos de despliegue, bases de datos, reportes confidenciales y código con licencias de terceros por verificar.

### Nunca subir a Git, ni siquiera privado

- Contraseñas, tokens, claves API, llaves privadas, cookies, archivos `.env` con secretos, credenciales de Hostinger/MySQL o información que no se tiene autorización de almacenar.
- Historial de invitaciones con tokens o registros con secretos incrustados.

Usar secretos administrados por GitHub Actions o por el proveedor, con permisos mínimos y rotación. Un `.gitignore` evita nuevas inclusiones accidentales, **pero no elimina secretos ya presentes en el historial**: si se detectan, revocar y rotar inmediatamente.

## Antes de hacer público un repositorio

1. Revisar **todo el historial Git**, no solo la última versión, buscando secretos y datos de terceros.
2. Examinar dependencias y licencias de código/datasets/modelos.
3. Confirmar que el código público no contenga componentes esenciales que se quiera licenciar exclusivamente.
4. Preparar un README legible, un `SECURITY.md` y una licencia elegida explícitamente si se otorgarán derechos de reutilización.
5. Comprobar que no existan workflows inseguros ni configuraciones de publicación automática no deseadas.
6. Revisar el dif y obtener aprobación explícita antes de cambiar visibilidad o desplegar.

**No se debe aplicar automáticamente la regla “todo público” a repositorios existentes.** Primero auditoría, después decisión.

## Flujo normal de desarrollo

- `main` protegida; trabajo en ramas y PR; al menos una revisión significativa en cambios sensibles.
- CI mínimo: lint, tests, escaneo de dependencias y de secretos cuando se habiliten y no comprometan datos.
- Versiones publicadas con tag/release y bitácora; despliegue separado y reversible.
- `CODEOWNERS` solo cuando se acuerden revisores reales.
- Documentar qué se implementó, probó, publicó y validó comercialmente.

## Modelo recomendado de repositorios

- **Índice público:** `documentation-hub` (este repositorio).
- **Productos:** conservar AquaEye y gl-strategic-engine como repositorios de producto actuales; evaluar cuidadosamente qué partes ya públicas ameritan protección futura.
- **Dirección interna:** crear un **repositorio privado dedicado** para decisiones, prospectos y actas, sin mezclarlo con los proyectos existentes.
- **Seguridad de demos:** desarrollo aislado en un repositorio privado, sin copiar carpetas a la raíz corporativa.
- **Respaldos:** mantener privados; separar el control de versiones del mecanismo de recuperación operativa.

Cambiar un repositorio público a privado **no revoca automáticamente forks, clones o copias anteriores**. Revisar exposición previa y permisos de explotación comercial antes de decidir.

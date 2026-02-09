# Registro de Cambios - Plataforma Servicios Profesionales GL

## Versión 1.0 - Febrero 2026

### Normalización general de documentación

- Se unificó el tono de toda la documentación a registro institucional y técnico.
- Se eliminaron formulaciones en segunda persona ("vos", "tu", "podés") y se reescribieron en forma impersonal o descriptiva.
- Se eliminaron secciones de carácter motivacional, tutorial o coaching ("consejos", "tips", "celebrate small wins", "no presionarse", etc.).
- Se eliminaron emojis de todos los documentos.
- Se eliminaron referencias a herramientas internas de desarrollo no relevantes para la documentación entregable.

### Estructura de costos

- Se eliminaron todos los costos de desarrollo como gasto directo. El desarrollo se realiza ad honorem.
- Se mantienen exclusivamente los costos de infraestructura en USD (servicios cloud, dominio, email, monitoreo).
- Las referencias a equipos de desarrollo se reformularon como "referencia de mercado" (dimensionamiento comparativo), sin sugerir contratación.

### Planificación temporal

- Se ajustó el horizonte del MVP Web a 12 meses (~400 horas acumuladas a 8 horas semanales).
- Se movió explícitamente la aplicación mobile al Año 2 del proyecto.
- Se calibraron las horas por etapa para mantener coherencia con el timeline de 1 año.

### Créditos y autoría

- Se normalizó la sección de créditos en todos los documentos con la estructura estándar:
  - Idea original: Homero Bonafert y Mario Garberoglio
  - Desarrollo técnico y conceptualización: Juan Martin Marina

### Formato y presentación

- Se corrigieron tablas Markdown para correcta visualización en preview y en Word.
- Se generaron archivos .docx sincronizados con cada .md mediante conversión automatizada (Markdown -> HTML -> Word con ajuste de tablas).
- Se creó script de conversión reutilizable en `tools/md_to_docx.py`.

### Archivos modificados

| Archivo | Tipo de cambio |
|---------|----------------|
| PLAN_DESARROLLO_SOLO.md | Reescritura completa de tono |
| BOCETO_APP_GL.md | Correcciones de segunda persona y coaching |
| PLAN_ACCION_PRESUPUESTO.md | Ajuste de costos y timeline |
| PRESUPUESTO_REAL_FINAL.md | Eliminación de segunda persona |
| RESUMEN_EJECUTIVO_GL.md | Ajustes menores de tono |
| ARQUITECTURA_OPTIMIZADA_10K.md | Normalización de justificaciones |
| ARQUITECTURA_DETALLADA.md | Sin cambios (ya estaba normalizado) |
| NOTA_IMPORTANTE_AD_HONOREM.md | Sin cambios (ya estaba normalizado) |
| STACK_TECNOLOGICO_RESUMEN.md | Sin cambios (ya estaba normalizado) |
| README.md | Eliminación de emojis y ajuste de créditos |

---

**Versión**: 1.0
**Fecha**: Febrero 2026

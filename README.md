# Ecosistema de Automatización IA — Clasificación de Leads VIP

Entrega Final del curso de Automatización con IA. Sistema autónomo de extremo a extremo que recibe leads desde un formulario web, los clasifica con IA (VIP / Estándar / Descartar), los registra en una base de datos y — solo para los leads VIP — incorpora un punto de aprobación humana (Human-in-the-loop) antes de notificar al lead por email.

## Stack

| Categoría | Tecnología |
|---|---|
| Orquestador | [n8n](https://n8n.io) — workflow "Corazon - Clasificacion Leads VIP" |
| Base de datos | [Airtable](https://airtable.com) — base "Ecosistema IA - Clasificación de Leads VIP" (3 tablas vinculadas) |
| Procesamiento IA | OpenAI GPT-4o-mini, con prompt estructurado (ver `docs/`) |
| Canal de salida | Slack (aprobación humana VIP) + Gmail (confirmación al lead) |

## Contenido del repositorio

```
docs/
  Entrega-Final-Ecosistema-IA-Leads-VIP.pdf   → los 4 documentos de la entrega en un solo PDF:
                                                  1. Diagrama de arquitectura
                                                  2. Manual operativo de datos (esquema + JSON)
                                                  3. Matriz de costos y modelos de IA
                                                  4. Seguridad y resiliencia
n8n/
  corazon-workflow.json                        → export del workflow n8n (blueprint técnico)
screenshots/
  01-n8n-workflow-completo.jpg                 → evidencia: workflow completo (13 nodos)
  02-airtable-leads-table.jpg                  → evidencia: tabla Leads
  03-airtable-errores-table.jpg                → evidencia: tabla Errores
  04-nodo-ia-clasificacion.jpg                 → evidencia: nodo de clasificación con IA (input real)
```

## Enlaces obligatorios

- **PDF (arquitectura + documentación):** [`docs/Entrega-Final-Ecosistema-IA-Leads-VIP.pdf`](docs/Entrega-Final-Ecosistema-IA-Leads-VIP.pdf)
- **Lógica del flujo (JSON n8n):** [`n8n/corazon-workflow.json`](n8n/corazon-workflow.json)
- **Base de datos en modo lectura (Airtable, tabla Leads, agrupada por Estado):** https://airtable.com/appEYflVUSmKkVota/shrdtXrsHKPL4OQ3C
- **Dashboard de Control — tasa de errores (Airtable, tabla Errores, agrupada por Tipo_Error):** https://airtable.com/appEYflVUSmKkVota/shr65wjZkxCLoEq9v
- **Screenshots de evidencia:** carpeta [`screenshots/`](screenshots/)

> Nota sobre el dashboard: Airtable solo permite compartir públicamente páginas de *Interfaces* (paneles con gráficos) en su plan de pago Team. Con la cuenta gratuita usada en este proyecto, el "Dashboard de Control" se implementó como dos vistas Grid públicas y agrupadas (Leads por Estado, Errores por Tipo_Error) con barra de resumen (conteos), que cumplen el mismo rol de panel de KPIs en modo lectura sin requerir upgrade de plan.

## Arquitectura (resumen)

`Formulario Web → Validar Datos Completos → Clasificar Lead con IA (OpenAI GPT-4o-mini) → Parsear Respuesta IA → Crear/Actualizar Lead (Airtable) → ¿Es VIP? → [VIP] Aprobación Humana (Slack) → ¿Fue Aprobado? → Actualizar Lead + Email Confirmación (Gmail)`

Ruta de error dedicada: datos incompletos en el formulario se registran en la tabla `Errores` sin perder el intento. Detalle completo, diagrama visual y los 5 payloads JSON de transferencia entre nodos están en el PDF.

## Autor

Manuel Sánchez — Septiembre 2026

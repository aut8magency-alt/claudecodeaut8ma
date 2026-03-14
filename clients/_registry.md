# aut8ma Client Registry

**Última actualización**: 2026-03-14
**Total de clientes activos**: 0
**Cupos disponibles este mes**: 10

---

## Clientes Activos

| ID | Slug | Cliente | Plan | Estado Automation | Estado Video | Fecha Inicio | Próxima Revisión |
|----|------|---------|------|-------------------|--------------|-------------|------------------|
| — | — | *(sin clientes aún)* | — | — | — | — | — |

---

## Pipeline de Automation — Estados

| Estado | Descripción |
|--------|-------------|
| `—` | No solicitado |
| `intake` | Notas de reunión recibidas, INTAKE procesando |
| `architecture` | Lista de nodos siendo diseñada por NEXARCH |
| `json` | JSON siendo generado por JSONCRAFT |
| `prompt-b` | System prompt siendo escrito por SCRIBE |
| `qa` | Validación QA en progreso por QA-GATE |
| `qa-revision-1` | Retornado para revisión (1ra vez) |
| `qa-revision-2` | Retornado para revisión (2da vez) |
| `delivered` | Documento final entregado a Tavo ✅ |
| `on-hold` | En espera — falta info del cliente |

## Pipeline de Video — Estados

| Estado | Descripción |
|--------|-------------|
| `—` | No solicitado |
| `briefed` | Brief de video recibido |
| `reference-analysis` | LENS analizando video de referencia |
| `blueprint` | BLUEPRINT generando blueprint |
| `resources` | SCOUT buscando assets |
| `capcut-config` | CONFIGURA generando config técnica |
| `delivered` | Paquete de video entregado a Tavo ✅ |
| `on-hold` | En espera — falta guión o assets del cliente |

---

## Instrucciones para Agregar un Cliente Nuevo

1. Crear carpeta: `clients/CLIENT-[NNN]-[slug]/`
2. Crear archivo: `clients/CLIENT-[NNN]-[slug]/profile.md`
3. Agregar fila en la tabla de arriba con estado `intake`
4. Decirle a ARIA: `"ARIA, cliente nuevo CLIENT-[NNN], intake: [notas de la reunión]"`

### Formato del Slug
- Todo en minúsculas
- Solo guiones (sin underscores, sin espacios)
- Sin acentos ni caracteres especiales
- Máximo 20 caracteres
- Derivado del nombre del negocio

**Ejemplos:**
- "Acme Venezuela" → `acme-venezuela`
- "Farmacia del Norte" → `farmacia-norte`
- "Clínica Sol Naciente" → `clinica-sol-naciente`

---

## Resumen por Plan

| Plan | Clientes Activos | Ingresos Mensuales |
|------|-----------------|-------------------|
| Emprendedor ($69+/mes) | 0 | $0 |
| Negocio ($129+/mes) | 0 | $0 |
| Premium ($199+/mes) | 0 | $0 |
| **TOTAL** | **0** | **$0** |

---

## Historial de Clientes Entregados

*(vacío — primer registro)*

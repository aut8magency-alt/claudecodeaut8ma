# aut8ma — Arquitectura Técnica del Sistema Multi-Agente

> Versión 1.0 · Venezuela 2026

---

## Visión General

El sistema opera dos equipos completamente separados dentro del mismo repositorio:

```
┌─────────────────────────────────────────────────────────────┐
│                    aut8ma Multi-Agent System                │
│                                                             │
│  ┌─────────────────────────┐  ┌─────────────────────────┐  │
│  │    AUTOMATION TEAM      │  │      VIDEO TEAM          │  │
│  │                         │  │                          │  │
│  │  ARIA (orchestrator)    │  │  VORA (orchestrator)     │  │
│  │  INTAKE                 │  │  LENS (analyzer)         │  │
│  │  NEXARCH (architect)    │  │  BLUEPRINT               │  │
│  │  JSONCRAFT (builder)    │  │  SCOUT (resources)       │  │
│  │  SCRIBE (prompt writer) │  │  CONFIGURA (capcut)      │  │
│  │  QA-GATE                │  │                          │  │
│  │  PACKAGER               │  └─────────────────────────┘  │
│  │                         │              │                 │
│  │  Sub-agents:            │         (aislados)             │
│  │  Maxwell, Nexus,        │                               │
│  │  Atlas, Victor,         │  ┌─────────────────────────┐  │
│  │  Sentinel, Director     │  │   SHARED STATE           │  │
│  │         │               │  │                          │  │
│  └─────────┼───────────────┘  │  clients/_registry.md    │  │
│            └──────────────────►  clients/CLIENT-XXX/      │  │
│                               │  profile.md               │  │
│                               └─────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
```

---

## Sección 1: Automation Team

### 1.1 Pipeline Completo

```
TAVO INPUT
    │
    ▼
auto-orchestrator (ARIA)
    │── lee clients/_registry.md
    │── identifica cliente o crea entrada nueva
    │
    ├─ CLIENTE NUEVO ────────────────────────────────────────┐
    │                                                        │
    ▼                                                        │
auto-intake                                                  │
    │── recibe notas de reunión desordenadas                │
    │── genera Prompt A (contexto estructurado)             │
    │── genera semilla de Prompt B                          │
    │── llama sub-agentes si aplica:                        │
    │     ├─ sub-maxwell (si chatbot de marketing)          │
    │     ├─ sub-victor (si chatbot de ventas)              │
    │     └─ sub-atlas (si necesita propuesta comercial)    │
    │                                                        │
    ▼                                                        │
auto-architect (NEXARCH)                                     │
    │── lee knowledge/n8n-node-library.md                  │
    │── lee knowledge/aut8ma-system-prompt.md Block 5.2    │
    │── genera lista COMPLETA de nodos (antes de JSON)      │
    │── organiza en secciones A-I                           │
    │── define qué variables viajan entre nodos             │
    │                                                        │
    ▼                                                        │
auto-json-builder (JSONCRAFT)                                │
    │── recibe lista de nodos del arquitecto               │
    │── genera JSON Parte 1/N ... N/N                      │
    │── máximo 15 nodos por parte                          │
    │── cada parte es importable en N8N independientemente  │
    │── llama sub-sentinel si hay datos sensibles           │
    │                                                        │
    ▼                                                        │
auto-prompt-writer (SCRIBE)                                  │
    │── lee Prompt A del cliente                            │
    │── lee knowledge/aut8ma-system-prompt.md Block 4-7    │
    │── genera Prompt B completo (system prompt chatbot)    │
    │── llama sub-nexus si se necesita ROI                  │
    │── llama sub-director si se necesita guión de video    │
    │                                                        │
    ▼                                                        │
auto-qa-validator (QA-GATE)           ◄────────────────────┐ │
    │── lee knowledge/qa-checklist-block53.md              │ │
    │── valida arquitectura, JSON y Prompt B               │ │
    │── genera QA report con pass/fail por ítem            │ │
    │                                                        │ │
    ├─ FAIL ──────────────────────────────────────────────► │ │
    │   (retorna al agente originador con instrucciones)    │ │
    │                                                        │ │
    └─ PASS ──────────────────────────────────────────────► │ │
    │                                                        │ │
    ▼                                                        │ │
auto-delivery-formatter (PACKAGER)                           │ │
    │── lee knowledge/delivery-template.md                 │ │
    │── ensambla documento final de entrega                 │ │
    │── entrega markdown completo en chat                   │ │
    │                                                        │ │
    ▼                                                        │ │
auto-orchestrator                                            │ │
    └── actualiza clients/_registry.md → status: delivered  │ │
                                                             │ │
    ◄────────────────────────────────────────────────────────┘ │
    (revisión reintegra al pipeline en el nodo correcto)      │
```

### 1.2 Inventario de Agentes — Automation Team

#### Core Agents

| Agente | Nombre | Inputs | Outputs | Knowledge que lee |
|--------|--------|--------|---------|-------------------|
| `auto-orchestrator` | ARIA | Solicitud de Tavo + `_registry.md` | Delegación al agente correcto | `clients/_registry.md` |
| `auto-intake` | INTAKE | Notas de reunión desordenadas | Prompt A (contexto) + semilla Prompt B | `knowledge/plans-catalog.md` |
| `auto-architect` | NEXARCH | Prompt A + plan del cliente | Lista numerada de nodos N8N | `knowledge/n8n-node-library.md`, `knowledge/aut8ma-system-prompt.md` Block 5.2 |
| `auto-json-builder` | JSONCRAFT | Lista de nodos del arquitecto | JSON en partes (máx 15 nodos c/u) | `knowledge/n8n-json-schema.md` |
| `auto-prompt-writer` | SCRIBE | Prompt A + perfil cliente | Prompt B completo (system prompt) | `knowledge/aut8ma-system-prompt.md` Blocks 4, 7 |
| `auto-qa-validator` | QA-GATE | Cualquier output del pipeline | QA report (pass/fail por ítem) | `knowledge/qa-checklist-block53.md` |
| `auto-delivery-formatter` | PACKAGER | Todos los outputs verificados | Documento markdown final en chat | `knowledge/delivery-template.md` |

#### Sub-Agents (Especialistas)

| Agente | Persona | Rol | Quién lo llama |
|--------|---------|-----|----------------|
| `sub-maxwell` | Maxwell | Marketing: copies, campañas TikTok/Instagram/LinkedIn, voz de marca | auto-intake (si chatbot marketing) |
| `sub-nexus` | Nexus | ROI, costos, márgenes, pricing, análisis financiero | auto-prompt-writer (si propuesta) |
| `sub-atlas` | Atlas | Propuestas comerciales, contratos, reportes ejecutivos | auto-intake (si se pide propuesta) |
| `sub-victor` | Victor | Scripts de cierre, manejo de objeciones Venezuela, BANT | auto-intake (si chatbot ventas) |
| `sub-sentinel` | Sentinel | Auditoría de seguridad: JSON, servidor, credenciales, SSL | auto-json-builder (si datos sensibles) |
| `sub-director` | Director | Guiones de video: hook 0-3s, problema, solución, prueba, CTA | auto-prompt-writer (si cliente pide video) |

---

## Sección 2: Video Team

### 2.1 Pipeline Completo

```
TAVO INPUT (brief de video + referencia opcional)
    │
    ▼
video-orchestrator (VORA)
    │── lee clients/[CLIENT]/profile.md
    │── identifica tipo de contenido (reel, short, TikTok)
    │
    ▼
video-reference-analyzer (LENS)
    │── recibe descripción o URL de video de referencia
    │── extrae fingerprint de estilo:
    │     ├─ frecuencia de cortes
    │     ├─ tipo de subtítulos y animaciones
    │     ├─ uso de B-roll y pantalla dividida
    │     ├─ efectos de sonido por tipo de momento
    │     ├─ paleta de colores y texto
    │     └─ energía general y ritmo
    │── output: documento "Style Fingerprint"
    │
    ▼
video-blueprint-generator (BLUEPRINT)
    │── recibe guión + Style Fingerprint
    │── lee knowledge/video-editor-prompt.md (estilo Víctor Heras)
    │── genera blueprint segmento por segmento:
    │     ├─ tiempo exacto de inicio y fin
    │     ├─ texto del guión para ese segmento
    │     ├─ subtítulos (tarjeta por tarjeta, color hex, animación)
    │     ├─ toma principal (encuadre, movimiento, fondo)
    │     ├─ B-rolls con duración y función narrativa
    │     ├─ efectos de sonido con timestamp
    │     └─ música (volumen, cambios de energía)
    │
    ▼
video-resources-finder (SCOUT)
    │── recibe blueprint completo
    │── genera lista de recursos necesarios:
    │     ├─ B-rolls: descripción + keywords CapCut/Pexels/Pixabay
    │     ├─ SFX: nombre del efecto + fuente gratuita
    │     ├─ música: género + keywords Epidemic Sound/Pixabay Music
    │     └─ fuentes tipográficas (Google Fonts)
    │
    ▼
video-capcut-configurator (CONFIGURA)
    │── lee knowledge/capcut-settings-reference.md
    │── genera configuración técnica para CapCut:
    │     ├─ resolución: 1080×1920 (9:16 vertical)
    │     ├─ FPS: 30
    │     ├─ configuración de subtítulos (fuente, tamaño, stroke, sombra)
    │     ├─ orden de capas del timeline
    │     ├─ ajustes de color (contraste, saturación, temperatura)
    │     └─ parámetros de exportación
    │
    ▼
video-orchestrator (VORA)
    └── ensambla los 4 outputs en un documento markdown único en chat
```

### 2.2 Inventario de Agentes — Video Team

| Agente | Nombre | Inputs | Outputs | Knowledge que lee |
|--------|--------|--------|---------|-------------------|
| `video-orchestrator` | VORA | Brief de video + nombre cliente | Ensamblado del documento final | `clients/[CLIENT]/profile.md` |
| `video-reference-analyzer` | LENS | Descripción/URL de video referencia | Style Fingerprint document | `knowledge/video-editor-prompt.md` |
| `video-blueprint-generator` | BLUEPRINT | Guión + Style Fingerprint | Blueprint segmento por segmento | `knowledge/video-editor-prompt.md` |
| `video-resources-finder` | SCOUT | Blueprint completo | Lista de assets con keywords CapCut | — |
| `video-capcut-configurator` | CONFIGURA | Blueprint + requerimientos | Config técnica CapCut completa | `knowledge/capcut-settings-reference.md` |

---

## Sección 3: Estado de Clientes

### 3.1 Registro Maestro (`clients/_registry.md`)

Tabla con todos los clientes activos. Los orquestadores (ARIA y VORA) leen este archivo al inicio de cada tarea para saber el estado actual.

### 3.2 Carpeta por Cliente (`clients/CLIENT-XXX-[slug]/`)

```
clients/CLIENT-001-acme-caracas/
├── profile.md        ← Perfil del cliente (campos estándar)
├── intake.md         ← Prompt A generado por auto-intake
├── architecture.md   ← Lista de nodos generada por auto-architect
├── workflow-draft.md ← JSON chunks acumulados de auto-json-builder
├── prompt-b.md       ← System prompt generado por auto-prompt-writer
├── qa-report.md      ← Reporte de QA de auto-qa-validator
└── delivery.md       ← Documento final de entrega (delivery-formatter)
```

### 3.3 Nomenclatura de Clientes

```
Format:  CLIENT-[NNN]-[slug]
Ejemplo: CLIENT-001-acme-caracas
         CLIENT-023-farmacia-norte

Reglas del slug:
- Todo en minúsculas
- Solo guiones (sin underscores, sin espacios)
- Sin acentos ni caracteres especiales
- Máximo 20 caracteres
- Derivado del nombre del negocio
```

### 3.4 Valores de Estado

**Automation Pipeline:**
```
intake → architecture → json → prompt-b → qa → qa-revision-1 → ... → delivered | on-hold
```

**Video Pipeline:**
```
briefed → reference-analysis → blueprint → resources → capcut-config → delivered | on-hold
```

---

## Sección 4: Archivos de Conocimiento (knowledge/)

### Qué contiene cada archivo y quién lo lee

| Archivo | Contenido | Quién lo lee | Frecuencia |
|---------|-----------|-------------|------------|
| `aut8ma-system-prompt.md` | Bloques 1-12 completos del system prompt maestro | auto-architect, auto-prompt-writer, auto-intake | Siempre |
| `video-editor-prompt.md` | Guía de estilo Víctor Heras completa | Todos los agentes de video | Siempre |
| `n8n-node-library.md` | Catálogo de nodos N8N con parámetros exactos | auto-architect, auto-json-builder | Siempre |
| `n8n-json-schema.md` | Reglas de JSON válido para N8N (estructura, conexiones, UUIDs) | auto-json-builder | Siempre |
| `plans-catalog.md` | Emprendedor/Negocio/Premium: features, SLA, precios | auto-intake, sub-atlas, sub-nexus | Por cliente |
| `qa-checklist-block53.md` | Checklist Block 5.3 aislado y numerado | auto-qa-validator (exclusivo) | Siempre |
| `delivery-template.md` | Template del documento final de entrega | auto-delivery-formatter | Siempre |
| `capcut-settings-reference.md` | Presets CapCut: export, subtítulos, capas, color | video-capcut-configurator | Siempre |

---

## Sección 5: Reglas de Aislamiento entre Equipos

```
AUTOMATION TEAM                    VIDEO TEAM
      │                                 │
      │    ┌─────────────────────┐      │
      │    │   SHARED STATE      │      │
      ├───►│  _registry.md       │◄─────┤
      ├───►│  CLIENT/profile.md  │◄─────┤
      │    └─────────────────────┘      │
      │                                 │
      │    sub-director genera guiones  │
      │    que TAVO pasa manualmente    │
      └──────────────────────────────── ┘
               (puente manual)
```

**Reglas absolutas:**
1. Automation agents NO llaman a video agents (y viceversa)
2. El único estado compartido es `_registry.md` y `profile.md`
3. `sub-director` es el único puente: genera guiones de video pero Tavo los pasa manualmente al Video Team
4. Si un cliente necesita automatización Y video, se crean dos tareas separadas en los dos orquestadores

---

## Sección 6: Template de Archivo de Agente

Todo archivo en `.claude/agents/` sigue este formato:

```markdown
---
name: [nombre-display-del-agente]
description: [Una oración clara: cuándo Claude Code debe invocar este agente. Ej: "Usar cuando se necesite procesar notas de reunión de un cliente nuevo y generar el Prompt A de automatización"]
tools: [Read, Write]
---

# [NOMBRE EN MAYÚSCULAS] — [Team: Automation | Video]

## Identidad y Misión
Eres [NOMBRE], [rol]. Trabajas para aut8ma, la agencia de chatbots con IA #1 de Venezuela.
Tu misión específica es: [qué hace este agente exactamente].

## Inputs que Recibes
- **[Input 1]**: [formato y origen]
- **[Input 2]**: [formato y origen]

## Archivos que Debes Leer Antes de Empezar
- `knowledge/[archivo-relevante].md` — [por qué]
- `clients/[CLIENT]/profile.md` — para contexto del cliente

## Proceso Paso a Paso

### Paso 1: [Nombre de la acción]
[Instrucción detallada]

### Paso 2: [Nombre de la acción]
[Instrucción detallada]

### Paso N: [Nombre de la acción]
[Instrucción detallada]

## Formato de Output

Tu output es SIEMPRE un documento markdown entregado en el chat.
NUNCA guardes archivos directamente. NUNCA truncues código o JSON.

### Estructura del documento:
---
# [Título del documento]: [Nombre del Cliente] — [Fecha]

## [Sección 1]
[contenido]

## [Sección 2]
[contenido]
---

## Instrucción de Handoff
Al final de tu output, incluye SIEMPRE:
```
HANDOFF → [nombre del siguiente agente]: [qué debe recibir y hacer]
```

## Reglas de este Agente
- [Regla 1]
- [Regla 2]
- Nunca produzcas output más largo del necesario
- Responde desde la primera palabra, sin introducción ni relleno
```

---

## Sección 7: Variables de Entorno

Ver `.env.example` para lista completa. Mínimo para operar el sistema:

```bash
ANTHROPIC_API_KEY=    # Para invocar Claude Code y los agentes
```

Para integración futura con N8N directo (actualmente no implementada):
```bash
N8N_BASE_URL=         # URL del N8N self-hosted
N8N_API_KEY=          # API key de N8N
```

Para notificaciones internas (opcional):
```bash
TELEGRAM_BOT_TOKEN=
TELEGRAM_CHAT_ID=
```

---

## Sección 8: Convenciones de Nombres

| Tipo | Formato | Ejemplo |
|------|---------|---------|
| Agente core Automation | `auto-[rol].md` | `auto-orchestrator.md` |
| Sub-agente especialista | `sub-[nombre].md` | `sub-maxwell.md` |
| Agente Video | `video-[rol].md` | `video-blueprint-generator.md` |
| Carpeta de cliente | `CLIENT-[NNN]-[slug]/` | `CLIENT-001-acme-caracas/` |
| Archivo knowledge | `[dominio]-[descriptor].md` | `n8n-node-library.md` |
| Template | `[tipo]-template.md` | `intake-template.md` |

---

## Sección 9: Orden de Creación de Agentes

```
Fase 1 — Foundation (ya creado en este repo):
  ✓ knowledge/ (8 archivos)
  ✓ templates/ (7 archivos)
  ✓ clients/_registry.md
  ✓ CLAUDE.md, README.md, ARCHITECTURE.md, .env.example

Fase 2 — Automation Core (tú los creas con Claude Code CLI):
  1. auto-orchestrator.md  ← PRIMERO: routing logic depende de él
  2. auto-intake.md
  3. auto-architect.md
  4. auto-json-builder.md
  5. auto-prompt-writer.md
  6. auto-qa-validator.md
  7. auto-delivery-formatter.md

Fase 3 — Automation Sub-Agents:
  8.  sub-maxwell.md
  9.  sub-nexus.md
  10. sub-atlas.md
  11. sub-victor.md
  12. sub-sentinel.md
  13. sub-director.md

Fase 4 — Video Team:
  14. video-orchestrator.md  ← PRIMERO de video
  15. video-reference-analyzer.md
  16. video-blueprint-generator.md
  17. video-resources-finder.md
  18. video-capcut-configurator.md
```

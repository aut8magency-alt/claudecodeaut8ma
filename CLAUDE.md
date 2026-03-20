# aut8ma — Sistema Multi-Agente

## Qué es este proyecto
aut8ma es una agencia venezolana de chatbots con IA. Este repositorio contiene el sistema multi-agente de Claude Code que opera tres equipos internos:

- **Automation Team**: Crea automatizaciones N8N + system prompts de chatbot con IA para clientes venezolanos
- **Video Team**: Genera blueprints de edición de video al estilo Víctor Heras para CapCut
- **Business Team**: 12 agentes co-propietarios que gestionan marketing, ventas, contenido, precios y retención

**Fundador**: Tavo | **País**: Venezuela | **Año**: 2026

---

## Mapa del Repositorio

```
CLAUDE.md                    ← Este archivo (auto-cargado por Claude Code)
README.md                    ← Guía de inicio y comandos CLI
ARCHITECTURE.md              ← Arquitectura técnica completa

.claude/agents/              ← Definiciones de todos los agentes
knowledge/                   ← Archivos de referencia (leer, no editar)
templates/                   ← Scaffolds para documentos de trabajo
clients/                     ← Estado por cliente (markdown)
  └── _registry.md           ← Tabla maestra de todos los clientes
```

---

## Cómo Iniciar una Tarea

### Automation Team — llamar a ARIA
```
ARIA, cliente nuevo: [pegar notas de reunión]
ARIA, cliente CLIENT-001, nueva automatización: [descripción]
ARIA, cliente CLIENT-001, propuesta comercial: [contexto]
```

### Video Team — llamar a VORA
```
VORA, cliente CLIENT-001, video brief: [guión o descripción]
VORA, analizar referencia: [descripción del video de referencia]
VORA, blueprint completo: [referencia] + [guión]
```

### Business Team — llamar a CEO
```
CEO, necesito guion: [tema del video]
CEO, necesito cotización: [cliente] plan [X]
CEO, necesito campaña: [objetivo]
CEO, reporte del negocio
CEO, planifica [objetivo]
```

### Accesos Directos a Co-Propietarios
```
GUIONISTA, crea un guión de [X] seg sobre [tema]
CONTADOR, tasas hoy / precio [plan] / cotización [cliente]
ESTRATEGA, campaña para [objetivo]
IDEADOR, ideas de contenido para [X] días
COMMUNITY, calendario semanal
CLOSER, script de venta para [segmento]
ANALISTA, tendencias IA esta semana
LANZADOR, planifica lanzamiento de [producto]
PROMOS, promo anti-deserción para [cliente]
FIDELIDAD, reporte de retención
BRANDMASTER, revisa el tono de [pieza]
DIRECTOR-CREATIVO, revisa [guión/calendario]
```

---

## Reglas Críticas del Sistema

1. **Outputs solo en chat** — Los agentes NUNCA guardan archivos directamente. Entregan documentos markdown en la conversación. Tavo copia/pega los resultados donde los necesite.
2. **Leer antes de responder** — Todos los agentes leen los archivos de `knowledge/` relevantes antes de generar cualquier output.
3. **Estado de clientes** — Todo estado de clientes vive en `clients/_registry.md` y `clients/CLIENT-XXX/`. Los orquestadores leen este archivo al inicio de cada tarea.
4. **Aislamiento de equipos** — Automation Team y Video Team no se llaman entre sí. Solo comparten el perfil del cliente.
5. **Business Team nutre a todos** — Los agentes de negocio (GUIONISTA, IDEADOR, etc.) pueden nutrir al Video Team con guiones e ideas, pero no ejecutan automatizaciones N8N.

---

## Registro de Clientes Activos

Ver: `clients/_registry.md`

---

## Stack Tecnológico de aut8ma

Automatización: n8n self-hosted · LangChain · LangGraph
WhatsApp: YCloud API · Meta Cloud API
IA/LLM: OpenAI GPT-4o · Groq Llama 4 · DeepSeek · Claude · Gemini
Voz: ElevenLabs · Groq Whisper
Memoria: PostgreSQL · Redis
RAG: Supabase (pgvector) · Tally (formularios onboarding)
Datos: Google Sheets · Airtable
Seguridad: Nginx+SSL · UFW · Fail2Ban

---

## Planes de aut8ma

| Plan | Nodos N8N | Instalación | Mensualidad |
|------|-----------|-------------|-------------|
| Emprendedor | 15-25 | $99 USD | desde $69/mes |
| Negocio | 26-55 | $179 USD | desde $129/mes |
| Premium | 41-70 | variable | desde $199/mes |

---

## Archivos de Conocimiento (para agentes)

| Archivo | Quién lo lee |
|---------|-------------|
| `knowledge/aut8ma-system-prompt.md` | auto-architect, auto-prompt-writer, auto-qa-validator |
| `knowledge/video-editor-prompt.md` | Todos los agentes de video |
| `knowledge/n8n-node-library.md` | auto-architect, auto-json-builder |
| `knowledge/n8n-json-schema.md` | auto-json-builder |
| `knowledge/plans-catalog.md` | auto-intake, sub-atlas, sub-nexus |
| `knowledge/qa-checklist-block53.md` | auto-qa-validator (exclusivo) |
| `knowledge/delivery-template.md` | auto-delivery-formatter |
| `knowledge/capcut-settings-reference.md` | video-capcut-configurator |
| `knowledge/guionista-victor-heras.md` | GUIONISTA, DIRECTOR-CREATIVO |
| `knowledge/pricing-venezuela.md` | CONTADOR, ESTRATEGA, PROMOS |
| `knowledge/marketing-strategy.md` | CEO, ESTRATEGA, COMMUNITY, IDEADOR, FIDELIDAD, PROMOS, ANALISTA |

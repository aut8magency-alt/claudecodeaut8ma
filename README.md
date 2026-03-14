# aut8ma — Sistema Multi-Agente Claude Code

> Sistema de agentes de IA para la agencia aut8ma (Venezuela 2026)
> Automation Team (N8N) + Video Team (CapCut)

---

## Descripción del Proyecto

Este repositorio contiene las **foundations** del sistema multi-agente que potencia las operaciones internas de aut8ma:

- **Automation Team**: 7 agentes core + 6 sub-agentes especializados que crean automatizaciones N8N completas (hasta 70 nodos), system prompts de chatbot IA y propuestas comerciales
- **Video Team**: 5 agentes que generan blueprints de edición al estilo Víctor Heras, listas de recursos y configuraciones técnicas para CapCut

Escala diseñada para: **20+ clientes simultáneos**

---

## Requisitos Previos

- [Claude Code CLI](https://github.com/anthropics/claude-code) instalado
- Node.js 18+ (para Claude Code)
- Cuenta Anthropic con acceso a Claude API
- Git

---

## Configuración Inicial

### 1. Clonar e inicializar el proyecto

```bash
cd "C:\Users\Gabriel Hung\claudecode"
cp .env.example .env
# Editar .env con tus API keys reales
```

### 2. Verificar la estructura

```bash
# Debe mostrar: CLAUDE.md, README.md, ARCHITECTURE.md, knowledge/, templates/, clients/, .claude/
ls
```

### 3. Abrir Claude Code en este directorio

```bash
claude
```

Claude Code auto-carga `CLAUDE.md` y está listo para operar.

---

## Estructura del Proyecto

```
claudecode/
├── CLAUDE.md                    ← Auto-cargado por Claude Code (contexto de la agencia)
├── README.md                    ← Este archivo
├── ARCHITECTURE.md              ← Arquitectura técnica detallada
├── .env.example                 ← Template de variables de entorno
│
├── .claude/
│   └── agents/                  ← Definiciones de agentes (crear con comandos abajo)
│       ├── auto-orchestrator.md
│       ├── auto-intake.md
│       ├── auto-architect.md
│       ├── auto-json-builder.md
│       ├── auto-prompt-writer.md
│       ├── auto-qa-validator.md
│       ├── auto-delivery-formatter.md
│       ├── sub-maxwell.md
│       ├── sub-nexus.md
│       ├── sub-atlas.md
│       ├── sub-victor.md
│       ├── sub-sentinel.md
│       ├── sub-director.md
│       ├── video-orchestrator.md
│       ├── video-reference-analyzer.md
│       ├── video-blueprint-generator.md
│       ├── video-resources-finder.md
│       └── video-capcut-configurator.md
│
├── knowledge/                   ← Archivos de referencia (fuentes de verdad)
│   ├── aut8ma-system-prompt.md
│   ├── video-editor-prompt.md
│   ├── n8n-node-library.md
│   ├── n8n-json-schema.md
│   ├── plans-catalog.md
│   ├── qa-checklist-block53.md
│   ├── delivery-template.md
│   └── capcut-settings-reference.md
│
├── templates/                   ← Scaffolds para documentos de trabajo
│   ├── intake-template.md
│   ├── architecture-template.md
│   ├── json-chunk-template.md
│   ├── prompt-b-template.md
│   ├── qa-report-template.md
│   ├── delivery-doc-template.md
│   └── video-blueprint-template.md
│
└── clients/                     ← Estado por cliente
    ├── _registry.md             ← Tabla maestra de todos los clientes
    └── CLIENT-001-[slug]/       ← Una carpeta por cliente
        ├── profile.md
        ├── intake.md
        ├── architecture.md
        ├── workflow-draft.md
        ├── prompt-b.md
        ├── qa-report.md
        └── delivery.md
```

---

## Crear los Agentes (comandos CLI)

Los agentes de Claude Code viven en `.claude/agents/`. Créalos en este orden:

### Orden recomendado de creación

```bash
# Dentro de claude code, puedes pedirle que cree cada agente:
# "Crea el agente auto-orchestrator siguiendo el template en templates/ y la arquitectura en ARCHITECTURE.md"

# O crea los archivos manualmente siguiendo el template:
# Ver: templates/agent-template.md (incluido en ARCHITECTURE.md sección 6)
```

### Fase 1 — Automation Team Core

| # | Comando / Archivo | Agente | Descripción |
|---|---|---|---|
| 1 | `.claude/agents/auto-orchestrator.md` | ARIA | Router principal del Automation Team |
| 2 | `.claude/agents/auto-intake.md` | INTAKE | Procesa notas de reunión → Prompt A + B |
| 3 | `.claude/agents/auto-architect.md` | NEXARCH | Diseña arquitectura N8N (lista de nodos) |
| 4 | `.claude/agents/auto-json-builder.md` | JSONCRAFT | Genera JSON N8N en partes de 15 nodos |
| 5 | `.claude/agents/auto-prompt-writer.md` | SCRIBE | Escribe Prompt B (system prompt del chatbot) |
| 6 | `.claude/agents/auto-qa-validator.md` | QA-GATE | Valida outputs contra Block 5.3 |
| 7 | `.claude/agents/auto-delivery-formatter.md` | PACKAGER | Ensambla documento final de entrega |

### Fase 2 — Automation Sub-Agents

| # | Archivo | Persona | Especialidad |
|---|---|---|---|
| 8 | `.claude/agents/sub-maxwell.md` | Maxwell | Marketing, campañas, copies |
| 9 | `.claude/agents/sub-nexus.md` | Nexus | ROI, finanzas, pricing |
| 10 | `.claude/agents/sub-atlas.md` | Atlas | Propuestas comerciales, contratos |
| 11 | `.claude/agents/sub-victor.md` | Victor | Scripts de venta, objeciones Venezuela |
| 12 | `.claude/agents/sub-sentinel.md` | Sentinel | Seguridad, auditoría de JSON |
| 13 | `.claude/agents/sub-director.md` | Director | Guiones de video |

### Fase 3 — Video Team

| # | Archivo | Nombre | Descripción |
|---|---|---|---|
| 14 | `.claude/agents/video-orchestrator.md` | VORA | Router del Video Team |
| 15 | `.claude/agents/video-reference-analyzer.md` | LENS | Analiza videos de referencia |
| 16 | `.claude/agents/video-blueprint-generator.md` | BLUEPRINT | Blueprint de edición segmento por segmento |
| 17 | `.claude/agents/video-resources-finder.md` | SCOUT | B-rolls, SFX, música con keywords CapCut |
| 18 | `.claude/agents/video-capcut-configurator.md` | CONFIGURA | Config técnica CapCut |

---

## Checklist Antes de Crear Agentes

- [ ] `CLAUDE.md` existe y contiene el contexto de aut8ma
- [ ] Carpeta `knowledge/` existe con los 8 archivos de referencia
- [ ] Carpeta `templates/` existe con los 7 templates
- [ ] `clients/_registry.md` existe con el schema correcto
- [ ] `.env` existe con `ANTHROPIC_API_KEY` configurado
- [ ] Claude Code reconoce el proyecto (auto-carga `CLAUDE.md`)

---

## Variables de Entorno Necesarias

Ver `.env.example` para la lista completa. Mínimo para operar:

```bash
ANTHROPIC_API_KEY=sk-ant-...
```

---

## Cómo Registrar un Cliente Nuevo

1. Agregar una fila en `clients/_registry.md`
2. Crear la carpeta `clients/CLIENT-XXX-[slug]/`
3. Crear `clients/CLIENT-XXX-[slug]/profile.md` con el perfil del cliente
4. Decirle a ARIA: `"ARIA, cliente nuevo CLIENT-XXX, intake: [notas de la reunión]"`

---

## Flujo de Trabajo por Equipo

```
Automation: ARIA → INTAKE → NEXARCH → JSONCRAFT → SCRIBE → QA-GATE → PACKAGER
Video:      VORA → LENS → BLUEPRINT → SCOUT → CONFIGURA → [entrega en chat]
```

Ver `ARCHITECTURE.md` para el flujo completo con diagramas.

---

## Soporte

- Documentación: `ARCHITECTURE.md`
- Estándares N8N: `knowledge/aut8ma-system-prompt.md`
- Estilo de video: `knowledge/video-editor-prompt.md`
- Estado de clientes: `clients/_registry.md`

# Intake Document: [Nombre del Cliente] — [Fecha]

**Procesado por**: INTAKE (auto-intake)
**Plan**: Emprendedor / Negocio / Premium

---

## Notas Originales de la Reunión (raw)

```
[Pegar aquí las notas desordenadas de Tavo — no editar]
```

---

## ANÁLISIS DE COHERENCIA

### Conflictos Detectados
- [ ] Ninguno — proceder
- [ ] [Conflicto 1]: [descripción]
- [ ] [Conflicto 2]: [descripción]

### Ambigüedades que Requieren Aclaración
- [ ] [Ambigüedad 1]: ¿[pregunta para Tavo]?
- [ ] [Ambigüedad 2]: ¿[pregunta para Tavo]?

> ⚠️ Si hay conflictos o ambigüedades → DETENER y preguntar a Tavo antes de continuar.

---

## PROMPT A — Contexto Estructurado de Automatización

```
═══════════════════════════════════════════════════════
PROMPT DE AUTOMATIZACIÓN — [NOMBRE DEL NEGOCIO]
Plan: [Emprendedor/Negocio/Premium] · Nodos estimados: [X-Y]
═══════════════════════════════════════════════════════

CONTEXTO DEL NEGOCIO:
Nombre: [nombre del negocio]
Ciudad: [ciudad]
Industria: [tipo de negocio]
Problema principal: [problema que resuelve el chatbot]
Canal principal: [WhatsApp / Instagram / ambos]
Volumen esperado: [X mensajes/día]
Horario de atención: [horario]
Sistemas actuales: [qué usan hoy]

PRODUCTOS Y SERVICIOS:
[lista completa con precios si se conocen]

OBJETIVO DE LA AUTOMATIZACIÓN:
[Qué problema resuelve exactamente]
[Métricas de éxito esperadas]

FUNCIONALIDADES REQUERIDAS (específicas del cliente):
→ [Funcionalidad 1 — descripción exacta]
→ [Funcionalidad 2 — descripción exacta]
→ [Funcionalidad 3 — descripción exacta]

FUNCIONALIDADES ESTÁNDAR aut8ma (siempre incluidas):
→ Sistema RAG con Supabase (catálogo del negocio)
→ Memoria de conversaciones con PostgreSQL
→ Buffer Redis 5s anti-spam
→ Filtro anti-bucle y anti-ban WhatsApp
→ Clasificación leads Verde/Amarillo/Rojo/Negro
→ Recontacto automático 1h/24h/48h
→ Sistema de ventas Wolf of Wall Street integrado
→ Transcripción de audios con Groq Whisper
→ Manejo de errores con fallback amigable

INTEGRACIONES ESPECÍFICAS:
→ [Integración 1: ej. Google Calendar para citas]
→ [Integración 2: ej. Google Sheets CRM]
→ [Integración 3: ej. sistema de inventario]

DATOS QUE CAPTURA EL CHATBOT:
→ [Dato 1]: [para qué se usa]
→ [Dato 2]: [para qué se usa]

VARIABLES DE ENTORNO NECESARIAS:
→ YCLOUD_API_KEY, OPENAI_API_KEY, GROQ_API_KEY (estándar)
→ [Variables específicas del cliente]

RESTRICCIONES:
→ [Cosa que el chatbot NO debe hacer]
→ [Limitaciones técnicas conocidas]

MÉTRICAS DE ÉXITO:
→ Tiempo de respuesta: <5 segundos
→ Leads clasificados: 100% automático
→ [KPIs específicos del cliente]
```

---

## SEMILLA DE PROMPT B

```
IDENTIDAD DEL AGENTE:
Nombre del asistente: [nombre elegido por el cliente]
Negocio que representa: [nombre del negocio]
Tono: [formal / informal / amigable / profesional]
Emojis: máximo [X] por mensaje
Palabras que siempre usa: [lista]
Palabras que nunca usa: [lista]

MISIÓN:
[Adaptar al tipo de negocio]

PRODUCTOS/SERVICIOS:
[Lista completa con precios]

DATOS OBLIGATORIOS PARA CONFIRMAR [PEDIDO/CITA]:
→ [Dato 1]
→ [Dato 2]

OBJECIONES ESPECÍFICAS DE ESTE NEGOCIO:
"[Objeción más común]": "[cómo responder]"

RESTRICCIONES:
[Lo que no puede hacer el chatbot]
```

---

## VALIDACIONES IMPLEMENTADAS

- [ ] El plan contratado coincide con el volumen esperado de mensajes
- [ ] Las funcionalidades solicitadas caben en el límite de nodos del plan
- [ ] No hay integraciones que requieran APIs no disponibles
- [ ] El horario de atención está definido (para el nodo IF de horario)
- [ ] El tono del cliente es compatible con el sistema Wolf of Wall Street

---

## RECOMENDACIONES Y PREGUNTAS PENDIENTES

1. [Recomendación 1]
2. [Pregunta pendiente para el cliente]

---

## ROI ESTIMADO

- Horas ahorradas por semana: ~[X] horas
- Leads recuperados: ~[X]% de los que antes se perdían
- Tiempo de retorno de inversión: ~[X] semanas

---

**HANDOFF → auto-architect**: Usar el Prompt A de arriba para diseñar la arquitectura N8N completa de [N] nodos.

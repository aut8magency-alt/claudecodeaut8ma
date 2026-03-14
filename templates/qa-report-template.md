# QA Report: [Nombre del Cliente] — [Fecha]

**Revisado por**: QA-GATE (auto-qa-validator)
**Documento revisado**: architecture / json-parte-[N] / prompt-b / delivery
**Resultado general**: ✅ PASS | ❌ FAIL

---

## SECCIÓN 1: SEGURIDAD

| # | Ítem | Estado | Notas |
|---|------|--------|-------|
| S1 | Filtro anti-bucle (fromMe===true→STOP) | ✅/❌ | |
| S2 | Filtro status updates (body.statuses→STOP) | ✅/❌ | |
| S3 | Credenciales como process.env (no hardcodeadas) | ✅/❌ | |
| S4 | Bloque anti-inyección en Prompt B | ✅/❌ | |

## SECCIÓN 2: ANTI-BAN WHATSAPP

| # | Ítem | Estado | Notas |
|---|------|--------|-------|
| A1 | Wait 10s post-webhook | ✅/❌ | |
| A2 | Buffer Redis 5s debounce | ✅/❌ | |
| A3 | Respond Webhook en RAMA PARALELA | ✅/❌ | |
| A4 | 20+ variaciones de respuesta en prompt | ✅/❌ | |
| A5 | SessionKey dinámica = phoneNumber | ✅/❌ | |

## SECCIÓN 3: SISTEMA DE VENTAS

| # | Ítem | Estado | Notas |
|---|------|--------|-------|
| V1 | System prompt Wolf of Wall Street (7 pasos) | ✅/❌ | |
| V2 | Clasificación leads Verde/Amarillo/Rojo/Negro | ✅/❌ | |
| V3 | Recontacto automático 1h/24h/48h | ✅/❌ | |
| V4 | Cross-selling Regla del 50% para Lista Verde | ✅/❌ | |
| V5 | Downselling para Lista Amarilla/Roja | ✅/❌ | |

## SECCIÓN 4: RAG

| # | Ítem | Estado | Notas |
|---|------|--------|-------|
| R1 | Supabase con pgvector (search_knowledge) | ✅/❌ | |
| R2 | Onboarding via Tally mencionado | ✅/❌ | |
| R3 | Threshold 0.78 configurado | ✅/❌ | |
| R4 | RAG consultado antes de cada respuesta IA | ✅/❌ | |

## SECCIÓN 5: JSON

| # | Ítem | Estado | Notas |
|---|------|--------|-------|
| J1 | UUIDs únicos por nodo | ✅/❌ | |
| J2 | Todas las conexiones verificadas | ✅/❌ | |
| J3 | Try-catch en nodos Code críticos | ✅/❌ | |
| J4 | Fallbacks para APIs externas | ✅/❌ | |

## SECCIÓN 6: ERRORES CRÍTICOS

| # | Error crítico | Estado |
|---|--------------|--------|
| E1 | SessionKey estática | ✅ No hay / ❌ Encontrado |
| E2 | Respond Webhook en serie | ✅ No hay / ❌ Encontrado |
| E3 | Sin filtro anti-bucle | ✅ No hay / ❌ Encontrado |
| E4 | URL imagen de página web (no .png directo) | ✅ No hay / ❌ Encontrado |
| E5 | maxIterations del agente IA sin límite | ✅ No hay / ❌ Encontrado |
| E6 | Sin Wait 10s post-webhook | ✅ No hay / ❌ Encontrado |
| E7 | Sin sistema de recontacto | ✅ No hay / ❌ Encontrado |
| E8 | Sin RAG | ✅ No hay / ❌ Encontrado |
| E9 | Credenciales hardcodeadas en JSON | ✅ No hay / ❌ Encontrado |
| E10 | JSON truncado | ✅ Completo / ❌ Truncado |

---

## ÍTEMS FALLIDOS — INSTRUCCIONES DE CORRECCIÓN

*(Completar solo si hay fallas)*

### ❌ [Código] — [Nombre del ítem]

**Problema encontrado**: [Descripción exacta de qué está mal y dónde]

**Corrección requerida**: [Instrucción exacta y específica de cómo corregirlo]

**Retornar a**: auto-architect / auto-json-builder / auto-prompt-writer

---

### ❌ [Código] — [Nombre del ítem]

**Problema encontrado**: [Descripción]

**Corrección requerida**: [Instrucción]

**Retornar a**: [agente]

---

## RESUMEN DE REVISIONES

| Revisión # | Fecha | Resultado | Ítems fallidos |
|-----------|-------|-----------|----------------|
| 1 | [fecha] | ✅ PASS / ❌ FAIL | [lista] |
| 2 | [fecha] | ✅ PASS / ❌ FAIL | [lista] |

---

## DECISIÓN FINAL

**→ ✅ APROBADO para entrega** — Continuar con auto-delivery-formatter

*o*

**→ ❌ RETORNAR a [nombre del agente]** con las correcciones indicadas arriba.

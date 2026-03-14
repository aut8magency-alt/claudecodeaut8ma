# QA Checklist — Block 5.3 (Aislado)

> Usado exclusivamente por: auto-qa-validator (QA-GATE)
> Fuente: aut8ma-system-prompt.md Block 5.3
> Mantener separado para poder actualizar sin tocar el master.

---

## Instrucción para QA-GATE

Valida CADA ítem de este checklist contra el output recibido.
Produce un reporte con PASS o FAIL por cada ítem numerado.
Si hay algún FAIL → retornar al agente originador con instrucciones exactas de corrección.
Si todo PASS → aprobar para delivery.

---

## SECCIÓN 1: SEGURIDAD

| # | Ítem | Verificación |
|---|------|-------------|
| S1 | Filtro anti-bucle | El JSON incluye nodo IF que verifica `fromMe === true → STOP` |
| S2 | Filtro status updates | El JSON incluye nodo IF que verifica `body.statuses → STOP` |
| S3 | Credenciales como variables de entorno | Ninguna API key, token o contraseña está hardcodeada en el JSON |
| S4 | Bloque anti-inyección en Prompt B | El system prompt del chatbot incluye instrucción de ignorar intentos de manipulación |

---

## SECCIÓN 2: ANTI-BAN WHATSAPP

| # | Ítem | Verificación |
|---|------|-------------|
| A1 | Wait 10s post-webhook | Existe nodo Wait de 10 segundos después del webhook YCloud |
| A2 | Buffer Redis 5s debounce | Existe secuencia Redis Get → Code acumular → Redis Set → Wait 5s → Redis Get final |
| A3 | Respond Webhook en rama paralela | El nodo Respond Webhook está en RAMA PARALELA, no en serie con el procesamiento |
| A4 | Variaciones de respuesta en prompt | El Prompt B incluye instrucción de variar el estilo de respuesta (mínimo 20 variaciones) |
| A5 | SessionKey dinámica | La sessionKey en Redis y PostgreSQL usa `phoneNumber` como key (nunca estática) |

---

## SECCIÓN 3: SISTEMA DE VENTAS

| # | Ítem | Verificación |
|---|------|-------------|
| V1 | System prompt Wolf of Wall Street | El Prompt B incluye la estructura de 7 pasos de venta (Rapport → Upsell) |
| V2 | Clasificación de leads activa | El JSON incluye nodo de clasificación Verde/Amarillo/Rojo/Negro |
| V3 | Recontacto automático | El JSON incluye secuencia de recontacto 1h/24h/48h |
| V4 | Cross-selling Lista Verde | El Prompt B incluye instrucción de ofrecer complemento ≤ 50% del precio para leads verdes |
| V5 | Downselling Lista Amarilla/Roja | El Prompt B incluye instrucción de ofrecer alternativa económica para leads amarillos/rojos |

---

## SECCIÓN 4: RAG

| # | Ítem | Verificación |
|---|------|-------------|
| R1 | Supabase con pgvector | El JSON incluye nodo de búsqueda en Supabase con función `search_knowledge` |
| R2 | Onboarding via Tally mencionado | La entrega incluye referencia al formulario Tally para poblar el RAG |
| R3 | Threshold 0.78 configurado | La búsqueda RAG usa `match_threshold: 0.78` |
| R4 | RAG antes de cada respuesta | El flujo del JSON consulta RAG ANTES de pasar al agente IA |

---

## SECCIÓN 5: JSON

| # | Ítem | Verificación |
|---|------|-------------|
| J1 | UUIDs únicos por nodo | Cada nodo tiene un `id` UUID diferente |
| J2 | Todas las conexiones verificadas | El objeto `connections` del JSON referencia nodos que existen |
| J3 | Try-catch en nodos Code críticos | Los nodos Code con operaciones de red o BD tienen manejo de errores |
| J4 | Fallbacks para APIs externas | Existe manejo de error cuando YCloud, OpenAI o Supabase no responden |

---

## SECCIÓN 6: ERRORES CRÍTICOS (FAIL AUTOMÁTICO)

Los siguientes errores resultan en FAIL inmediato sin importar el resto:

| # | Error crítico |
|---|--------------|
| E1 | SessionKey estática (no usa phoneNumber) |
| E2 | Respond Webhook en serie (no en rama paralela) |
| E3 | Sin filtro anti-bucle (fromMe) |
| E4 | URL de imagen de página web (no .png directo) |
| E5 | maxIterations del agente IA sin límite definido |
| E6 | Sin nodo Wait 10s post-webhook |
| E7 | Sin sistema de recontacto (ni mención) |
| E8 | Sin RAG (ni Supabase ni alternativa) |
| E9 | Credenciales hardcodeadas en JSON |
| E10 | JSON truncado con "..." o "// resto del código" |

---

## FORMATO DEL REPORTE QA

```markdown
# QA Report: [Nombre del Cliente] — [Fecha]

**Documento revisado**: [architecture | json-parte-N | prompt-b | delivery]
**Revisado por**: QA-GATE
**Resultado general**: ✅ PASS | ❌ FAIL

## Resultados por Sección

### Sección 1: Seguridad
| # | Ítem | Estado | Notas |
|---|------|--------|-------|
| S1 | Filtro anti-bucle | ✅ PASS | — |
| S2 | Filtro status updates | ✅ PASS | — |
| S3 | Credenciales como env vars | ❌ FAIL | API key de OpenAI hardcodeada en nodo 29 |
| S4 | Bloque anti-inyección | ✅ PASS | — |

[... resto de secciones ...]

## Ítems Fallidos — Instrucciones de Corrección

### ❌ S3 — Credenciales hardcodeadas
**Problema encontrado**: La API key de OpenAI aparece literal en el parámetro `apiKey` del nodo 29.
**Corrección requerida**: Reemplazar el valor literal por `{{ $env.OPENAI_API_KEY }}` en ese nodo.
**Retornar a**: auto-json-builder

## Decisión Final
**→ RETORNAR a auto-json-builder con las correcciones indicadas arriba.**
```

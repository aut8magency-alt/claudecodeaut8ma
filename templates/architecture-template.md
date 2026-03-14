# Arquitectura N8N: [Nombre del Cliente] — [Nombre del Workflow]

**Diseñado por**: NEXARCH (auto-architect)
**Plan**: Emprendedor / Negocio / Premium
**Total de nodos**: [N]
**Partes JSON**: [M] (máx. 15 nodos por parte)

---

## Mapa Completo de Nodos

> FASE 1: Esta lista debe ser revisada y aprobada por Tavo ANTES de generar el JSON.

### SECCIÓN A — ENTRADA Y FILTROS [nodos 1-8]

| # | Nombre del Nodo | Tipo N8N | Función | Variables de entrada | Variables de salida |
|---|----------------|----------|---------|---------------------|---------------------|
| 1 | Webhook YCloud | `n8n-nodes-base.webhook` | Recibe mensajes entrantes de YCloud | — | `body` (payload completo) |
| 2 | Respond Webhook | `n8n-nodes-base.respondToWebhook` | Confirma 200 OK (RAMA PARALELA) | — | — |
| 3 | Extraer Payload | `n8n-nodes-base.code` | Extrae campos útiles del body | `body` | `phoneNumber`, `messageType`, `fromMe`, `textBody`, `audioId` |
| 4 | IF Anti-bucle | `n8n-nodes-base.if` | Detiene si fromMe=true | `fromMe` | TRUE→continua / FALSE→STOP |
| 5 | IF Status Updates | `n8n-nodes-base.if` | Detiene si es status update | `body.statuses` | TRUE→STOP / FALSE→continua |
| 6 | IF Mensaje Vacío | `n8n-nodes-base.if` | Detiene si no hay texto ni media | `textBody`, `audioId` | TRUE→continua / FALSE→STOP |
| 7 | Wait 10s | `n8n-nodes-base.wait` | Anti-ban Meta (OBLIGATORIO) | — | — |
| 8 | Normalizar Teléfono | `n8n-nodes-base.code` | Limpia y normaliza el número | `phoneNumber` | `phoneNumber` normalizado |

### SECCIÓN B — BUFFER [nodos 9-13]

| # | Nombre del Nodo | Tipo N8N | Función | Variables |
|---|----------------|----------|---------|-----------|
| 9 | Redis Get Buffer | `n8n-nodes-base.redis` | Obtiene mensajes en buffer | `phoneNumber` |
| 10 | Acumular Mensaje | `n8n-nodes-base.code` | Agrega mensaje actual al buffer | buffer + nuevo mensaje |
| 11 | Redis Set Buffer | `n8n-nodes-base.redis` | Guarda buffer actualizado | buffer actualizado |
| 12 | Wait 5s | `n8n-nodes-base.wait` | Espera más mensajes (debounce) | — |
| 13 | Redis Get Final | `n8n-nodes-base.redis` | Lee buffer final y lo limpia | `phoneNumber` → `allMessages` |

### SECCIÓN C — MULTIMEDIA [nodos 14-20] (si aplica)

| # | Nombre del Nodo | Tipo N8N | Función |
|---|----------------|----------|---------|
| 14 | IF Es Audio | `n8n-nodes-base.if` | Detecta si hay audio |
| 15 | Descargar Audio | `n8n-nodes-base.httpRequest` | Descarga audio de YCloud |
| 16 | Transcribir (Whisper) | `n8n-nodes-base.httpRequest` | Groq Whisper → texto |
| 17 | IF Es Imagen | `n8n-nodes-base.if` | Detecta si hay imagen |
| 18 | Descargar Imagen | `n8n-nodes-base.httpRequest` | Descarga imagen de YCloud |
| 19 | Analizar Imagen (GPT-4V) | `@n8n/n8n-nodes-langchain.lmChatOpenAi` | Describe la imagen |
| 20 | Consolidar Input | `n8n-nodes-base.code` | Combina texto + audio + imagen |

### SECCIÓN D — MEMORIA Y RAG [nodos 21-28]

| # | Nombre del Nodo | Tipo N8N | Función |
|---|----------------|----------|---------|
| 21 | Postgres Get Sesión | `n8n-nodes-base.postgres` | Historial de conversación |
| 22 | Formatear Historial | `n8n-nodes-base.code` | Prepara historial para el agente |
| 23 | Postgres Get Perfil | `n8n-nodes-base.postgres` | Perfil del cliente |
| 24 | OpenAI Embeddings | `@n8n/n8n-nodes-langchain.embeddingsOpenAi` | Vectoriza el mensaje |
| 25 | Supabase RAG Search | `n8n-nodes-base.httpRequest` | Búsqueda semántica en knowledge_base |
| 26 | Detectar Intención | `n8n-nodes-base.code` | Clasifica la intención del mensaje |
| 27 | Clasificar Lead | `n8n-nodes-base.code` | Verde/Amarillo/Rojo/Negro |
| 28 | Enriquecer Contexto | `n8n-nodes-base.code` | Combina historial + perfil + RAG + intención |

### SECCIÓN E — AGENTE IA [nodos 29-38]

| # | Nombre del Nodo | Tipo N8N | Función |
|---|----------------|----------|---------|
| 29 | LangChain Agent | `@n8n/n8n-nodes-langchain.agent` | Agente IA con system prompt Wolf + tools |
| 30 | Post-procesar Respuesta | `n8n-nodes-base.code` | Limpia y formatea la respuesta |
| 31 | Detectar Pedido | `n8n-nodes-base.code` | Verifica si hay pedido confirmado |
| 32 | IF Hay Pedido | `n8n-nodes-base.if` | Bifurca si hay pedido |
| 33 | Postgres Registrar Pedido | `n8n-nodes-base.postgres` | Guarda el pedido |
| 34 | Google Sheets Update | `n8n-nodes-base.googleSheets` | Actualiza CRM |
| 35 | Update Lead Status | `n8n-nodes-base.code` | Actualiza clasificación |
| 36 | Postgres Save Sesión | `n8n-nodes-base.postgres` | Guarda sesión actualizada |
| 37 | Seleccionar Variación | `n8n-nodes-base.code` | Anti-ban: varía el formato de respuesta |
| 38 | IF Horario Atención | `n8n-nodes-base.if` | Verifica si está en horario |

### SECCIÓN F — ENVÍO [nodos 39-46]

| # | Nombre del Nodo | Tipo N8N | Función |
|---|----------------|----------|---------|
| 39 | HTTP Enviar Texto | `n8n-nodes-base.httpRequest` | Envía texto a YCloud |
| 40 | IF Hay Imagen | `n8n-nodes-base.if` | Verifica si hay imagen que enviar |
| 41 | HTTP Enviar Imagen | `n8n-nodes-base.httpRequest` | Envía imagen a YCloud |
| 42 | IF Hay Audio | `n8n-nodes-base.if` | Verifica si enviar audio |
| 43 | ElevenLabs TTS | `n8n-nodes-base.httpRequest` | Genera audio de respuesta |
| 44 | HTTP Enviar Audio | `n8n-nodes-base.httpRequest` | Envía audio a YCloud |
| 45 | Postgres Update Métricas | `n8n-nodes-base.postgres` | Registra métricas del día |
| 46 | HTTP Notificar Propietario | `n8n-nodes-base.httpRequest` | Telegram si hay pedido/lead caliente |

### [Secciones G-I — según plan Negocio/Premium]

*(Completar secciones G: Recontacto, H: Errores, I: Reportes según el plan)*

---

## Diagrama de Flujo Simplificado

```
[1-Webhook] → [2-Respond](paralelo) + [3-Extraer Payload]
[3] → [4-Anti-bucle]→STOP | → [5-Status]→STOP | → [6-Vacío]→STOP | → [7-Wait 10s]
[7] → [8-Normalizar] → [9-13 Buffer Redis]
[13] → [14-20 Multimedia (si audio/imagen)]
[20] → [21-28 Memoria + RAG]
[28] → [29-38 Agente IA + Pedido]
[38] → [39-46 Envío + Notificaciones]
[46] → [47-54 Recontacto (si Plan Negocio/Premium)]
```

---

## División en Partes para el JSON

| Parte | Nodos | Secciones | Nodo conector |
|-------|-------|-----------|---------------|
| Parte 1 | 1-15 | A completa + B | "Wait 5s" |
| Parte 2 | 16-28 | B final + C + D | "Enriquecer Contexto" |
| Parte 3 | 29-38 | E | "IF Horario" |
| Parte 4 | 39-[N] | F + G + H + I | — |

---

## Variables Globales del Workflow

| Variable | Tipo | Origen | Destino |
|----------|------|--------|---------|
| `phoneNumber` | string | Nodo 3 | Todos los Redis/Postgres |
| `textBody` | string | Nodo 3 | Nodo 20 (consolidar) |
| `allMessages` | array | Nodo 13 | Nodo 20 |
| `ragContext` | string | Nodo 25 | Nodo 28 |
| `sessionData` | object | Nodo 21 | Nodo 22 |
| `systemPrompt` | string | Nodo 28 | Nodo 29 |
| `aiResponse` | string | Nodo 29 | Nodo 30 |
| `leadStatus` | string | Nodo 27 | Nodos 34, 35, 47 |

---

**HANDOFF → auto-json-builder**: Usar esta arquitectura para generar el JSON en [M] partes de máx. 15 nodos cada una.

> ⚠️ Esperar confirmación/aprobación de la arquitectura antes de generar el JSON.

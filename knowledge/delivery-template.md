# Template de Documento de Entrega Final

> Usado por: auto-delivery-formatter (PACKAGER)
> Este es el formato exacto del documento que recibe el cliente/Tavo.

---

## TEMPLATE COMPLETO

```markdown
═══════════════════════════════════════════════════════════════
🤖 ENTREGA DE AUTOMATIZACIÓN — aut8ma
Cliente: [NOMBRE DEL CLIENTE] | Plan: [PLAN] | Fecha: [FECHA]
═══════════════════════════════════════════════════════════════

## 📋 RESUMEN EJECUTIVO

**Cliente**: [Nombre del negocio]
**Plan contratado**: [Emprendedor / Negocio / Premium]
**Chatbot**: [Nombre del asistente IA]
**Canal principal**: [WhatsApp / Instagram / ambos]
**Total de nodos N8N**: [N]
**Partes JSON**: [N partes de máx 15 nodos c/u]

**Lo que recibe en este documento:**
1. ✅ Prompt A — Contexto de automatización
2. ✅ Arquitectura N8N — Mapa completo de nodos
3. ✅ JSON en [N] partes — Listo para importar en N8N
4. ✅ Prompt B — System prompt del agente IA
5. ✅ Schema SQL — Para Supabase (RAG)
6. ✅ Variables de entorno necesarias
7. ✅ Checklist de instalación
8. ✅ Checklist de pruebas

---

## PARTE 1 — PROMPT A: CONTEXTO DE AUTOMATIZACIÓN

═══════════════════════════════════════════════════════
PROMPT DE AUTOMATIZACIÓN — [NOMBRE DEL NEGOCIO]
Plan: [Plan] · Nodos: [X-Y]
═══════════════════════════════════════════════════════

### Contexto del Negocio
[Descripción del negocio, qué venden, ciudad, canal]

### Objetivo de la Automatización
[Qué problema resuelve, métricas de éxito]

### Funcionalidades Incluidas
→ [Funcionalidad específica del cliente 1]
→ [Funcionalidad específica del cliente 2]
→ [Funcionalidades estándar aut8ma...]

### Integraciones
→ YCloud WhatsApp API
→ OpenAI GPT-4o
→ Supabase RAG
→ [Integraciones específicas del cliente]

### Datos que Captura el Chatbot
→ [Dato 1]: [uso]
→ [Dato 2]: [uso]

---

## PARTE 2 — ARQUITECTURA N8N

### Mapa de Nodos (Total: [N] nodos en [M] partes)

| # | Nodo | Tipo N8N | Sección | Función |
|---|------|----------|---------|---------|
| 1 | Webhook YCloud | webhook | A | Entrada de mensajes |
| 2 | Respond Webhook | respondToWebhook | A | Confirma recepción (rama paralela) |
| ... | ... | ... | ... | ... |

### Diagrama de Flujo
```
[Webhook] → [Respond Webhook] (paralelo)
          → [Extraer Payload]
               → [IF Anti-bucle] → STOP si fromMe=true
               → [IF Status]     → STOP si statuses
               → [Wait 10s]
               → [...]
```

### Variables que Viajan entre Nodos
- `phoneNumber`: clave de sesión y Redis
- `textBody`: texto del mensaje procesado
- `ragContext`: resultado de búsqueda en Supabase
- `sessionData`: historial de conversación
- `leadStatus`: Verde/Amarillo/Rojo/Negro
- [otras variables específicas del cliente]

---

## PARTE 3 — JSON N8N

> Importar en este orden en N8N: Parte 1, luego Parte 2, etc.
> Hacer las conexiones entre partes según las instrucciones al final de cada parte.

### PARTE 1/[N] — [Nombre de la sección A]
Nodos: 1 al 15
Conecta con PARTE 2 vía nodo "[Nombre]"

```json
{
  "name": "aut8ma — [Cliente] — Chatbot WhatsApp — Parte 1",
  "nodes": [
    ... (JSON completo aquí, nunca truncado)
  ],
  "connections": { ... }
}
```

→ Importa esta parte en N8N, luego di "siguiente" o sigue con la PARTE 2.

---

### PARTE 2/[N] — [Nombre de la sección B-C]
[... misma estructura ...]

---

[... repetir para cada parte ...]

---

## PARTE 4 — GUÍA DE ENSAMBLAJE

### Orden de Importación en N8N
1. Importar PARTE 1 → verificar que los nodos aparecen
2. Importar PARTE 2 → conectar manualmente el nodo "[X]" (PARTE 1) con el nodo "[Y]" (PARTE 2)
3. [... continuar por partes ...]

### Conexiones Manuales Requeridas
| Desde (PARTE/Nodo) | Hacia (PARTE/Nodo) | Instrucción |
|---|---|---|
| P1 / Extraer Payload | P2 / Redis Get Buffer | Arrastrar output de "Extraer Payload" al input de "Redis Get Buffer" |
| [etc.] | [etc.] | [etc.] |

---

## PARTE 5 — PROMPT B: SYSTEM PROMPT DEL AGENTE IA

═══════════════════════════════════════════════════════
SYSTEM PROMPT — [NOMBRE DEL ASISTENTE]
Agente IA de [NOMBRE DEL NEGOCIO]
═══════════════════════════════════════════════════════

[Prompt B completo — nunca truncado]

---

## PARTE 6 — SCHEMA SQL SUPABASE (RAG)

```sql
-- Ejecutar en el SQL Editor de Supabase

CREATE EXTENSION IF NOT EXISTS vector;

CREATE TABLE knowledge_base (
  id BIGSERIAL PRIMARY KEY,
  client_id VARCHAR(50) NOT NULL,
  content TEXT NOT NULL,
  embedding VECTOR(1536),
  metadata JSONB,
  created_at TIMESTAMP DEFAULT NOW()
);

CREATE INDEX ON knowledge_base
USING ivfflat (embedding vector_cosine_ops) WITH (lists = 100);

CREATE OR REPLACE FUNCTION search_knowledge(
  query_embedding VECTOR(1536),
  client_filter VARCHAR(50),
  match_threshold FLOAT DEFAULT 0.78,
  match_count INT DEFAULT 5
)
RETURNS TABLE (id BIGINT, content TEXT, similarity FLOAT)
LANGUAGE plpgsql AS $$
BEGIN
  RETURN QUERY
  SELECT kb.id, kb.content,
    1 - (kb.embedding <=> query_embedding) AS similarity
  FROM knowledge_base kb
  WHERE kb.client_id = client_filter
    AND 1 - (kb.embedding <=> query_embedding) > match_threshold
  ORDER BY kb.embedding <=> query_embedding
  LIMIT match_count;
END;
$$;
```

---

## PARTE 7 — VARIABLES DE ENTORNO

Configurar en el servidor N8N (archivo .env o panel de variables):

```bash
# WhatsApp
YCLOUD_API_KEY=[obtener en ycloud.com]
WHATSAPP_NUMBER=[número del bot sin +]

# IA
OPENAI_API_KEY=[obtener en platform.openai.com]
GROQ_API_KEY=[obtener en console.groq.com]

# Base de datos
POSTGRES_HOST=[IP o dominio del servidor]
POSTGRES_PORT=5432
POSTGRES_DB=aut8ma_[slug_cliente]
POSTGRES_USER=aut8ma_user
POSTGRES_PASSWORD=[contraseña segura]

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# Supabase (RAG)
SUPABASE_URL=[obtener en supabase.com]
SUPABASE_ANON_KEY=[obtener en supabase.com]

# Cliente
CLIENT_ID=[slug del cliente, ej: acme-caracas]
BOT_NAME=[nombre del chatbot, ej: Luna]

# Notificaciones
TELEGRAM_BOT_TOKEN=[opcional]
TELEGRAM_CHAT_ID=[opcional]

# [Variables específicas del cliente...]
```

---

## PARTE 8 — CHECKLIST DE INSTALACIÓN

### Antes de Activar

- [ ] N8N self-hosted está funcionando y accesible por HTTPS
- [ ] PostgreSQL instalado con las tablas `sessions`, `orders`, `metrics`
- [ ] Redis instalado y corriendo
- [ ] Supabase: schema SQL ejecutado, tabla `knowledge_base` existe
- [ ] Tally: formulario de onboarding creado y webhook conectado a N8N
- [ ] YCloud: número de WhatsApp aprobado y webhook configurado
- [ ] Todas las variables de entorno configuradas en N8N
- [ ] Las 3 partes del JSON importadas en N8N
- [ ] Conexiones manuales realizadas (ver Guía de Ensamblaje)
- [ ] Credenciales de Postgres, Redis y OpenAI configuradas en N8N

### Pruebas Obligatorias Antes de Entregar al Cliente

- [ ] Enviar "Hola" al número → bot responde en <5 segundos
- [ ] Enviar audio → bot transcribe y responde
- [ ] Enviar imagen → bot analiza y responde
- [ ] Enviar consulta de catálogo → bot responde con datos del RAG
- [ ] Simular pedido completo → se registra en Postgres y Google Sheets
- [ ] Lead amarillo → llega recontacto en 1 hora
- [ ] Intentar inyección de prompt → bot ignora y continúa su rol
- [ ] Verificar en logs que no hay errores 500

---

## NOTAS FINALES

**Formulario Tally de Onboarding RAG**: [URL del formulario]
El cliente debe llenarlo para que el chatbot sepa todo sobre su negocio.
Una vez enviado, N8N procesa y sube la info al RAG automáticamente.

**Soporte post-instalación**: [canal de soporte]
**Próxima revisión mensual**: [fecha]

---
*Generado por aut8ma · [FECHA] · Venezuela*
```

# N8N Node Library — aut8ma

> Catálogo de nodos usados en automatizaciones de aut8ma.
> Referencia para: auto-architect, auto-json-builder

---

## NODOS CORE (usados en casi todas las automatizaciones)

### Webhook
```json
{
  "type": "n8n-nodes-base.webhook",
  "typeVersion": 2,
  "parameters": {
    "httpMethod": "POST",
    "path": "unique-path-here",
    "responseMode": "responseNode"
  }
}
```
**Uso**: Entrada principal. Recibe mensajes de YCloud.
**Gotcha**: Usar `responseMode: "responseNode"` para que el Respond Webhook funcione en paralelo.

### Respond to Webhook
```json
{
  "type": "n8n-nodes-base.respondToWebhook",
  "typeVersion": 1,
  "parameters": {
    "respondWith": "text",
    "responseBody": "OK"
  }
}
```
**Uso**: Responder 200 OK a YCloud inmediatamente.
**CRÍTICO**: Debe estar en RAMA PARALELA al processing. Nunca en serie.

### IF (Condicional)
```json
{
  "type": "n8n-nodes-base.if",
  "typeVersion": 2,
  "parameters": {
    "conditions": {
      "options": { "caseSensitive": true },
      "conditions": [
        {
          "leftValue": "={{ $json.fromMe }}",
          "rightValue": true,
          "operator": { "type": "boolean", "operation": "equals" }
        }
      ]
    }
  }
}
```
**Uso**: Filtros, bifurcaciones de lógica.
**Gotcha**: TypeVersion 2 usa la nueva sintaxis de conditions.

### Code (JavaScript)
```json
{
  "type": "n8n-nodes-base.code",
  "typeVersion": 2,
  "parameters": {
    "jsCode": "// código aquí\nreturn [{ json: { resultado: 'valor' } }];"
  }
}
```
**Uso**: Procesamiento custom, transformaciones, lógica compleja.
**Gotcha**: Siempre usar try-catch en operaciones de red o BD. Retornar array de objetos con `json`.

### Wait
```json
{
  "type": "n8n-nodes-base.wait",
  "typeVersion": 1,
  "parameters": {
    "unit": "seconds",
    "amount": 10
  }
}
```
**Uso**: Wait 10s post-webhook (anti-ban), Wait 5s para buffer Redis.
**Gotcha**: `unit` puede ser "seconds", "minutes", "hours". Para timers dinámicos usar `resumeWebhook`.

### HTTP Request
```json
{
  "type": "n8n-nodes-base.httpRequest",
  "typeVersion": 4.2,
  "parameters": {
    "method": "POST",
    "url": "https://api.ycloud.com/v2/whatsapp/messages",
    "authentication": "genericCredentialType",
    "genericAuthType": "httpHeaderAuth",
    "sendHeaders": true,
    "headerParameters": {
      "parameters": [
        { "name": "X-API-Key", "value": "={{ $env.YCLOUD_API_KEY }}" }
      ]
    },
    "sendBody": true,
    "bodyContentType": "json",
    "jsonBody": "={{ JSON.stringify($json.body) }}"
  }
}
```
**Uso**: Llamadas a YCloud, Supabase, ElevenLabs, APIs externas.

---

## NODOS REDIS

### Redis (Get)
```json
{
  "type": "n8n-nodes-base.redis",
  "typeVersion": 1,
  "parameters": {
    "operation": "get",
    "key": "={{ 'buffer:' + $json.phoneNumber }}"
  }
}
```

### Redis (Set)
```json
{
  "type": "n8n-nodes-base.redis",
  "typeVersion": 1,
  "parameters": {
    "operation": "set",
    "key": "={{ 'buffer:' + $json.phoneNumber }}",
    "value": "={{ JSON.stringify($json.buffer) }}",
    "expire": true,
    "ttl": 60
  }
}
```
**Uso**: Buffer de mensajes, caché de sesiones.
**Gotcha**: Usar TTL en todos los Set para evitar memoria fantasma.

---

## NODOS POSTGRESQL

### Postgres (Execute Query)
```json
{
  "type": "n8n-nodes-base.postgres",
  "typeVersion": 2.4,
  "parameters": {
    "operation": "executeQuery",
    "query": "SELECT * FROM sessions WHERE phone_number = $1",
    "additionalFields": {
      "queryParameters": "={{ $json.phoneNumber }}"
    }
  }
}
```
**Uso**: Leer/escribir sesiones, perfiles, pedidos, métricas.
**Gotcha**: Usar parámetros `$1, $2...` nunca interpolación directa (SQL injection).

---

## NODOS DE IA / LLM

### OpenAI (Chat)
```json
{
  "type": "@n8n/n8n-nodes-langchain.lmChatOpenAi",
  "typeVersion": 1,
  "parameters": {
    "model": "gpt-4o",
    "options": {
      "maxTokens": 1000,
      "temperature": 0.7
    }
  }
}
```

### LangChain Agent
```json
{
  "type": "@n8n/n8n-nodes-langchain.agent",
  "typeVersion": 1.7,
  "parameters": {
    "agentType": "toolsAgent",
    "options": {
      "systemMessage": "={{ $json.systemPrompt }}",
      "maxIterations": 5
    }
  }
}
```
**CRÍTICO**: `maxIterations` NUNCA infinito. Usar 5-10 máximo.

### OpenAI Embeddings
```json
{
  "type": "@n8n/n8n-nodes-langchain.embeddingsOpenAi",
  "typeVersion": 1,
  "parameters": {
    "model": "text-embedding-3-small"
  }
}
```

---

## NODOS DE VOZ

### HTTP Request (Groq Whisper — transcribir audio)
```javascript
// En nodo Code:
const audioBuffer = await fetch(audioUrl).then(r => r.arrayBuffer());
const formData = new FormData();
formData.append('file', new Blob([audioBuffer]), 'audio.ogg');
formData.append('model', 'whisper-large-v3');
const response = await fetch('https://api.groq.com/openai/v1/audio/transcriptions', {
  method: 'POST',
  headers: { 'Authorization': `Bearer ${process.env.GROQ_API_KEY}` },
  body: formData
});
const { text } = await response.json();
return [{ json: { transcription: text } }];
```

### HTTP Request (ElevenLabs — generar audio)
```javascript
const response = await fetch(`https://api.elevenlabs.io/v1/text-to-speech/${process.env.ELEVENLABS_VOICE_ID}`, {
  method: 'POST',
  headers: {
    'xi-api-key': process.env.ELEVENLABS_API_KEY,
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    text: $json.responseText,
    model_id: 'eleven_multilingual_v2',
    voice_settings: { stability: 0.5, similarity_boost: 0.75 }
  })
});
```

---

## NODOS DE INTEGRACIONES

### Google Sheets (Append Row)
```json
{
  "type": "n8n-nodes-base.googleSheets",
  "typeVersion": 4.4,
  "parameters": {
    "operation": "appendOrUpdate",
    "documentId": "={{ $env.SHEETS_ID }}",
    "sheetName": "Leads",
    "columns": {
      "mappingMode": "defineBelow",
      "value": {
        "Teléfono": "={{ $json.phoneNumber }}",
        "Nombre": "={{ $json.customerName }}",
        "Estado": "={{ $json.leadStatus }}"
      }
    }
  }
}
```

### Telegram (Send Message — notificación al propietario)
```json
{
  "type": "n8n-nodes-base.telegram",
  "typeVersion": 1.2,
  "parameters": {
    "chatId": "={{ $env.TELEGRAM_CHAT_ID }}",
    "text": "={{ $json.notificationMessage }}"
  }
}
```

### Cron (Trigger programado)
```json
{
  "type": "n8n-nodes-base.scheduleTrigger",
  "typeVersion": 1.2,
  "parameters": {
    "rule": {
      "interval": [
        {
          "field": "cronExpression",
          "expression": "59 23 * * *"
        }
      ]
    }
  }
}
```

---

## ERRORES COMUNES Y SOLUCIONES

| Error | Causa | Solución |
|-------|-------|---------|
| `Cannot read property of undefined` | Payload de webhook vacío | Agregar IF guard: `if (!message) return [{ json: { stop: true } }]` |
| Bucle infinito de mensajes | Sin filtro anti-bucle | Agregar IF: `fromMe === true → STOP` como nodo 4 |
| Meta penaliza el número | Sin Wait 10s o respuestas robotizadas | Agregar Wait 10s + 20+ variaciones en el prompt |
| RAG devuelve resultados irrelevantes | Threshold muy bajo | Subir threshold a 0.78 o más |
| Sesión compartida entre usuarios | SessionKey estática | Cambiar a `sessionKey = phoneNumber` |
| Webhook responde timeout | Respond Webhook en serie | Mover Respond Webhook a rama paralela |

# N8N JSON Schema — Reglas de Formato

> Referencia para: auto-json-builder (JSONCRAFT)
> Estructura exacta que debe tener todo JSON de workflow N8N.

---

## Estructura Top-Level de un Workflow N8N

```json
{
  "name": "aut8ma — [NombreCliente] — [NombreWorkflow]",
  "nodes": [...],
  "connections": {...},
  "active": false,
  "settings": {
    "executionOrder": "v1"
  },
  "staticData": null,
  "meta": {
    "templateCredsSetupCompleted": true
  },
  "pinData": {}
}
```

---

## Estructura de un Nodo

```json
{
  "id": "uuid-único-aquí",
  "name": "Nombre descriptivo del nodo",
  "type": "n8n-nodes-base.webhook",
  "typeVersion": 2,
  "position": [240, 300],
  "parameters": {
    "parametro1": "valor1",
    "parametro2": "={{ $json.campo }}"
  },
  "credentials": {
    "nombreCredencial": {
      "id": "credential-id-placeholder",
      "name": "Nombre de la credencial"
    }
  }
}
```

### Reglas de IDs
- Cada nodo tiene un `id` UUID v4 único
- Formato: `"xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx"`
- NUNCA repetir IDs en el mismo workflow
- NUNCA usar IDs secuenciales como "node-1", "node-2"

### Reglas de Posición
- Eje X: flujo de izquierda a derecha, +200-300px por nodo
- Eje Y: ramas paralelas con +200px de separación vertical
- Primer nodo siempre en `[240, 300]`

---

## Estructura de Connections

```json
{
  "connections": {
    "Nombre del Nodo Origen": {
      "main": [
        [
          {
            "node": "Nombre del Nodo Destino",
            "type": "main",
            "index": 0
          }
        ]
      ]
    }
  }
}
```

### Conexión Simple (1→1)
```json
"Webhook": {
  "main": [
    [{ "node": "Respond Webhook", "type": "main", "index": 0 }]
  ]
}
```

### Rama Paralela (1→2)
```json
"Webhook": {
  "main": [
    [
      { "node": "Respond Webhook", "type": "main", "index": 0 },
      { "node": "Extraer Payload", "type": "main", "index": 0 }
    ]
  ]
}
```

### Nodo IF (dos salidas: true/false)
```json
"IF Anti-bucle": {
  "main": [
    [{ "node": "Siguiente Nodo (true)", "type": "main", "index": 0 }],
    [{ "node": "NoOp STOP (false)", "type": "main", "index": 0 }]
  ]
}
```
**Índice 0** = rama TRUE (condición cumplida)
**Índice 1** = rama FALSE (condición no cumplida)

---

## Credenciales — Formato Correcto

```json
"credentials": {
  "postgresApi": {
    "id": "{{POSTGRES_CREDENTIAL_ID}}",
    "name": "aut8ma Postgres"
  }
}
```

**Reglas:**
- NUNCA poner valores reales de credenciales
- Usar placeholders descriptivos: `"{{POSTGRES_CREDENTIAL_ID}}"`
- El instalador reemplaza estos placeholders en N8N

---

## Variables de Entorno en Expressions

```javascript
// Correcto
"apiKey": "={{ $env.YCLOUD_API_KEY }}"
"url": "={{ $env.N8N_WEBHOOK_URL }}"

// Incorrecto — NUNCA así
"apiKey": "sk-abc123realkey"
"apiKey": "process.env.OPENAI_API_KEY"  // esto es código JS, no expression N8N
```

---

## Expressions N8N — Sintaxis

```javascript
// Acceder al JSON del nodo anterior
"={{ $json.campo }}"
"={{ $json.objeto.subcampo }}"

// Acceder a datos de otro nodo específico
"={{ $('Nombre del Nodo').item.json.campo }}"

// Variables de entorno
"={{ $env.NOMBRE_VARIABLE }}"

// Expresiones condicionales
"={{ $json.tipo === 'audio' ? 'es audio' : 'no es audio' }}"

// Concatenar strings
"={{ 'buffer:' + $json.phoneNumber }}"
"={{ `Hola ${$json.nombre}, tu pedido es ${$json.pedido}` }}"
```

---

## Nodos Code — Estructura Correcta

```javascript
// CORRECTO: con try-catch y return array
try {
  const body = $input.first().json;
  const phoneNumber = body.messages?.[0]?.from;

  if (!phoneNumber) {
    return [{ json: { error: 'No phone number', stop: true } }];
  }

  return [{ json: { phoneNumber, processed: true } }];
} catch (error) {
  return [{ json: { error: error.message, stop: true } }];
}

// INCORRECTO
const phone = $json.from;  // $json no existe en Code nodes v2
return phone;  // debe retornar array de objetos con json
```

---

## Template de Parte JSON (para JSONCRAFT)

Cada parte del JSON debe seguir este formato de entrega:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PARTE [N]/[TOTAL] — [Nombre de la sección]
Nodos: [X] al [Y]
Conecta con PARTE [N+1] vía nodo "[Nombre del nodo conector]"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[JSON completo de los nodos de esta parte]

→ Responde "siguiente" para la PARTE [N+1]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Checklist de Validación JSON (pre-QA)

Antes de entregar un JSON, verificar:

- [ ] Todos los `id` son UUIDs válidos y únicos
- [ ] `name` del workflow incluye "aut8ma", nombre del cliente y nombre del workflow
- [ ] Todas las conexiones en `connections` referencian nombres de nodos que existen
- [ ] No hay credenciales reales hardcodeadas
- [ ] Nodos Code tienen try-catch
- [ ] Wait 10s post-webhook existe
- [ ] Respond Webhook está conectado en rama paralela desde el Webhook
- [ ] IF anti-bucle (fromMe) existe como nodo 4 o antes
- [ ] SessionKey usa phoneNumber (no estática)
- [ ] maxIterations del agente IA está definido (5-10)
- [ ] JSON no está truncado — si es largo, dividir en PARTES

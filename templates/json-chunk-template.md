# JSON Chunk Template

**Generado por**: JSONCRAFT (auto-json-builder)

---

## FORMATO DE ENTREGA (repetir para cada parte)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PARTE [N]/[TOTAL] — [Nombre de la sección]
Nodos: [X] al [Y]
Conecta con PARTE [N+1] vía nodo: "[Nombre exacto del nodo conector]"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

```json
{
  "name": "aut8ma — [NombreCliente] — Chatbot WhatsApp — Parte [N]",
  "nodes": [
    {
      "id": "uuid-único-1",
      "name": "Nombre Descriptivo Nodo 1",
      "type": "n8n-nodes-base.webhook",
      "typeVersion": 2,
      "position": [240, 300],
      "parameters": {
        "httpMethod": "POST",
        "path": "ycloud-webhook-[cliente-slug]",
        "responseMode": "responseNode"
      }
    },
    {
      "id": "uuid-único-2",
      "name": "Respond Webhook",
      "type": "n8n-nodes-base.respondToWebhook",
      "typeVersion": 1,
      "position": [460, 180],
      "parameters": {
        "respondWith": "text",
        "responseBody": "OK"
      }
    }
  ],
  "connections": {
    "Nombre Descriptivo Nodo 1": {
      "main": [
        [
          { "node": "Respond Webhook", "type": "main", "index": 0 },
          { "node": "Siguiente Nodo en Serie", "type": "main", "index": 0 }
        ]
      ]
    }
  },
  "active": false,
  "settings": { "executionOrder": "v1" },
  "staticData": null,
  "meta": { "templateCredsSetupCompleted": true },
  "pinData": {}
}
```

```
→ Importa esta parte en N8N.
→ Responde "siguiente" para continuar con la PARTE [N+1].
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## GUÍA DE ENSAMBLAJE (entregar al final de todas las partes)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
GUÍA DE ENSAMBLAJE — [NombreCliente] — Chatbot WhatsApp
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

ORDEN DE IMPORTACIÓN EN N8N:
1. Importar PARTE 1 (nodos 1-15)
2. Importar PARTE 2 (nodos 16-28)
3. Importar PARTE 3 (nodos 29-38)
4. Importar PARTE 4 (nodos 39-N)

CONEXIONES MANUALES (arrastrar en el canvas de N8N):

| Desde | Hacia | Instrucción |
|-------|-------|-------------|
| PARTE 1 / [Nodo X] | PARTE 2 / [Nodo Y] | Arrastrar el conector del output de "[X]" al input de "[Y]" |
| PARTE 2 / [Nodo A] | PARTE 3 / [Nodo B] | Arrastrar el conector del output de "[A]" al input de "[B]" |
| PARTE 3 / [Nodo C] | PARTE 4 / [Nodo D] | Arrastrar el conector del output de "[C]" al input de "[D]" |

CONFIGURAR CREDENCIALES EN N8N:
1. Ir a Credentials → Add Credential
2. Crear: PostgreSQL (con los datos del servidor)
3. Crear: Redis (localhost:6379)
4. Crear: HTTP Header Auth → nombre: "YCloud", header: X-API-Key, valor: [tu YCLOUD_API_KEY]
5. Ir a cada nodo que use credenciales y seleccionarla del dropdown

CHECKLIST FINAL ANTES DE ACTIVAR:
□ Todas las conexiones entre partes realizadas
□ Credenciales configuradas en todos los nodos que las necesitan
□ Variable de entorno CLIENT_ID configurada con el slug del cliente
□ Webhook URL copiada y configurada en YCloud
□ Test de "Hola" enviado → bot responde correctamente
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

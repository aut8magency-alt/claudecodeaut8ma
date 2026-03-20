# Configuracion LLM Gratuito — aut8ma

## Modelo Principal (Gratuito)
- **Modelo**: Qwen 2.5 Coder 7B
- **Runtime**: Ollama (local)
- **Endpoint**: http://localhost:11434/api/generate
- **Chat endpoint**: http://localhost:11434/api/chat
- **Sin costo**: Corre localmente, sin tokens, sin limites

## Como Usar Desde Agentes

### Llamada simple (generate)
```bash
curl http://localhost:11434/api/generate -d '{
  "model": "qwen2.5-coder:7b",
  "prompt": "Tu instruccion aqui",
  "stream": false
}'
```

### Llamada chat (multi-turn)
```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen2.5-coder:7b",
  "messages": [
    {"role": "system", "content": "Eres un asistente de aut8ma"},
    {"role": "user", "content": "Tu pregunta"}
  ],
  "stream": false
}'
```

### Desde Node.js
```javascript
async function askQwen(prompt, system = '') {
  const res = await fetch('http://localhost:11434/api/chat', {
    method: 'POST',
    body: JSON.stringify({
      model: 'qwen2.5-coder:7b',
      messages: [
        ...(system ? [{ role: 'system', content: system }] : []),
        { role: 'user', content: prompt }
      ],
      stream: false
    })
  });
  const data = await res.json();
  return data.message.content;
}
```

## Cuando Usar Cada LLM
| Tarea | LLM | Razon |
|-------|-----|-------|
| Generar JSON N8N | Qwen 2.5 Coder | Gratis, bueno con codigo |
| Escribir prompts | Claude (via agentes) | Mejor razonamiento |
| Ideas de contenido | Qwen 2.5 Coder | Gratis, rapido |
| Analisis de mercado | Claude (via agentes) | Mejor contexto |
| Guiones de video | Claude (via agentes) | Mejor creatividad |
| Tareas repetitivas | Qwen 2.5 Coder | Sin costo |

## Iniciar Ollama
```bash
ollama serve  # Inicia el servidor (puerto 11434)
ollama run qwen2.5-coder:7b  # Test interactivo
```

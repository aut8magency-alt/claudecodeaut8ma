# Delivery Document Template

> Template para: auto-delivery-formatter (PACKAGER)
> El agente ensambla este documento con todos los outputs del pipeline.
> Ver: knowledge/delivery-template.md para el template completo.

---

Este template es un atajo. El template completo y definitivo de entrega está en:
`knowledge/delivery-template.md`

El PACKAGER debe leer `knowledge/delivery-template.md` y usarlo como estructura
para ensamblar el documento final con los outputs de:
1. auto-intake (Prompt A)
2. auto-architect (arquitectura de nodos)
3. auto-json-builder (todas las partes JSON)
4. auto-prompt-writer (Prompt B)
5. Schema SQL de Supabase (estándar — incluir siempre)
6. Variables de entorno (según el cliente)
7. Checklist de instalación (estándar)
8. Checklist de pruebas (estándar)

El documento final debe ser un único bloque markdown completo,
listo para copiar y pegar por Tavo, sin ningún truncamiento.

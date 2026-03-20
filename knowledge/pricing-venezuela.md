# Calculadora de Precios y Descuentos — aut8ma

## APIs de Tasas de Cambio

| API | URL | Devuelve |
|-----|-----|----------|
| BCV Oficial | https://ve.dolarapi.com/v1/dolares | USD/Bs, EUR/Bs oficiales |
| Paralelo | https://ve.dolarapi.com/v1/dolares/paralelo | Tasa mercado paralelo |
| Binance P2P | https://ve.dolarapi.com/v1/dolares/cripto | USDT/Bs via Binance |
| Backup | https://api.exchangerate-api.com/v4/latest/USD | USD vs todas las monedas |

## Precios Base

### Plan Emprendedor
- Instalacion: $99 USD (unica vez)
- Mensualidad: desde $69 USD/mes
- Costo real: ~$8-15/mes → margen ~80%

### Plan Negocio
- Instalacion: $179 USD (unica vez)
- Mensualidad: desde $129 USD/mes
- Costo real: ~$20-35/mes → margen ~82%

### Plan Premium
- Instalacion: variable
- Mensualidad: desde $199 USD/mes
- Costo real: ~$50-80/mes → margen ~75%+

### Add-ons
| Add-on | Precio |
|--------|--------|
| Voz ElevenLabs | +$80/mes |
| Automatizacion extra | +$50/mes |
| Canal extra | +$40/mes |
| RAG adicional | +$60/mes |
| Dashboard | +$70/mes |

## Promociones Activas
- 30% descuento: pago en dolares efectivo
- 20% descuento: pago adelantado (un solo pago)
- 10 cupos limitados por mes
- 12 meses: 1 mes gratis + 20% permanente

## Contexto Economico Venezuela
- Alta inflacion — precios en Bs se desactualizan rapido
- Siempre cotizar en USD como referencia base
- Usar tasa paralela o Binance para Bs (mas real que BCV)
- Presupuesto tipico: $50-300/mes para tech

## Segmentos
| Segmento | Presupuesto | Plan Recomendado |
|----------|-------------|------------------|
| Micro-emprendedor | $30-80/mes | Emprendedor |
| PYME establecida | $80-200/mes | Negocio |
| Empresa mediana | $200-500/mes | Premium |
| Corporativo | $500+/mes | Premium + add-ons |

## Metodos de Pago Venezuela
- Zelle (preferido empresas con USD)
- Pago Movil en Bs
- Binance P2P / USDT
- Efectivo USD
- Transferencia bancaria USD

## Reglas de Precio
- SIEMPRE obtener tasas del dia antes de dar precio en Bs
- SIEMPRE USD primero, luego Bs
- SIEMPRE minimo 2 tasas (BCV + Binance)
- SIEMPRE mostrar margen de ganancia
- NUNCA usar solo tasa BCV para cotizar
- NUNCA dar precios en Bs sin verificar tasa

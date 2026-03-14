# CapCut Settings Reference — aut8ma Video Team

> Referencia para: video-capcut-configurator (CONFIGURA)
> Configuraciones técnicas para videos verticales (TikTok/Reels/Shorts)

---

## CONFIGURACIÓN DE PROYECTO

### Resolución y FPS
```
Resolución: 1080 × 1920 px (9:16 vertical — estándar TikTok/Reels)
FPS: 30 fps
Color Space: SDR (Rec.709)
```

### Configuración de Exportación
```
Formato: MP4
Codec video: H.264
Bitrate: 15 Mbps (alta calidad)
Codec audio: AAC
Bitrate audio: 320 kbps
Sample rate: 44.1 kHz
Estéreo: Sí
```

---

## ESTRUCTURA DE CAPAS DEL TIMELINE

Orden de capas de abajo hacia arriba:

| Capa | Contenido | Volumen |
|------|-----------|---------|
| 1 | 🎵 Música de fondo | -13 a -15 dB |
| 2 | 🔊 Efectos de sonido (SFX) | 0 dB (variar por efecto) |
| 3 | 🎤 Audio del presentador/VO | 0 dB (referencia) |
| 4 | 🎬 Video de fondo / B-roll | — |
| 5 | 👤 Video del presentador (si aplica) | — |
| 6 | 🎨 Overlays, gráficos, iconos | — |
| 7 | 💬 Subtítulos | — |

---

## CONFIGURACIÓN DE SUBTÍTULOS

### Fuente Principal
```
Fuente: Montserrat Black
  → Descargar gratis: fonts.google.com/specimen/Montserrat
  → En CapCut: buscar "Montserrat" en la biblioteca de fuentes
  → Alternativa si no disponible: Anton (Google Fonts)
```

### Subtítulos Normales
```
Tamaño: 85-95 px (en proyecto 1080×1920)
Color relleno: #FFFFFF (blanco puro)
Contorno (stroke): #000000 (negro), 4 px de grosor
Sombra: #000000, opacidad 40%, offset X: 3px, Y: 3px, blur: 2px
Posición: Centro horizontal, 68-72% del alto vertical
Máx. palabras por línea: 4
Máx. líneas simultáneas: 2
Animación entrada: Scale (de 80% a 100%) en 0.1s
Animación salida: Fade out en 0.05s
```

### Palabras Destacadas — Amarillo
```
Color: #FFE500
Tamaño: 120-135 px (30-40% más grande que el normal)
Mismo stroke negro 4px
Animación: Scale punch (100% → 115% → 100%) en 0.15s total
Usos: GRATIS, AUTOMÁTICO, números grandes ($200, 90%), resultados
```

### Palabras Destacadas — Rojo
```
Color: #FF3333
Tamaño: 110-120 px
Mismo stroke negro 4px
Animación: Scale punch
Usos: PROBLEMA, PIERDE, NUNCA MÁS, urgencia negativa
```

### Palabras Destacadas — Verde
```
Color: #00FF88
Tamaño: 110-120 px
Mismo stroke negro 4px
Animación: Scale punch
Usos: AHORA, SOLUCIÓN, resultados positivos
```

### Palabras Destacadas — Naranja
```
Color: #FF6B00
Tamaño: 110-120 px
Mismo stroke negro 4px
Animación: Scale punch
Usos: CTA, DESCUENTO, OFERTA
```

### Subtítulos de Shock (pantalla completa)
```
Tamaño: 130-150 px
Fondo: Rectángulo #000000 con opacidad 70%, padding 20px
Sin presentador en pantalla
Animación: Aparición instantánea (0 segundos de entrada)
SFX: Impact boom al aparecer
```

---

## AJUSTES DE COLOR (Color Grading)

### Para Video de Presentador
```
Brillo: 0 (sin cambio)
Contraste: +15
Saturación: +10
Temperatura: -5 (ligeramente fría)
Tinte: 0
Vibranza: +8
Sombras: +5
Luces: -5
```

### Vignette
```
Intensidad: -15 a -20
Radio: 70%
Redondez: 50%
Suavidad: 80%
```

### Para B-roll (hacer coincidir con el presentador)
```
Mismo ajuste de contraste y saturación
Temperatura puede variar ±3 según el clip
```

---

## CORTES Y TRANSICIONES

### Jump Cut (corte directo — el más usado)
```
Transición: Ninguna — corte directo en el timeline
Frecuencia: cada 2-4 segundos
Cuándo: entre frases o ideas diferentes
```

### Zoom Cut
```
Cómo hacerlo en CapCut:
1. Dividir el clip en el punto de corte
2. En el clip derecho: Velocidad → Curva → Inicio rápido (0.3s)
3. Aumentar escala del clip derecho +15% en el fotograma de corte
4. Agregar SFX Whoosh en el punto de corte
Cuándo: En momentos de énfasis o cambio de energía
```

### L-Cut (audio continúa sobre siguiente clip)
```
Cómo hacerlo en CapCut:
1. Dividir el clip de audio por separado del video
2. Extender el audio del clip anterior sobre el inicio del siguiente video
Cuándo: Transición suave de presentador a B-roll
```

---

## EFECTOS DE SONIDO — BIBLIOTECA

### Dónde conseguirlos (gratis)
- freesound.org (crear cuenta gratuita)
- pixabay.com/sound-effects/
- mixkit.co/free-sound-effects/

### Efectos Esenciales para aut8ma

| Nombre | Keywords de búsqueda | Cuándo usarlo |
|--------|---------------------|---------------|
| Impact Boom | "impact boom", "cinematic hit" | Palabras de alto impacto, datos sorprendentes |
| Whoosh | "whoosh transition", "air swoosh" | Zoom cuts, cambios de texto |
| Cash Register | "cash register", "cha-ching" | Resultados económicos, ventas, "GRATIS" |
| Keyboard Typing | "keyboard typing fast", "mechanical keyboard" | Automatización, IA, tecnología |
| Phone Notification | "phone notification", "message ping" | WhatsApp, mensajes, apps |
| Suspense Riser | "tension riser", "suspense build" | Antes de revelar problema o solución |
| Success Jingle | "success fanfare short", "achievement sound" | CTA final, resultados positivos |
| UI Click | "ui button click", "interface click" | Confirmar pasos, checkmarks |
| Crowd Cheer | "crowd cheer short" | Prueba social, resultados excepcionales |

---

## MÚSICA DE FONDO

### Características
```
BPM: 90-110 (energía media-alta)
Género: Hip-hop instrumental / Trap suave / Lo-fi beats modernos
Sin melodía prominente (textura de fondo, no protagonista)
Volumen: -13 a -15 dB durante el video
Sube a: -10 dB en los últimos 5 segundos (CTA)
Fade out: 0.5s al final
```

### Fuentes Gratuitas
- pixabay.com/music/ → buscar "hip hop instrumental background"
- youtube.com/audiolibrary → buscar "upbeat hip hop"
- freemusicarchive.org

### Keywords de Búsqueda
```
"motivational hip hop background"
"business success background music"
"upbeat trap instrumental no copyright"
"lo-fi hip hop energy"
```

---

## FONDOS DE VIDEO

### Estilo Víctor Heras
```
Tipo: Urbano / tecnología / negocios
Movimiento: Ken Burns lento (zoom in o parallax)
Velocidad: 0.3-0.5x de la velocidad original
Desenfoque: Gaussian blur 8-12px sobre el fondo
Overlay: Vignette oscura -20, más un overlay de color sólido con opacidad 20-30%
```

### Fuentes de B-roll (gratis)
- pexels.com/videos/
- pixabay.com/videos/
- coverr.co

### Keywords para Fondos
```
"city night lights"
"office business professional"
"technology abstract dark"
"smartphone money success"
"tropical venezuela caracas" (para contexto local)
```

---

## OVERLAYS Y GRÁFICOS

### Checkmarks (✅) con texto
```
Posición: Izquierda del texto
Color: #00FF88
Animación: Slide desde izquierda, 0.3s
Espacio entre items: 80px
```

### Banners de Urgencia
```
Fondo: #FF0000 semitransparente (opacidad 80%)
Texto: Blanco #FFFFFF, Montserrat Black
Posición: Parte inferior (últimos 20% del frame)
Animación: Fade in, luego parpadeo lento (opacity 80%↔100%, 1s)
```

### Iconos de WhatsApp
```
Usar PNG oficial de WhatsApp con fondo transparente
Tamaño: 80-100px
Animación: Bounce in desde abajo
```

---

## FLUJO DE TRABAJO CAPCUT (PASO A PASO)

1. **Crear proyecto**: 1080×1920, 30fps
2. **Importar clips**: Presentador, B-rolls, fondos
3. **Armar timeline**: Fondo → Presentador → B-rolls
4. **Color grading**: Aplicar ajustes de color al presentador
5. **Agregar música**: Capa inferior, ajustar a -13dB
6. **Auto Caption**: Generar subtítulos automáticos con CapCut AI
7. **Editar subtítulos**: Aplicar estilos, colores, tamaños por tarjeta
8. **Agregar SFX**: Sincronizar con momentos clave del blueprint
9. **Zoom cuts**: Aplicar en momentos de énfasis
10. **Overlays**: Agregar checkmarks, banners, iconos
11. **Revisión final**: Ver completo sin parar, ajustar timing
12. **Exportar**: MP4, 15Mbps, 30fps, AAC 320kbps

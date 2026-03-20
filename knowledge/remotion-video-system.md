# Sistema de Video Remotion — aut8ma

## Que es Remotion
Framework de React para crear videos programaticamente.
En lugar de editar manualmente en Adobe Premiere o CapCut,
defines el video como componentes React y Remotion lo renderiza a MP4.

## Capacidades
- Crear B-rolls animados con texto, imagenes y transiciones
- Subtitulos animados estilo Victor Heras (colores hex, timing)
- Generar intros, outros y transiciones automaticamente
- Renderizar a MP4 en cualquier resolucion (1080x1920 vertical)
- Componer multiples escenas en un video completo
- Agregar audio, musica de fondo y efectos de sonido

## Estructura de un Video Remotion
```
video-projects/
  src/
    Root.tsx           <- Composicion principal
    scenes/
      Hook.tsx         <- Escena del hook (0-3s)
      Problem.tsx      <- Escena del problema (3-15s)
      Story.tsx        <- Historia personal (15-30s)
      Solution.tsx     <- Solucion (30-45s)
      SocialProof.tsx  <- Prueba social (45-55s)
      CTA.tsx          <- Call to action (55-60s)
    components/
      AnimatedSubtitle.tsx  <- Subtitulos estilo VH
      BRoll.tsx             <- B-roll con zoom/pan
      Transition.tsx        <- Transiciones
    assets/
      audio/
      images/
```

## Configuracion Video Vertical (TikTok/Reels/Shorts)
```typescript
export const VIDEO_CONFIG = {
  width: 1080,
  height: 1920,
  fps: 30,
  durationInFrames: 1800, // 60 segundos
};
```

## Ejemplo: Subtitulo Animado Estilo Victor Heras
```tsx
import { useCurrentFrame, interpolate, spring } from 'remotion';

const AnimatedSubtitle = ({ text, color = '#FFFFFF', highlightWords = [] }) => {
  const frame = useCurrentFrame();
  const scale = spring({ frame, fps: 30, config: { damping: 12 } });

  return (
    <div style={{
      position: 'absolute',
      bottom: 200,
      width: '100%',
      textAlign: 'center',
      transform: `scale(${scale})`,
    }}>
      {text.split(' ').map((word, i) => (
        <span key={i} style={{
          fontFamily: 'Montserrat Black',
          fontSize: highlightWords.includes(word) ? 72 : 48,
          color: highlightWords.includes(word) ? color : '#FFFFFF',
          textShadow: '3px 3px 6px rgba(0,0,0,0.8)',
          margin: '0 6px',
        }}>{word} </span>
      ))}
    </div>
  );
};
```

## Comandos
```bash
# Preview en navegador
npx remotion preview src/index.ts

# Renderizar a MP4
npx remotion render src/index.ts MainVideo out/video.mp4

# Renderizar solo una escena
npx remotion render src/index.ts Hook out/hook.mp4
```

## Niveles de Instruccion desde Dashboard

### Simple
"Crea un B-roll de 5 segundos con texto 'aut8ma' sobre fondo oscuro"
-> Genera un componente React basico con texto animado

### Mediano
"Crea un video de 30 seg con hook sobre chatbots, subtitulos animados
amarillos en las palabras clave, y transicion zoom entre escenas"
-> Genera multiples escenas con la estructura Victor Heras

### Complejo
"Video completo de 60 seg estilo Victor Heras: hook de dolor sobre
perdida de clientes, historia personal, solucion aut8ma, prueba social
con datos, CTA con urgencia. B-rolls de graficos subiendo, WhatsApp
con notificaciones, persona frustrada"
-> Genera el proyecto Remotion completo con todas las escenas,
   subtitulos, B-rolls, SFX y musica

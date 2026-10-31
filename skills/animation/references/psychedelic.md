# Psychedelic Retro · `/animation psychedelic`

> Famille : Esthétiques artistiques · #57

## C'est quoi
Les années 70 : ondulations hypnotiques, spirales, typographies qui fondent, couleurs saturées chaudes (orange, rose, jaune, violet), effet « groovy ».

## Quand l'utiliser
- Musique, festival, rétro 70s, contenus fun et « trippy ».

## Technique clé
- Anneaux concentriques : `repeating-radial-gradient(circle at 50% 50%, #FF6B35 0 40px, #F7C59F 40px 80px, #EF476F 80px 120px, #7B2CBF 120px 160px)` dont le centre et la taille bougent → effet tunnel.
- Ondulation : filtre SVG `feTurbulence` basse fréquence (0.004) + `feDisplacementMap` (scale 60) appliqué au tout, `baseFrequency` qui oscille.
- Rotation lente de spirale : `conic-gradient` répété + `rotate(frame*1.5deg)`.
- Typo « groovy » ronde (« Shrikhand », « Monoton ») déformée par le même filtre.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

export const Psychedelic: React.FC<{text?: string}> = ({text = 'GROOVY'}) => {
  const frame = useCurrentFrame();
  const t = frame / 30;
  const shift = (frame * 4) % 160;
  return (
    <AbsoluteFill>
      <svg width={0} height={0}>
        <filter id="wavy">
          <feTurbulence type="turbulence" baseFrequency={0.004 + 0.002 * Math.sin(t)} numOctaves={2} seed={4} />
          <feDisplacementMap in="SourceGraphic" scale={70} xChannelSelector="R" yChannelSelector="G" />
        </filter>
      </svg>
      <AbsoluteFill style={{filter: 'url(#wavy)', transform: 'scale(1.15)',
        background: `repeating-radial-gradient(circle at ${50 + 10 * Math.sin(t)}% ${50 + 8 * Math.cos(t * 0.7)}%,
          #FF6B35 ${shift}px ${shift + 40}px, #F7C59F ${shift + 40}px ${shift + 80}px, #EF476F ${shift + 80}px ${shift + 120}px, #7B2CBF ${shift + 120}px ${shift + 160}px)`}} />
      <AbsoluteFill style={{justifyContent: 'center', alignItems: 'center', filter: 'url(#wavy)'}}>
        <div style={{fontFamily: '"Shrikhand", "Cooper Black", serif', fontSize: 190, color: '#FFE66D', WebkitTextStroke: '6px #3D0C45',
          paintOrder: 'stroke fill', textShadow: '10px 10px 0 #3D0C45', transform: `rotate(${Math.sin(t * 2) * 4}deg)`}}>{text}</div>
      </AbsoluteFill>
    </AbsoluteFill>
  );
};
```

## Variations
- Spirale hypnotique noir/blanc (`repeating-conic-gradient`) qui tourne.
- Fleurs 70s (marguerites) qui pulsent en rythme.
- Texte « liquide » qui coule vers le bas.

## Pièges
- Contrastes forts qui clignotent vite = risque photosensible : mouvements lents.
- `repeating-radial-gradient` animé + displacement sur 1080×1920 est lourd : tester le temps de rendu.

# Paper Cut-out · `/animation paper-cutout`

> Famille : Esthétiques artistiques · #47

## C'est quoi
Des formes en papier découpé, superposées en couches avec de petites ombres portées, animées en stop-motion.

## Quand l'utiliser
- Explainer artisanal, conte, éducation, marques douces / enfants.

## Technique clé
- Chaque couche : forme SVG pleine (vagues, collines, nuages) + `filter: drop-shadow(0 6px 6px rgba(0,0,0,.25))`.
- Texture papier : bruit `feTurbulence` très léger en `multiply` à 8 %, bords légèrement irréguliers (`feDisplacementMap` scale 3).
- Stop-motion : positions quantifiées à 8–12 i/s + petite rotation aléatoire ±1° par pose.
- Parallaxe des couches : chaque couche bouge à une vitesse différente (plus proche = plus vite).
- Palette pastel mate : `#F2C14E`, `#F78154`, `#4D9078`, `#B4436C`, `#5FAD56`.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

const LAYERS = ['#B4D8E7', '#4D9078', '#5FAD56', '#2F6B4F'];

export const PaperCutout: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const f = Math.floor(frame / 3); // 10 i/s
  return (
    <AbsoluteFill style={{background: '#FCE9C8'}}>
      <div style={{position: 'absolute', left: 640, top: 380 + Math.sin(f * 0.2) * 10, width: 220, height: 220, borderRadius: '50%',
        background: '#F2C14E', filter: 'drop-shadow(0 6px 6px rgba(0,0,0,.25))', transform: `rotate(${(random(`s${f}`) - 0.5) * 2}deg)`}} />
      <svg width={1080} height={1920} style={{position: 'absolute', inset: 0}}>
        {LAYERS.map((c, i) => {
          const y = 900 + i * 220;
          const shift = (f * (i + 1) * 4) % 400;
          let d = `M ${-400 - shift} 1920 L ${-400 - shift} ${y}`;
          for (let x = -400; x <= 1500; x += 200) d += ` Q ${x + 100 - shift} ${y - 90 + (i % 2) * 40} ${x + 200 - shift} ${y}`;
          d += ' L 1500 1920 Z';
          return <path key={i} d={d} fill={c} style={{filter: 'drop-shadow(0 -6px 8px rgba(0,0,0,.22))'}}
            transform={`rotate(${(random(`r${i}${f}`) - 0.5) * 0.6} 540 ${y})`} />;
        })}
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Personnages en papier articulés (attaches parisiennes) : membres qui pivotent par à-coups.
- Pop-up book : couches qui se déplient en `rotateX` depuis le bas.
- Texte découpé dans une couche (masque) avec la couche suivante visible à travers.

## Pièges
- Ombres trop floues = effet numérique. Ombres courtes et nettes.
- Mouvement fluide à 30 i/s = perte du charme fait main.

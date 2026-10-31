# Aurora & Mesh Gradient · `/animation aurora`

> Famille : Mouvements organiques · #20

## C'est quoi
De grandes taches de couleur floues qui dérivent et se mélangent lentement comme une aurore boréale ou un « mesh gradient » Apple.

## Quand l'utiliser
- Fond premium derrière un titre, une carte en verre, une interface.
- Ambiance calme, tech, luxe.

## Technique clé
- 4 à 6 cercles (`div` ronds de 600–900 px) en couleurs vives, `filter: blur(120px)`, `mix-blend-mode: screen` sur fond très sombre.
- Trajectoires de Lissajous : `x = cx + A*sin(t*f1 + φ)`, `y = cy + B*cos(t*f2 + φ)` avec `t = frame/30`, fréquences 0.2–0.5.
- Grain léger par-dessus (SVG `feTurbulence` à 6 % d'opacité) pour éviter le banding.
- Palette : `#00B5C7`, `#A855F7`, `#EC4899`, `#155E75`.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

const BLOBS = [
  {c: '#00B5C7', x: 300, y: 600, f: 0.31, p: 0},
  {c: '#A855F7', x: 760, y: 900, f: 0.23, p: 2},
  {c: '#EC4899', x: 420, y: 1300, f: 0.37, p: 4},
  {c: '#155E75', x: 700, y: 400, f: 0.19, p: 1},
];

export const Aurora: React.FC<{text?: string}> = ({text = 'AURORA'}) => {
  const t = useCurrentFrame() / 30;
  return (
    <AbsoluteFill style={{background: '#020210', overflow: 'hidden'}}>
      {BLOBS.map((b, i) => (
        <div key={i} style={{
          position: 'absolute', width: 800, height: 800, borderRadius: '50%', background: b.c, opacity: 0.7,
          left: b.x - 400 + 220 * Math.sin(t * b.f + b.p), top: b.y - 400 + 260 * Math.cos(t * b.f * 1.3 + b.p),
          filter: 'blur(120px)', mixBlendMode: 'screen',
        }} />
      ))}
      <AbsoluteFill style={{justifyContent: 'center', alignItems: 'center', fontFamily: 'Inter, sans-serif', fontWeight: 800, fontSize: 120, color: '#fff', letterSpacing: 12}}>
        {text}
      </AbsoluteFill>
    </AbsoluteFill>
  );
};
```

## Variations
- Rideaux d'aurore : bandes verticales étirées (`scaleY(3)`) en vert/cyan qui ondulent.
- Mesh gradient net : moins de flou (60px) + plus de taches pour un rendu « Stripe ».
- Shader WebGL (`@remotion/three`) pour un bruit de couleur continu.

## Pièges
- `blur(120px)` sur de grands éléments est coûteux : réduire la taille et augmenter `scale`.
- Banding (escaliers de couleur) en H.264 : ajouter du grain et encoder avec `crf` ≤ 18.

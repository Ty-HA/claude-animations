# Cube / Box Transition · `/animation cube-transition`

> Famille : Styles 3D · #40

## C'est quoi
Passer d'une scène à l'autre comme si on faisait tourner un cube : la scène A est sur une face, la scène B sur la face voisine.

## Quand l'utiliser
- Transition entre deux plans, enchaînement de stories, avant/après.

## Technique clé
- Cube de largeur `W` : face A `rotateY(0) translateZ(W/2)`, face B `rotateY(90deg) translateZ(W/2)`.
- Conteneur : `perspective: 2000px` ; le cube tourne de `0 → -90°` autour de son centre, en `translateZ(-W/2)` pour pivoter sur l'axe central.
- Durée 15–20 frames, `Easing.inOut(Easing.cubic)`.
- Recul pendant la rotation : `scale(1 - 0.15 * sin(π p))` pour voir le volume.
- Assombrir la face qui s'éloigne (`brightness 1 → 0.5`).

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame, useVideoConfig} from 'remotion';

export const CubeTransition: React.FC<{text?: string; a?: React.ReactNode; b?: React.ReactNode; at?: number}> = ({
  at = 30,
  a = <AbsoluteFill style={{background: '#00B5C7', justifyContent: 'center', alignItems: 'center', fontSize: 140, color: '#fff', fontFamily: 'Inter', fontWeight: 900}}>AVANT</AbsoluteFill>,
  b = <AbsoluteFill style={{background: '#A855F7', justifyContent: 'center', alignItems: 'center', fontSize: 140, color: '#fff', fontFamily: 'Inter', fontWeight: 900}}>APRÈS</AbsoluteFill>,
}) => {
  const frame = useCurrentFrame();
  const {width: W} = useVideoConfig();
  const p = interpolate(frame, [at, at + 18], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp', easing: Easing.inOut(Easing.cubic)});
  const face = (rot: number, dim: number): React.CSSProperties => ({
    position: 'absolute', inset: 0, backfaceVisibility: 'hidden', transform: `rotateY(${rot}deg) translateZ(${W / 2}px)`, filter: `brightness(${dim})`,
  });
  return (
    <AbsoluteFill style={{background: '#000', perspective: 2000}}>
      <AbsoluteFill style={{transformStyle: 'preserve-3d', transform: `scale(${1 - 0.15 * Math.sin(Math.PI * p)}) translateZ(${-W / 2}px) rotateY(${-90 * p}deg)`}}>
        <div style={face(0, 1 - 0.5 * p)}>{a}</div>
        <div style={face(90, 0.5 + 0.5 * p)}>{b}</div>
      </AbsoluteFill>
    </AbsoluteFill>
  );
};
```

## Variations
- Rotation verticale (`rotateX`) pour un enchaînement « vers le haut ».
- Enchaîner 4 scènes sur les 4 faces (rotation de 90° à chaque fois).
- Version `@remotion/transitions` : écrire une `TransitionPresentation` réutilisable.

## Pièges
- Oublier `translateZ(-W/2)` : le cube pivote sur un bord au lieu du centre.
- Les vidéos dans des faces 3D sont coûteuses en rendu : garder la transition courte.

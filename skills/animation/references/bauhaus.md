# Bauhaus · `/animation bauhaus`

> Famille : Esthétiques artistiques · #49

## C'est quoi
Composition à partir de formes pures (cercle, carré, triangle, demi-cercle) en couleurs primaires (rouge, jaune, bleu) + noir et crème, qui se combinent et se réarrangent.

## Quand l'utiliser
- Contenus design/archi, affiches culturelles, motifs de marque géométriques.

## Technique clé
- Palette : `#E63946` rouge, `#F4C430` jaune, `#1D4E89` bleu, `#111` noir, fond `#F1E9DA`.
- Grille 3×4 de cases ; chaque case contient une forme (cercle, quart de cercle, triangle, demi-disque).
- Animation : rotations par pas de 90° (`spring`), formes qui glissent d'une case à l'autre, échange de couleurs.
- Quart de cercle : `borderRadius: '100% 0 0 0'` ; demi-cercle : `borderRadius: '999px 999px 0 0'` ; triangle : `clipPath: polygon(...)`.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, spring, useCurrentFrame, useVideoConfig} from 'remotion';

const COLORS = ['#E63946', '#F4C430', '#1D4E89', '#111'];
const SHAPES: React.CSSProperties[] = [
  {borderRadius: '50%'}, {borderRadius: '100% 0 0 0'}, {clipPath: 'polygon(0 100%, 50% 0, 100% 100%)'}, {borderRadius: '999px 999px 0 0', height: '50%', marginTop: '50%'}, {},
];

export const Bauhaus: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const step = Math.floor(frame / 20);
  const C = 260;
  return (
    <AbsoluteFill style={{background: '#F1E9DA', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{display: 'grid', gridTemplateColumns: `repeat(3, ${C}px)`, gap: 0}}>
        {Array.from({length: 15}, (_, i) => {
          const s = spring({frame: frame - step * 20 - (i % 3) * 2, fps, config: {damping: 14}});
          const rot = (Math.floor(random(`r${i}${step}`) * 4) + s - 1) * 90;
          return (
            <div key={i} style={{width: C, height: C, overflow: 'hidden'}}>
              <div style={{width: '100%', height: '100%', background: COLORS[(i + step) % 4], ...SHAPES[Math.floor(random(`s${i}`) * SHAPES.length)],
                transform: `rotate(${rot}deg)`}} />
            </div>
          );
        })}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Typographie Bauhaus (police géométrique, minuscules) intégrée à la grille.
- Composition qui se construit forme par forme sur un beat musical.

## Pièges
- Couleurs pastel ou dégradés = plus du tout Bauhaus. Couleurs pleines et plates.
- Rotations continues : préférer les pas nets de 90°.

# Risograph & Halftone · `/animation risograph`

> Famille : Esthétiques artistiques · #46

## C'est quoi
Le rendu d'impression risographie : encres vives (rose fluo, bleu, jaune), trame de points (halftone), calques légèrement décalés, grain, animation saccadée à 12 i/s.

## Quand l'utiliser
- Fanzine, affiche d'événement, illustration indé, culture musique/art.

## Technique clé
- 2 calques de couleur (`#FF48B0` fluo pink, `#0078BF` riso blue) en `mixBlendMode: 'multiply'` sur papier `#F5F0E6`, décalés de 4–8 px (mauvais repérage), décalage qui « tremble » à chaque image.
- **Halftone** : `radial-gradient(circle, couleur 35%, transparent 36%)` en `backgroundSize: 14px 14px`, masqué par la forme ; ou filtre SVG.
- **12 i/s** : `const f = Math.floor(frame / 2.5)` utilisé pour tout mouvement et jitter.
- Grain papier : `feTurbulence` baseFrequency 0.9 en `multiply` 20 %.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

export const Risograph: React.FC<{text?: string}> = ({text = 'RISO\nFEST'}) => {
  const frame = useCurrentFrame();
  const f = Math.floor(frame / 2.5);
  const j = (k: string) => (random(`${k}${f}`) - 0.5) * 8;
  const layer = (color: string, dx: number, dy: number, dots: boolean): React.CSSProperties => ({
    position: 'absolute', inset: 0, display: 'flex', justifyContent: 'center', alignItems: 'center', whiteSpace: 'pre',
    fontFamily: '"Arial Black", Impact', fontSize: 280, lineHeight: 0.85, textAlign: 'center',
    transform: `translate(${dx}px, ${dy}px) rotate(${Math.sin(f * 0.5) * 2}deg)`, mixBlendMode: 'multiply',
    ...(dots
      ? {backgroundImage: `radial-gradient(circle, ${color} 38%, transparent 40%)`, backgroundSize: '14px 14px', WebkitBackgroundClip: 'text', color: 'transparent'}
      : {color}),
  });
  return (
    <AbsoluteFill style={{background: '#F5F0E6'}}>
      <div style={layer('#0078BF', j('a'), j('b'), true)}>{text}</div>
      <div style={layer('#FF48B0', 10 + j('c'), 8 + j('d'), false)}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Photo en trame : filtre SVG `feColorMatrix` en niveaux de gris + seuil par motif de points.
- 3 couleurs (ajouter jaune `#FFE800`) pour des zones de superposition orange/vert.

## Pièges
- Mélange `normal` au lieu de `multiply` = perte totale de l'effet encre.
- Animation fluide à 30 i/s : ça ne fait plus « imprimé ». Quantifier.

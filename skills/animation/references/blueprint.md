# Blueprint · `/animation blueprint`

> Famille : Esthétiques artistiques · #54

## C'est quoi
Un plan d'architecte ou d'ingénieur qui se trace en direct : lignes blanches fines sur fond bleu, grille, cotes et annotations.

## Quand l'utiliser
- Expliquer comment c'est construit, coulisses d'un produit, architecture, « le plan ».

## Technique clé
- Fond `#0B3D91` + grille : `linear-gradient(rgba(255,255,255,.12) 1px, transparent 1px)` en 40 px et une grille majeure 200 px plus marquée.
- Traits : blanc 2–3 px, tracés avec `strokeDasharray/Dashoffset` en séquence (chaque élément démarre quand le précédent finit).
- Cotes : lignes fines avec flèches (`marker-end`) + texte monospace (`1200 mm`).
- Police technique : « Architects Daughter » pour les notes manuscrites, monospace pour les cotes.
- Utiliser `pathLength={1}` sur chaque élément SVG pour animer `strokeDashoffset` de 1 → 0 sans mesurer.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, useCurrentFrame} from 'remotion';

const DRAW = [
  {d: 'M240 1300 L240 800 L540 560 L840 800 L840 1300 Z', start: 0},
  {d: 'M460 1300 L460 1080 L620 1080 L620 1300', start: 25},
  {d: 'M300 860 L420 860 L420 980 L300 980 Z', start: 35},
  {d: 'M660 860 L780 860 L780 980 L660 980 Z', start: 42},
  {d: 'M240 1380 L840 1380', start: 55},
];

export const Blueprint: React.FC<{text?: string}> = ({text = 'MAISON A — 6000 mm'}) => {
  const frame = useCurrentFrame();
  const grid = 'linear-gradient(rgba(255,255,255,.1) 1px, transparent 1px), linear-gradient(90deg, rgba(255,255,255,.1) 1px, transparent 1px)';
  return (
    <AbsoluteFill style={{background: '#0B3D91', backgroundImage: grid, backgroundSize: '40px 40px'}}>
      <svg width={1080} height={1920}>
        {DRAW.map(({d, start}, i) => (
          <path key={i} d={d} pathLength={1} fill="none" stroke="#fff" strokeWidth={3} strokeDasharray={1}
            strokeDashoffset={interpolate(frame, [start, start + 25], [1, 0], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'})} />
        ))}
        <text x={540} y={1430} textAnchor="middle" fill="#fff" fontFamily="monospace" fontSize={34}
          opacity={interpolate(frame, [70, 80], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'})}>{text}</text>
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Transformation blueprint → rendu réel : fondu enchaîné vers la photo du produit.
- Vue éclatée : pièces qui s'écartent avec leurs cotes.
- Cercle de détail (loupe) qui zoome sur une partie.

## Pièges
- `pathLength={1}` + `strokeDasharray={1}` : bien mettre les deux, sinon pointillés.
- Trop d'éléments tracés en même temps = chaos. Séquencer.

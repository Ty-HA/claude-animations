# Blobs & Metaballs · `/animation metaballs`

> Famille : Mouvements organiques · #21

## C'est quoi
Des boules qui se rapprochent et **fusionnent** comme du mercure ou une lampe à lave, avec des ponts visqueux entre elles.

## Quand l'utiliser
- Fonds organiques, loaders, transitions « liquides », logos qui se forment.

## Technique clé
- Le filtre **gooey** SVG : flou puis seuil d'alpha.
  ```xml
  <filter id="goo">
    <feGaussianBlur in="SourceGraphic" stdDeviation="22" result="b"/>
    <feColorMatrix in="b" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 40 -18"/>
  </filter>
  ```
- Le `40 -18` de la matrice règle la netteté du bord (plus grand = plus net).
- Cercles sur trajectoires sinusoïdales autour du centre, rayons 60–140 px.
- Dégradé appliqué **après** le filtre (groupe parent) pour une couleur continue.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

export const Metaballs: React.FC<{text?: string; count?: number}> = ({count = 9}) => {
  const t = useCurrentFrame() / 30;
  return (
    <AbsoluteFill style={{background: '#0b0618', justifyContent: 'center', alignItems: 'center'}}>
      <svg width={900} height={900}>
        <defs>
          <filter id="goo">
            <feGaussianBlur in="SourceGraphic" stdDeviation="22" result="b" />
            <feColorMatrix in="b" mode="matrix" values="1 0 0 0 0  0 1 0 0 0  0 0 1 0 0  0 0 0 40 -18" />
          </filter>
          <linearGradient id="g" x1="0" y1="0" x2="1" y2="1">
            <stop offset="0" stopColor="#00B5C7" /><stop offset="1" stopColor="#A855F7" />
          </linearGradient>
        </defs>
        <g filter="url(#goo)" fill="url(#g)">
          {Array.from({length: count}, (_, i) => {
            const a = random(`a${i}`) * 6.28, f = 0.4 + random(`f${i}`) * 0.8, R = 80 + random(`R${i}`) * 220;
            return <circle key={i} cx={450 + Math.cos(a + t * f) * R} cy={450 + Math.sin(a * 1.3 + t * f * 1.2) * R} r={60 + random(`r${i}`) * 80} />;
          })}
        </g>
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Lampe à lave : boules qui montent/descendent verticalement dans un tube, couleurs orange/rouge.
- Texte gooey : appliquer le filtre à un texte dont les lettres s'écartent puis se rejoignent.
- Interaction web : une boule suit le curseur.

## Pièges
- Le dégradé appliqué sur chaque cercle avant le filtre crée des coutures : mettre `fill` sur le groupe.
- `stdDeviation` doit être proportionnel à la taille des cercles, sinon pas de fusion.

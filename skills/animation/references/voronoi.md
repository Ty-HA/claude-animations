# Voronoi Cells · `/animation voronoi`

> Famille : Mouvements organiques · #26

## C'est quoi
Le plan découpé en cellules organiques (comme des cellules biologiques, de la peau de girafe ou un vitrail) qui bougent et se redessinent.

## Quand l'utiliser
- Fond bio/science, motif génératif, transition par fragmentation.

## Technique clé
- Lib : `d3-delaunay` → `Delaunay.from(points).voronoi([0,0,W,H])`, puis `voronoi.renderCell(i)` donne le path SVG de chaque cellule.
- Points qui dérivent : `x = x0 + 40*sin(t*f + φ)`.
- Style : remplissage par cellule (teinte selon index), contour sombre 4 px ; ou seulement les contours en néon.
- Effet « cellule vivante » : `strokeLinejoin: round` + léger arrondi via filtre gooey inversé.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';
import {Delaunay} from 'd3-delaunay';

export const Voronoi: React.FC<{text?: string; count?: number}> = ({count = 45}) => {
  const t = useCurrentFrame() / 30;
  const W = 1080, H = 1920;
  const pts = Array.from({length: count}, (_, i) => [
    random(`x${i}`) * W + 60 * Math.sin(t * (0.3 + random(`f${i}`)) + i),
    random(`y${i}`) * H + 60 * Math.cos(t * (0.3 + random(`g${i}`)) + i),
  ] as [number, number]);
  const v = Delaunay.from(pts).voronoi([0, 0, W, H]);
  return (
    <AbsoluteFill style={{background: '#0a0a12'}}>
      <svg width={W} height={H}>
        {pts.map((_, i) => (
          <path key={i} d={v.renderCell(i)} fill={`hsl(${170 + random(`c${i}`) * 110} 70% ${25 + random(`l${i}`) * 25}%)`}
            stroke="#0a0a12" strokeWidth={6} strokeLinejoin="round" />
        ))}
      </svg>
    </AbsoluteFill>
  );
};
```
Dépendance : `npm i d3-delaunay`.

## Variations
- Fragmentation : chaque cellule tombe/tourne pour révéler la scène suivante (transition « verre brisé »).
- Points relaxés (Lloyd) pour des cellules régulières façon nid d'abeille irrégulier.
- Contours seuls en cyan néon sur noir.

## Pièges
- Points hors du cadre : `renderCell` peut renvoyer `null`, tester avant de rendre.
- Trop de cellules (>300) = mosaïque illisible en 9:16.

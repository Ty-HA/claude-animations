# Low Poly · `/animation low-poly`

> Famille : Styles 3D · #34

## C'est quoi
Des objets ou paysages faits de grandes facettes triangulaires, chacune d'une teinte unie, qui accrochent la lumière.

## Quand l'utiliser
- Paysages stylisés, fonds géométriques, logos facettés, jeux.

## Technique clé
- Triangulation : points aléatoires (seedés) + bords → `d3-delaunay` → triangles.
- Couleur de chaque triangle = couleur d'un dégradé au centroïde, éclaircie selon une « lumière » : `L = base + 15 * sin(angle normal + t)`.
- Animation : légère ondulation des sommets intérieurs (`± 10 px`) et lumière qui tourne → les facettes scintillent.
- Contours : même couleur que la face (pas de trait) ou trait 1 px un peu plus clair.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';
import {Delaunay} from 'd3-delaunay';

const W = 1080, H = 1920;
const BASE = [
  ...Array.from({length: 90}, (_, i) => [random(`x${i}`) * W, random(`y${i}`) * H]),
  [0, 0], [W, 0], [0, H], [W, H], [W / 2, 0], [W / 2, H], [0, H / 2], [W, H / 2],
] as [number, number][];

export const LowPoly: React.FC<{text?: string}> = () => {
  const t = useCurrentFrame() / 30;
  const pts = BASE.map(([x, y], i) => (i < 90 ? [x + 10 * Math.sin(t + i), y + 10 * Math.cos(t * 1.2 + i)] : [x, y]) as [number, number]);
  const {triangles} = Delaunay.from(pts);
  const tris = [];
  for (let k = 0; k < triangles.length; k += 3) {
    const [a, b, c] = [pts[triangles[k]], pts[triangles[k + 1]], pts[triangles[k + 2]]];
    const cy = (a[1] + b[1] + c[1]) / 3, cx = (a[0] + b[0] + c[0]) / 3;
    const light = 12 * Math.sin(cx * 0.01 + cy * 0.006 + t * 1.5);
    tris.push(<polygon key={k} points={`${a} ${b} ${c}`} fill={`hsl(${250 - (cy / H) * 70} 60% ${30 + light + (cy / H) * 20}%)`}
      stroke={`hsl(${250 - (cy / H) * 70} 60% ${30 + light + (cy / H) * 20}%)`} strokeWidth={1} />);
  }
  return <AbsoluteFill><svg width={W} height={H}>{tris}</svg></AbsoluteFill>;
};
```
Dépendance : `npm i d3-delaunay`.

## Variations
- Montagnes : points plus denses en bas + silhouette, palette coucher de soleil.
- Objet 3D low poly : `@remotion/three` + `flatShading: true` sur une `IcosahedronGeometry(1, 1)`.
- Portrait low poly : couleur des triangles échantillonnée dans une photo.

## Pièges
- Triangles très allongés = moche : répartir les points (Poisson disk) plutôt qu'uniformément.
- Sans trait de même couleur, de fines lignes de fond apparaissent entre triangles (anti-aliasing).

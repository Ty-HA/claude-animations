# Voxel Art · `/animation voxel`

> Famille : Styles 3D · #44

## C'est quoi
De la 3D construite en petits cubes, façon Minecraft ou MagicaVoxel : personnages, îles, objets en blocs.

## Quand l'utiliser
- Gaming, univers ludique, construction progressive (« on bâtit ton projet bloc par bloc »).

## Technique clé
- Cube isométrique en SVG : 3 losanges (dessus clair, gauche moyen, droite sombre). Pour un cube en (x, y, z) de taille `s` : origine écran `X = (x - y) * s * 0.866`, `Y = (x + y) * s * 0.5 - z * s`.
- **Ordre de dessin** : trier par `x + y + z` croissant (peintre).
- Construction animée : chaque cube tombe à son tour (`delay = index * 2`), `translateY(-300 → 0)` avec `spring`.
- Couleurs : 3 nuances par matériau (dessus +15 % luminosité, droite −20 %).

## Snippet Remotion
```tsx
import {AbsoluteFill, spring, useCurrentFrame, useVideoConfig} from 'remotion';

const S = 56;
const VOX: [number, number, number, string][] = [];
for (let x = 0; x < 5; x++) for (let y = 0; y < 5; y++) VOX.push([x, y, 0, (x + y) % 2 ? '#5CC46B' : '#4CB35B']);
[[2, 2, 1], [2, 2, 2], [2, 2, 3]].forEach(([x, y, z]) => VOX.push([x, y, z, '#8B5A2B']));
[[1, 2, 4], [3, 2, 4], [2, 1, 4], [2, 3, 4], [2, 2, 4], [2, 2, 5]].forEach(([x, y, z]) => VOX.push([x, y, z, '#2E9E4F']));
VOX.sort((a, b) => a[0] + a[1] + a[2] - (b[0] + b[1] + b[2]));

const shade = (hex: string, k: number) => `color-mix(in srgb, ${hex} ${100 - Math.abs(k)}%, ${k > 0 ? '#fff' : '#000'})`;

export const Voxel: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const w = S * 0.866, h = S * 0.5;
  return (
    <AbsoluteFill style={{background: '#87CEEB'}}>
      <svg width={1080} height={1920}>
        <g transform="translate(540 1050)">
          {VOX.map(([x, y, z, c], i) => {
            const s = spring({frame: frame - i * 2, fps, config: {damping: 13}});
            if (frame < i * 2) return null;
            const X = (x - y) * w, Y = (x + y) * h - z * S - (1 - s) * 300;
            return (
              <g key={i} transform={`translate(${X} ${Y})`}>
                <polygon points={`0,${-h} ${w},0 0,${h} ${-w},0`} fill={shade(c, 15)} />
                <polygon points={`${-w},0 0,${h} 0,${h + S} ${-w},${S}`} fill={c} />
                <polygon points={`${w},0 0,${h} 0,${h + S} ${w},${S}`} fill={shade(c, -25)} />
              </g>
            );
          })}
        </g>
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Rotation de la scène par pas de 90° (réindexer x/y).
- Personnage voxel qui saute (groupe de cubes déplacé ensemble).
- Vraie 3D : `@remotion/three` + `InstancedMesh` de `BoxGeometry`.

## Pièges
- Mauvais tri = cubes qui passent devant ceux qui devraient les cacher.
- `color-mix` exige Chrome récent (OK dans Remotion) ; sinon précalculer les 3 teintes.

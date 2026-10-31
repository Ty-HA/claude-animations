# Isometric & Wireframe 3D · `/animation wireframe`

> Famille : Styles 3D · #32

## C'est quoi
Des objets 3D réduits à leurs arêtes (fil de fer) qui tournent dans l'espace, ou des scènes en perspective isométrique propre.

## Quand l'utiliser
- Tech, architecture, produit « en cours de conception », univers sci-fi / hologramme.

## Technique clé
- **Projection maison** (sans lib) : sommets 3D → rotation (matrices Y puis X) → perspective `x' = x * f / (z + d)`, `f = 900`, `d = 4`.
- Arêtes = paires d'indices → `<line>` SVG. Couleur cyan `#67E8F9`, `strokeWidth` 2, glow `drop-shadow`.
- Profondeur : opacité de l'arête selon z moyen (loin = 0.3).
- **Isométrique** : pas de perspective, `x' = (x - z) * cos30°`, `y' = y + (x + z) * sin30°`.
- Rotation : `ay = frame * 0.03`, `ax = 0.5 + 0.2 * sin(frame * 0.02)`.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

// Icosaèdre
const P = (1 + Math.sqrt(5)) / 2;
const V = [[-1, P, 0], [1, P, 0], [-1, -P, 0], [1, -P, 0], [0, -1, P], [0, 1, P], [0, -1, -P], [0, 1, -P], [P, 0, -1], [P, 0, 1], [-P, 0, -1], [-P, 0, 1]];
const E: [number, number][] = [];
V.forEach((a, i) => V.forEach((b, j) => { if (j > i && Math.hypot(a[0] - b[0], a[1] - b[1], a[2] - b[2]) < 2.1) E.push([i, j]); }));

export const Wireframe: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const ay = frame * 0.03, ax = 0.5 + 0.2 * Math.sin(frame * 0.02);
  const proj = V.map(([x, y, z]) => {
    const x1 = x * Math.cos(ay) + z * Math.sin(ay), z1 = -x * Math.sin(ay) + z * Math.cos(ay);
    const y2 = y * Math.cos(ax) - z1 * Math.sin(ax), z2 = y * Math.sin(ax) + z1 * Math.cos(ax);
    const k = 900 / (z2 + 6);
    return {x: 540 + x1 * k * 0.9, y: 960 + y2 * k * 0.9, z: z2};
  });
  return (
    <AbsoluteFill style={{background: '#010012'}}>
      <svg width={1080} height={1920} style={{filter: 'drop-shadow(0 0 8px #00B5C7)'}}>
        {E.map(([a, b], i) => (
          <line key={i} x1={proj[a].x} y1={proj[a].y} x2={proj[b].x} y2={proj[b].y} stroke="#67E8F9" strokeWidth={2.5}
            opacity={0.35 + 0.65 * (1 - (proj[a].z + proj[b].z + 4) / 8)} />
        ))}
        {proj.map((p, i) => <circle key={i} cx={p.x} cy={p.y} r={6} fill="#A5F3FC" />)}
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Grille de sol en perspective qui défile (voir `synthwave`).
- Charger un vrai modèle : `@remotion/three` + `<lineSegments>` avec `EdgesGeometry`.
- Ville isométrique : cubes en `polygon` avec 3 teintes par face.

## Pièges
- Rotation calculée à partir de l'état précédent = dérive. Toujours partir des sommets d'origine.
- Trop d'arêtes = brouillard de lignes : < 200 arêtes visibles.

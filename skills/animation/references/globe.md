# 3D Globe · `/animation globe`

> Famille : Styles 3D · #41

## C'est quoi
Un globe terrestre qui tourne, avec des points lumineux sur les villes et des arcs qui relient les continents.

## Quand l'utiliser
- International, logistique, communauté mondiale, stats par pays, voyage.

## Technique clé
- **Globe en points (SVG, sans lib)** : grille lat/lon tous les 4–6°, projection orthographique : `x = R cos(lat) sin(lon - λ)`, `y = -R sin(lat)`, visible si `cos(lat) cos(lon - λ) > 0`.
- Rotation : `λ = frame * 0.6°`.
- Points de terre réels : utiliser un masque de continents (GeoJSON `world-110m` + `d3-geo` `geoOrthographic().rotate([-λ, -15])` + `geoPath`).
- Arcs : courbe de Bézier entre deux villes projetées, point de contrôle surélevé, `strokeDashoffset` animé.
- Atmosphère : `radial-gradient` cyan transparent autour du disque.

## Snippet Remotion (d3-geo)
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';
import {geoGraticule10, geoOrthographic, geoPath} from 'd3-geo';

const CITIES: [number, number][] = [[2.35, 48.85], [-74, 40.7], [139.7, 35.7], [-46.6, -23.5], [151.2, -33.9]];

export const Globe: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const proj = geoOrthographic().scale(380).translate([540, 960]).rotate([-frame * 0.6, -15]).clipAngle(90);
  const path = geoPath(proj);
  return (
    <AbsoluteFill style={{background: '#010012'}}>
      <svg width={1080} height={1920}>
        <circle cx={540} cy={960} r={420} fill="url(#atm)" />
        <defs><radialGradient id="atm"><stop offset="0.88" stopColor="#00B5C7" stopOpacity={0.35} /><stop offset="1" stopColor="#00B5C7" stopOpacity={0} /></radialGradient></defs>
        <circle cx={540} cy={960} r={380} fill="#04122a" />
        <path d={path(geoGraticule10()) ?? ''} fill="none" stroke="#155E75" strokeWidth={1.2} />
        {CITIES.map((c, i) => {
          const p = proj(c);
          const visible = path({type: 'Point', coordinates: c});
          return p && visible ? <circle key={i} cx={p[0]} cy={p[1]} r={9} fill="#67E8F9" style={{filter: 'drop-shadow(0 0 10px #00B5C7)'}} /> : null;
        })}
        {[[0, 1], [0, 2], [0, 3]].map(([a, b], i) => (
          <path key={i} d={path({type: 'LineString', coordinates: [CITIES[a], CITIES[b]]}) ?? ''} fill="none" stroke="#EC4899" strokeWidth={3} />
        ))}
      </svg>
    </AbsoluteFill>
  );
};
```
Dépendance : `npm i d3-geo` (+ `topojson-client world-atlas` pour les continents).

## Variations
- Continents pleins : `feature(world, world.objects.countries)` dessinés avec `path`.
- Globe en points façon GitHub : échantillonner la terre en points réguliers.
- Zoom final sur un pays qui s'allume.

## Pièges
- Sans `clipAngle(90)`, la face cachée se dessine par-dessus.
- Arcs « au-dessus » du globe : les lignes géodésiques de d3 restent collées à la surface ; pour des arcs aériens, calculer une Bézier en projection.

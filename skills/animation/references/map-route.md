# Map Route Tracing · `/animation map-route`

> Famille : Tableaux de bord & UI · #74

## C'est quoi
Un parcours qui se trace sur une carte : la ligne avance, un point (coureur, avion, voiture) la suit, la caméra peut suivre ou dézoomer.

## Quand l'utiliser
- Course à pied, vélo, voyage, livraison, itinéraire d'un road trip.

## Technique clé
- Données : trace GPX → liste lat/lon → projection simple (Mercator : `x = lon`, `y = ln(tan(π/4 + lat/2))`) normalisée dans le cadre, ou `d3-geo geoMercator().fitSize()`.
- Ligne : `polyline` avec `pathLength={1}` + `strokeDashoffset = 1 - p`. Point mobile : `path.getPointAtLength` n'existe pas au rendu → interpoler manuellement sur la liste de points (index `p * (n-1)`).
- Fond de carte : image statique (export Mapbox Static / OSM) en `<Img>`, ou carte stylisée sombre avec rues en SVG.
- Halo derrière la ligne (stroke large flou) + couleur vive, départ/arrivée marqués.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

// Trace d'exemple déjà projetée (x,y en px). Remplacer par ton GPX projeté.
const ROUTE: [number, number][] = [[200, 1500], [260, 1300], [380, 1220], [420, 1050], [560, 980], [640, 820], [600, 640], [720, 520], [860, 480], [880, 360]];

const pointAt = (p: number): [number, number] => {
  const f = p * (ROUTE.length - 1), i = Math.min(ROUTE.length - 2, Math.floor(f)), k = f - i;
  return [ROUTE[i][0] + (ROUTE[i + 1][0] - ROUTE[i][0]) * k, ROUTE[i][1] + (ROUTE[i + 1][1] - ROUTE[i][1]) * k];
};

export const MapRoute: React.FC<{text?: string}> = ({text = '20 km'}) => {
  const frame = useCurrentFrame();
  const p = interpolate(frame, [0, 120], [0, 1], {extrapolateRight: 'clamp', easing: Easing.inOut(Easing.sin)});
  const [x, y] = pointAt(p);
  const pts = ROUTE.map((q) => q.join(',')).join(' ');
  const streets = 'repeating-linear-gradient(30deg, #1c2333 0 2px, transparent 2px 90px), repeating-linear-gradient(-60deg, #1c2333 0 2px, transparent 2px 120px)';
  return (
    <AbsoluteFill style={{background: `${streets}, #0d1117`}}>
      <svg width={1080} height={1920}>
        <polyline points={pts} fill="none" stroke="#2a3346" strokeWidth={10} strokeLinejoin="round" strokeLinecap="round" />
        <polyline points={pts} fill="none" stroke="#FF4F8B" strokeWidth={26} strokeOpacity={0.25} strokeLinejoin="round" pathLength={1} strokeDasharray={1} strokeDashoffset={1 - p} />
        <polyline points={pts} fill="none" stroke="#FF4F8B" strokeWidth={10} strokeLinejoin="round" strokeLinecap="round" pathLength={1} strokeDasharray={1} strokeDashoffset={1 - p} />
        <circle cx={ROUTE[0][0]} cy={ROUTE[0][1]} r={16} fill="#22C55E" />
        <circle cx={x} cy={y} r={22} fill="#fff" stroke="#FF4F8B" strokeWidth={8} />
      </svg>
      <div style={{position: 'absolute', top: 300, left: 140, fontFamily: 'Inter', fontWeight: 900, fontSize: 120, color: '#fff'}}>
        {(p * parseFloat(text)).toFixed(1)} <span style={{fontSize: 60, color: '#FF4F8B'}}>km</span>
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Caméra qui suit le point (`translate(540 - x, 960 - y)` sur le groupe) puis dézoom final.
- Profil d'altitude qui se dessine en bas en même temps.
- Photo de chaque étape qui pop au kilomètre correspondant.

## Pièges
- `pathLength` répartit la progression par longueur, mais `pointAt` par index : avec des points espacés irrégulièrement, le marqueur et la ligne se décalent. Rééchantillonner la trace à pas constant.
- Cartes Mapbox/Google : respecter leurs conditions et attributions.

# Progress Rings · `/animation progress-rings`

> Famille : Tableaux de bord & UI · #73

## C'est quoi
Des anneaux concentriques qui se remplissent comme sur l'Apple Watch (Bouger, M'entraîner, Me lever), avec pourcentage au centre.

## Quand l'utiliser
- Objectifs, progression, stats sportives, KPIs.

## Technique clé
- Cercle SVG : `strokeDasharray = C` (circonférence `2πr`), `strokeDashoffset = C * (1 - p)`, `strokeLinecap: round`, rotation −90° pour partir du haut.
- Piste de fond : même cercle en couleur à 20 % d'opacité.
- Remplissage : `spring` ou `Easing.out(Easing.cubic)` sur 40–60 frames, anneaux décalés de 8 frames.
- Dépassement > 100 % : deuxième tour avec une ombre portée sur l'extrémité.
- Couleurs Apple : `#FA114F`, `#92E82A`, `#1EEAEF` sur fond noir.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

const RINGS = [{r: 300, c: '#FA114F', v: 0.86}, {r: 225, c: '#92E82A', v: 0.64}, {r: 150, c: '#1EEAEF', v: 1}];

export const ProgressRings: React.FC<{text?: string}> = ({text = 'Objectif'}) => {
  const frame = useCurrentFrame();
  return (
    <AbsoluteFill style={{background: '#000', justifyContent: 'center', alignItems: 'center'}}>
      <svg width={800} height={800} viewBox="0 0 800 800" style={{transform: 'rotate(-90deg)'}}>
        {RINGS.map((ring, i) => {
          const C = 2 * Math.PI * ring.r;
          const p = interpolate(frame, [i * 8, i * 8 + 50], [0, ring.v], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp', easing: Easing.out(Easing.cubic)});
          return (
            <g key={i}>
              <circle cx={400} cy={400} r={ring.r} fill="none" stroke={ring.c} strokeOpacity={0.2} strokeWidth={60} />
              <circle cx={400} cy={400} r={ring.r} fill="none" stroke={ring.c} strokeWidth={60} strokeLinecap="round"
                strokeDasharray={C} strokeDashoffset={C * (1 - p)} />
            </g>
          );
        })}
      </svg>
      <div style={{position: 'absolute', fontFamily: 'Inter, sans-serif', color: '#fff', textAlign: 'center'}}>
        <div style={{fontSize: 36, opacity: 0.7}}>{text}</div>
        <div style={{fontSize: 90, fontWeight: 800}}>{Math.round(interpolate(frame, [0, 58], [0, 86], {extrapolateRight: 'clamp'}))} %</div>
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Un seul anneau dégradé (`linearGradient` sur le stroke) pour un score.
- Anneau segmenté (pointillés `strokeDasharray="20 8"`) façon batterie.
- Feux d'artifice à 100 % (voir `fireworks`).

## Pièges
- `strokeLinecap: round` à 0 % affiche quand même un point : masquer quand `p < 0.01`.
- Rotation sur le `svg` entier fait tourner aussi un texte SVG : mettre le texte en HTML par-dessus.

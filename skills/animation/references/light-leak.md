# Light Leak & Lens Flare · `/animation light-leak`

> Famille : Transitions & caméra · #88

## C'est quoi
Des fuites de lumière chaude façon pellicule argentique (taches orange/rouge qui traversent l'image) et des reflets d'objectif (flares) qui suivent une source lumineuse.

## Quand l'utiliser
- Ambiance nostalgique, été, mariage, vlog, transition douce entre deux plans.

## Technique clé
- **Light leak** : 2–3 grands dégradés radiaux (`#FF6A00`, `#FF2D55`, `#FFD27A`) très flous, `mixBlendMode: 'screen'`, qui traversent l'écran en diagonale en 30–60 frames avec une opacité en cloche (`sin(π p)`).
- Utilisable comme transition : pic d'opacité au moment du cut → masque le raccord.
- **Lens flare** : source lumineuse (point blanc + halo) + série de cercles/hexagones semi-transparents alignés sur la droite source → centre écran, positions `centre + (centre - source) * k` pour k ∈ [-0.6, 1.4].
- Grain + légère dominante chaude pour l'effet film.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, useCurrentFrame} from 'remotion';

export const LightLeak: React.FC<{text?: string}> = ({text = 'souvenirs'}) => {
  const frame = useCurrentFrame();
  const p = interpolate(frame, [0, 60], [0, 1], {extrapolateRight: 'clamp'});
  const o = Math.sin(Math.PI * p);
  const sx = interpolate(frame, [0, 90], [200, 900]), sy = 380;
  const flares = [-0.5, 0.3, 0.7, 1.1, 1.4].map((k, i) => ({x: 540 + (540 - sx) * k, y: 960 + (960 - sy) * k, r: [60, 30, 90, 45, 140][i]}));
  return (
    <AbsoluteFill style={{background: 'linear-gradient(#3b2a4a, #c97b63)', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{fontFamily: 'Georgia, serif', fontStyle: 'italic', fontSize: 120, color: '#fff3e6'}}>{text}</div>
      <AbsoluteFill style={{mixBlendMode: 'screen', opacity: o}}>
        <div style={{position: 'absolute', width: 1400, height: 1400, left: -700 + p * 900, top: -500 + p * 1500, borderRadius: '50%',
          background: 'radial-gradient(circle, #FF6A00, rgba(255,45,85,.6) 40%, transparent 70%)', filter: 'blur(60px)'}} />
        <div style={{position: 'absolute', width: 900, height: 900, right: -300 + p * 300, top: 200, borderRadius: '50%',
          background: 'radial-gradient(circle, #FFD27A, transparent 65%)', filter: 'blur(50px)'}} />
      </AbsoluteFill>
      <svg width={1080} height={1920} style={{position: 'absolute', inset: 0, mixBlendMode: 'screen'}}>
        <circle cx={sx} cy={sy} r={70} fill="#fff" style={{filter: 'blur(8px)'}} />
        <circle cx={sx} cy={sy} r={260} fill="url(#halo)" />
        <defs><radialGradient id="halo"><stop offset="0" stopColor="#fff" stopOpacity={0.6} /><stop offset="1" stopColor="#FFB070" stopOpacity={0} /></radialGradient></defs>
        {flares.map((f, i) => <circle key={i} cx={f.x} cy={f.y} r={f.r} fill={i % 2 ? '#7CF2FF' : '#FFB070'} opacity={0.18} />)}
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Overlay de vraies vidéos de light leaks en `screen` (packs gratuits).
- Flare anamorphique : longue bande horizontale bleue fine.

## Pièges
- `normal` au lieu de `screen` = taches opaques sales.
- Flares trop nombreux/opaques = cheap. Subtilité : opacité ≤ 0.2.

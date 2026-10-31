# Bioluminescence · `/animation bioluminescence`

> Famille : Mouvements organiques · #22

## C'est quoi
Une lumière douce qui pulse comme des méduses ou du plancton dans les abysses : points et filaments bleu-vert qui respirent dans le noir.

## Quand l'utiliser
- Ambiance zen, nature, mystère, méditation, fond de musique lo-fi.

## Technique clé
- Fond `#000814` avec dégradé radial vers `#001d3d`.
- Points lumineux : opacité `0.3 + 0.7 * (0.5 + 0.5 * sin(t * f + φ))^3` (le cube rend la pulsation « respirante »).
- Couleurs : `#2EF2C4`, `#00B4D8`, `#90E0EF`, halo `drop-shadow(0 0 12px)`.
- Méduse : cloche en demi-ellipse qui se contracte (`scaleX(1 - 0.12*sin)`, `scaleY(1 + 0.08*sin)`) + tentacules en paths qui ondulent avec retard.
- Dérive lente vers le haut (10–20 px/s).

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

export const Bioluminescence: React.FC<{text?: string; count?: number}> = ({count = 140}) => {
  const t = useCurrentFrame() / 30;
  const pulse = Math.sin(t * 2);
  return (
    <AbsoluteFill style={{background: 'radial-gradient(circle at 50% 60%, #001d3d, #000814 70%)'}}>
      <svg width={1080} height={1920}>
        {Array.from({length: count}, (_, i) => {
          const p = Math.pow(0.5 + 0.5 * Math.sin(t * (0.6 + random(`f${i}`)) + random(`p${i}`) * 6.28), 3);
          const y = ((random(`y${i}`) * 1920 - t * (8 + 20 * random(`v${i}`))) % 1920 + 1920) % 1920;
          return <circle key={i} cx={random(`x${i}`) * 1080 + Math.sin(t + i) * 10} cy={y} r={1.5 + random(`r${i}`) * 3}
            fill={i % 3 ? '#2EF2C4' : '#90E0EF'} opacity={0.15 + 0.85 * p} style={{filter: 'drop-shadow(0 0 8px #2EF2C4)'}} />;
        })}
        <g transform={`translate(540 ${900 - t * 15}) scale(${1 - 0.12 * pulse} ${1 + 0.08 * pulse})`} style={{filter: 'drop-shadow(0 0 24px #00B4D8)'}}>
          <path d="M-140 0 A140 120 0 0 1 140 0 Q0 30 -140 0 Z" fill="rgba(0,180,216,.35)" stroke="#90E0EF" strokeWidth={3} />
          {[-90, -40, 10, 60, 100].map((x, k) => (
            <path key={k} d={`M${x} 10 q ${20 * Math.sin(t * 3 + k)} 80 0 160 t 0 160`} stroke="#2EF2C4" strokeWidth={3} fill="none" opacity={0.7} />
          ))}
        </g>
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Vague qui s'illumine au passage (plage la nuit) : opacité liée à la distance à une ligne qui avance.
- Champignons/forêt lumineuse : mêmes pulsations sur des silhouettes.

## Pièges
- Trop de `drop-shadow` = rendu lent : le mettre sur un groupe plutôt que sur chaque point.
- Couleurs trop saturées = effet néon, pas bio. Garder des bleus-verts doux.

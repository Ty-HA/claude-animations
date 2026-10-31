# Firework / Burst · `/animation fireworks`

> Famille : Personnages & fun · #100

## C'est quoi
Un feu d'artifice : une fusée monte, explose en gerbe de particules lumineuses qui retombent en scintillant et laissent des traînées.

## Quand l'utiliser
- Célébration, nouvel an, lancement, palier atteint, fin de série (« partie 5/5 ! »).

## Technique clé
- **Fusée** : point qui monte de `y0` à `yExplosion` en 20–30 frames, `Easing.out(Easing.quad)`, petite traînée.
- **Explosion** : N = 60–100 particules, angle `2π i/N + jitter`, vitesse `v0 * (0.6 + 0.4 * random)`. Position fermée : `x = cx + cos(a) * v0 * τ * drag(τ)`, `y = cy + sin(a) * v0 * τ * drag(τ) + 0.5 * g * τ²` avec `drag(τ) = (1 - e^{-kτ}) / (kτ)`.
- **Traînées** : dessiner chaque particule aussi à τ-2, τ-4, τ-6 avec opacité décroissante (ou `line` de τ-4 à τ).
- Fin de vie : opacité `1 - τ/life` + scintillement (`random(frame) > 0.3`) dans le dernier tiers.
- `globalCompositeOperation: 'lighter'` (canvas) ou `mixBlendMode: 'screen'` pour les superpositions lumineuses.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, random, useCurrentFrame} from 'remotion';

const SHOTS = [{x: 380, y: 620, at: 0, hue: 330}, {x: 720, y: 520, at: 18, hue: 185}, {x: 540, y: 820, at: 34, hue: 50}];
const N = 80, LIFE = 55, RISE = 22;

export const Fireworks: React.FC<{text?: string}> = ({text = 'BRAVO !'}) => {
  const frame = useCurrentFrame();
  return (
    <AbsoluteFill style={{background: 'linear-gradient(#02010a, #0b1033)'}}>
      <svg width={1080} height={1920} style={{mixBlendMode: 'screen'}}>
        {SHOTS.map((s, k) => {
          const f = frame - s.at;
          if (f < 0) return null;
          if (f < RISE) {
            const y = interpolate(f, [0, RISE], [1920, s.y], {easing: Easing.out(Easing.quad)});
            return <circle key={k} cx={s.x} cy={y} r={6} fill={`hsl(${s.hue} 100% 85%)`} />;
          }
          const tau = f - RISE;
          if (tau > LIFE) return null;
          return Array.from({length: N}, (_, i) => {
            const a = (i / N) * Math.PI * 2 + random(`a${k}${i}`) * 0.1;
            const v = 14 * (0.6 + 0.4 * random(`v${k}${i}`));
            const pos = (t: number) => {
              const d = t <= 0 ? 0 : (1 - Math.exp(-0.08 * t)) / 0.08;
              return [s.x + Math.cos(a) * v * d, s.y + Math.sin(a) * v * d + 0.5 * 0.12 * t * t];
            };
            const [x, y] = pos(tau), [x2, y2] = pos(tau - 5);
            const life = 1 - tau / LIFE;
            const twinkle = tau > LIFE * 0.6 ? (random(`t${k}${i}${frame}`) > 0.35 ? 1 : 0.2) : 1;
            return (
              <g key={`${k}-${i}`} opacity={life * twinkle}>
                <line x1={x2} y1={y2} x2={x} y2={y} stroke={`hsl(${s.hue} 100% 60%)`} strokeWidth={3} strokeLinecap="round" />
                <circle cx={x} cy={y} r={3.5} fill={`hsl(${s.hue} 100% 88%)`} />
              </g>
            );
          });
        })}
      </svg>
      <div style={{position: 'absolute', top: 1250, width: '100%', textAlign: 'center', fontFamily: 'Inter', fontWeight: 900, fontSize: 130, color: '#fff',
        textShadow: '0 0 30px #EC4899', opacity: interpolate(frame, [30, 40], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'})}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Gerbes en forme de cœur/étoile (angles et vitesses dérivés d'une forme paramétrique).
- Pluie dorée (saule) : `g` plus fort, durée plus longue, couleur or.
- Flash du ciel à l'explosion (`background` qui s'éclaire 2 frames).

## Pièges
- Particules en mouvement linéaire sans frottement = ça ressemble à une étoile qui grossit, pas à un feu d'artifice. Le `drag` est essentiel.
- 3 × 80 particules × 2 éléments SVG = OK ; au-delà, canvas.

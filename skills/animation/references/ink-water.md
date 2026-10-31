# Fluid / Ink in Water · `/animation ink-water`

> Famille : Mouvements organiques · #24

## C'est quoi
Une goutte d'encre qui tombe dans l'eau et se diffuse en volutes, nuages et filaments.

## Quand l'utiliser
- Intro artistique, transition, révélation de logo, univers beauté / parfum.

## Technique clé
- **Option rapide (SVG)** : cercles qui grossissent + `feTurbulence` (baseFrequency 0.008–0.015, numOctaves 3) + `feDisplacementMap` (scale 80 → 160 animé). Le `seed` de la turbulence fixe la forme.
- Animer `baseFrequency` légèrement et le rayon : `r = 40 + 500 * Easing.out(Easing.cubic)(p)`.
- Couleur : encre bleu nuit `#1B1464` ou rouge sur fond blanc cassé `#F4F1EA`, `mix-blend-mode: multiply`.
- **Option réaliste** : simulation de fluide WebGL (Navier-Stokes) — lourd ; ou utiliser une vraie vidéo d'encre en `mix-blend-mode: multiply`.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

export const InkWater: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const p = interpolate(frame, [0, 90], [0, 1], {extrapolateRight: 'clamp', easing: Easing.out(Easing.cubic)});
  return (
    <AbsoluteFill style={{background: '#F4F1EA'}}>
      <svg width={1080} height={1920}>
        <defs>
          <filter id="ink" x="-50%" y="-50%" width="200%" height="200%">
            <feTurbulence type="fractalNoise" baseFrequency={0.009 + 0.004 * p} numOctaves={3} seed={7} result="n" />
            <feDisplacementMap in="SourceGraphic" in2="n" scale={60 + 160 * p} xChannelSelector="R" yChannelSelector="G" />
            <feGaussianBlur stdDeviation={2 + 6 * p} />
          </filter>
        </defs>
        <g filter="url(#ink)" style={{mixBlendMode: 'multiply'}}>
          <circle cx={540} cy={700 + 300 * p} r={40 + 380 * p} fill="#1B1464" opacity={0.85 - 0.35 * p} />
          <circle cx={480} cy={820 + 420 * p} r={20 + 260 * p} fill="#3A2C9C" opacity={0.6} />
          <circle cx={620} cy={760 + 360 * p} r={10 + 200 * p} fill="#0E0B3A" opacity={0.7} />
        </g>
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Plusieurs couleurs qui se mélangent (bleu + rose, `multiply`).
- Révéler un texte : le texte en `mask` sous l'encre qui s'étend.
- Goutte qui tombe d'abord (petit cercle qui descend vite), puis diffusion.

## Pièges
- `feDisplacementMap` est lourd en 1080×1920 : limiter la zone du filtre (`x/y/width/height`).
- Le seed fixe la forme : changer `seed` pour une autre variante, jamais aléatoire par frame.

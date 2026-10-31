# Watercolor · `/animation watercolor`

> Famille : Esthétiques artistiques · #61

## C'est quoi
Des taches d'aquarelle qui se diffusent sur du papier : bords irréguliers plus foncés, transparences, couleurs qui se mélangent.

## Quand l'utiliser
- Contenus doux : bien-être, mariage, voyage, poésie, illustration délicate.

## Technique clé
- Papier : fond `#FBF8F1` + grain `feTurbulence` 0.8 en `multiply` 10 %.
- Tache : cercle → `feTurbulence` (0.012, 4 octaves) + `feDisplacementMap` (scale 50) pour un bord organique.
- **Bord plus foncé** (effet séchage) : `stroke` de la même couleur à 40 % d'opacité, 6 px, sous le même filtre.
- Plusieurs couches de la même tache à 25–35 % d'opacité, rayons légèrement différents, `mix-blend-mode: multiply`.
- Diffusion : rayon `0 → R` avec `Easing.out(Easing.quad)` sur 40–60 frames.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

const SPOTS = [
  {x: 420, y: 760, r: 280, c: '#E07A9B', d: 0},
  {x: 640, y: 980, r: 300, c: '#6FA8DC', d: 15},
  {x: 480, y: 1180, r: 240, c: '#F2C14E', d: 30},
];

export const Watercolor: React.FC<{text?: string}> = ({text = 'doucement'}) => {
  const frame = useCurrentFrame();
  return (
    <AbsoluteFill style={{background: '#FBF8F1'}}>
      <svg width={1080} height={1920}>
        <defs>
          <filter id="wc" x="-30%" y="-30%" width="160%" height="160%">
            <feTurbulence type="fractalNoise" baseFrequency={0.012} numOctaves={4} seed={9} result="n" />
            <feDisplacementMap in="SourceGraphic" in2="n" scale={55} xChannelSelector="R" yChannelSelector="G" />
          </filter>
        </defs>
        {SPOTS.map((s, i) => {
          const p = interpolate(frame, [s.d, s.d + 50], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp', easing: Easing.out(Easing.quad)});
          return (
            <g key={i} filter="url(#wc)" style={{mixBlendMode: 'multiply'}}>
              {[1, 0.88, 0.75].map((k, j) => (
                <circle key={j} cx={s.x} cy={s.y} r={s.r * p * k} fill={s.c} fillOpacity={0.28} stroke={s.c} strokeOpacity={0.45} strokeWidth={6} />
              ))}
            </g>
          );
        })}
      </svg>
      <div style={{position: 'absolute', top: 1500, width: '100%', textAlign: 'center', fontFamily: '"Caveat", cursive', fontSize: 110, color: '#5b4a3f'}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Lavis de ciel : grandes bandes horizontales diluées.
- Fleur aquarelle : pétales (ellipses tournées) qui se diffusent en séquence.
- Texte aquarelle : texte comme masque d'une tache.

## Pièges
- Une seule couche opaque = ça fait « blob numérique ». Superposer des couches transparentes.
- Ne pas changer le `seed` dans le temps (sinon le bord grouille).

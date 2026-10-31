# Neon Glow / Cyberpunk · `/animation neon`

> Famille : Mouvements organiques · #18

## C'est quoi
Des tubes de néon qui s'allument en grésillant, avec halo lumineux, reflets et traînées de lumière.

## Quand l'utiliser
- Enseigne, titre de nuit, univers cyberpunk / gaming / club.

## Technique clé
- Halo : `textShadow` en couches : `0 0 4px #fff, 0 0 12px C, 0 0 28px C, 0 0 60px C` (C = `#FF2EC4` rose, `#00F0FF` cyan).
- Texte : blanc légèrement teinté au centre (tube), police fine arrondie (`Monoton`, `Neonderthaw`, `Tilt Neon`).
- **Allumage** : clignotements irréguliers les 20 premières frames : tableau fixe `[1,0,1,1,0,0,1,0,1,1,1]` indexé par `frame/2`.
- Flicker permanent léger : opacité `0.92 + 0.08 * random(frame)`.
- Mur de briques sombre ou fond `#07030f` + reflet au sol (`transform: scaleY(-1)`, opacité 0.25, `blur(6px)`).

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

const BOOT = [0, 1, 0, 0, 1, 1, 0, 1, 0, 1, 1, 1];

export const Neon: React.FC<{text?: string; color?: string}> = ({text = 'OPEN 24/7', color = '#FF2EC4'}) => {
  const frame = useCurrentFrame();
  const on = frame < BOOT.length * 2 ? BOOT[Math.floor(frame / 2)] : 0.92 + 0.08 * random(`n${frame}`);
  const glow = `0 0 4px #fff, 0 0 12px ${color}, 0 0 28px ${color}, 0 0 60px ${color}`;
  const label = (flip = false): React.CSSProperties => ({
    fontFamily: '"Tilt Neon", "Arial Rounded MT Bold", sans-serif', fontSize: 150, color: '#fff5fd',
    textShadow: on ? glow : 'none', opacity: on ? on : 0.25,
    transform: flip ? 'scaleY(-1)' : undefined, filter: flip ? 'blur(6px)' : undefined,
  });
  return (
    <AbsoluteFill style={{background: '#07030f', justifyContent: 'center', alignItems: 'center'}}>
      <div style={label()}>{text}</div>
      <div style={{...label(true), opacity: (on ? on : 0.25) * 0.25}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Tube dessiné : path SVG avec `strokeDashoffset` (le néon se trace) + `filter` gaussien.
- Traînées lumineuses (light trails) : lignes qui filent avec un dégradé transparent → couleur.
- Deux couleurs qui alternent (rose/cyan) sur le rythme.

## Pièges
- `textShadow` avec 4 couches sur beaucoup de texte = rendu lent : limiter aux titres.
- Halo écrasé par la compression vidéo : renforcer légèrement (le flou sombre bien en H.264).

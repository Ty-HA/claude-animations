# Particles System · `/animation particles`

> Famille : Mouvements organiques · #17

## C'est quoi
Des milliers de points soumis à des forces (vent, gravité, attraction) qui forment des nuées, des explosions ou des constellations.

## Quand l'utiliser
- Fond tech/IA, poussière magique, neige, formation d'un logo à partir de points.

## Technique clé
- **Déterministe** : position = fonction de la frame, pas d'état accumulé. `x = x0 + vx*t + amp*sin(t*f + phase)`, tout tiré avec `random('seed'+i)`.
- Gravité : `y = y0 + vy*t + 0.5*g*t²`. Wrap écran : `((y % H) + H) % H`.
- Rendu : jusqu'à ~500 points en SVG `<circle>`, au-delà utiliser `<canvas>` dessiné dans un `useEffect` par frame (ou `@remotion/three`).
- Profondeur : taille et opacité proportionnelles à `z = random(...)` (petits = loin, flous).
- Liens (plexus) : relier les points à moins de 150 px, opacité `1 - d/150`.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

export const Particles: React.FC<{text?: string; count?: number}> = ({count = 350}) => {
  const frame = useCurrentFrame();
  const t = frame / 30;
  return (
    <AbsoluteFill style={{background: 'radial-gradient(circle at 50% 40%, #0a2540, #010012 70%)'}}>
      <svg width={1080} height={1920}>
        {Array.from({length: count}, (_, i) => {
          const z = random(`z${i}`);
          const x = (random(`x${i}`) * 1080 + Math.sin(t * (0.5 + z) + i) * 40 + t * 20 * z + 1080) % 1080;
          const y = ((random(`y${i}`) * 1920 - t * (30 + 90 * z)) % 1920 + 1920) % 1920;
          return <circle key={i} cx={x} cy={y} r={1 + z * 3.5} fill="#67E8F9" opacity={0.2 + z * 0.7} />;
        })}
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- **Formation de logo** : chaque particule a une cible (points échantillonnés sur un texte) et `interpolate` de sa position libre vers sa cible.
- **Attracteur** : force vers le centre `(cx - x) * k`.
- **Explosion** : voir `fireworks`.

## Pièges
- Ne jamais accumuler l'état d'une frame à l'autre (`useState`) : Remotion rend les frames dans le désordre.
- Trop de `<circle>` = rendu lent : passer en canvas au-delà de ~800 points.

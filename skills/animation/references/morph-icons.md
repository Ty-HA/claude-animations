# Morphing Icons · `/animation morph-icons`

> Famille : Personnages & fun · #99

## C'est quoi
Une icône qui se transforme fluidement en une autre : play → pause, menu → croix, cœur → étoile, micro → onde.

## Quand l'utiliser
- Démo d'UI, explication d'étapes (idée → code → vidéo), logos animés, transitions de concepts.

## Technique clé
- Interpoler des paths SVG : `@remotion/paths` (`interpolatePath(p, pathA, pathB)`) ou `flubber` (`interpolate(a, b, {maxSegmentLength: 4})`) qui gère des formes de nombres de points différents.
- Avec des paths compatibles (même nombre de commandes), `interpolatePath` suffit.
- Morph 12–18 frames, `Easing.inOut(Easing.cubic)` ; ajouter une rotation de 90° ou 180° pendant le morph pour du dynamisme.
- Couleur qui change en même temps (`interpolateColors`).

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, interpolateColors, useCurrentFrame} from 'remotion';
import {interpolatePath} from '@remotion/paths';

// Formes compatibles : 2 sous-chemins de 4 points chacun
const PLAY = 'M 30 20 L 30 80 L 55 65 L 55 35 Z M 55 35 L 55 65 L 80 50 L 80 50 Z';
const PAUSE = 'M 28 20 L 28 80 L 44 80 L 44 20 Z M 56 20 L 56 80 L 72 80 L 72 20 Z';

export const MorphIcons: React.FC<{text?: string; hold?: number}> = ({hold = 40}) => {
  const frame = useCurrentFrame();
  const k = Math.floor(frame / hold) % 2;
  const local = frame % hold;
  const p = interpolate(local, [hold - 15, hold], [0, 1], {extrapolateLeft: 'clamp', easing: Easing.inOut(Easing.cubic)});
  const [a, b] = k === 0 ? [PLAY, PAUSE] : [PAUSE, PLAY];
  const color = interpolateColors(p, [0, 1], k === 0 ? ['#00B5C7', '#EC4899'] : ['#EC4899', '#00B5C7']);
  return (
    <AbsoluteFill style={{background: '#010012', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{width: 420, height: 420, borderRadius: '50%', border: `8px solid ${color}`, display: 'flex', justifyContent: 'center', alignItems: 'center',
        boxShadow: `0 0 50px ${color}66`}}>
        <svg width={300} height={300} viewBox="0 0 100 100" style={{transform: `rotate(${p * 90}deg)`}}>
          <path d={interpolatePath(p, a, b)} fill={color} />
        </svg>
      </div>
    </AbsoluteFill>
  );
};
```
Dépendance : `npm i @remotion/paths`.

## Variations
- Chaîne d'icônes (idée 💡 → code → vidéo) via `flubber` pour des formes quelconques.
- Morph de texte (lettre → lettre) avec des polices converties en paths.
- Hamburger → croix (voir `micro-interactions`).

## Pièges
- Paths incompatibles avec `interpolatePath` = formes qui se tordent : rééchantillonner ou utiliser `flubber`.
- Sens de tracé opposé (horaire/anti-horaire) = la forme se retourne pendant le morph.

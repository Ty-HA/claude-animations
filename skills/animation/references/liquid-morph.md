# Liquid Morphing · `/animation liquid-morph`

> Famille : Mouvements organiques · #16

## C'est quoi
Des formes qui se transforment les unes dans les autres de façon fluide et élastique : cercle → étoile → goutte → blob.

## Quand l'utiliser
- Fonds de marque doux, transitions entre icônes, logos animés.

## Technique clé
- Interpoler des **paths** qui ont le même nombre de points : générer chaque forme en polaire avec N = 64 points, puis interpoler point à point.
- Bibliothèque alternative : `flubber` (`interpolate(pathA, pathB)`) ou `@remotion/paths` (`interpolatePath`).
- Easing élastique : `Easing.inOut(Easing.cubic)` pour le morph, plus un léger « wobble » `sin(angle*3 + frame*0.1)*8`.
- Dégradé violet → rose ou cyan, `filter: blur(0.5px)` + glow.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

const N = 64;
const shapes = [
  (a: number) => 260,                                   // cercle
  (a: number) => 200 + 90 * Math.cos(5 * a),             // étoile douce
  (a: number) => 230 + 70 * Math.sin(2 * a) * Math.cos(a), // goutte
];

export const LiquidMorph: React.FC<{text?: string; hold?: number}> = ({hold = 45}) => {
  const frame = useCurrentFrame();
  const k = Math.floor(frame / hold) % shapes.length;
  const p = interpolate(frame % hold, [15, hold], [0, 1], {extrapolateLeft: 'clamp', easing: Easing.inOut(Easing.cubic)});
  const A = shapes[k], B = shapes[(k + 1) % shapes.length];
  const pts = Array.from({length: N}, (_, i) => {
    const a = (i / N) * Math.PI * 2;
    const r = A(a) * (1 - p) + B(a) * p + 8 * Math.sin(a * 3 + frame * 0.1);
    return `${540 + r * Math.cos(a)},${540 + r * Math.sin(a)}`;
  });
  return (
    <AbsoluteFill style={{background: '#0b0618', justifyContent: 'center', alignItems: 'center'}}>
      <svg width={1080} height={1080}>
        <defs><linearGradient id="lg" x1="0" x2="1"><stop offset="0" stopColor="#A855F7" /><stop offset="1" stopColor="#EC4899" /></linearGradient></defs>
        <polygon points={pts.join(' ')} fill="url(#lg)" style={{filter: 'drop-shadow(0 0 30px rgba(236,72,153,.5))'}} />
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Lisser avec une courbe Catmull-Rom au lieu de `polygon` pour des bords parfaits.
- Morph de logos/icônes : `@remotion/paths` + `flubber` pour des paths quelconques.
- Plusieurs couches décalées en opacité pour un effet gélatine.

## Pièges
- Morph entre paths de nombres de points différents = déchirures. Toujours rééchantillonner.
- Morph trop rapide (<10 frames) : on ne voit pas la fluidité.

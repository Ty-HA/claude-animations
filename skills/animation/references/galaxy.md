# Galaxy Particles 3D · `/animation galaxy`

> Famille : Styles 3D · #42

## C'est quoi
Une galaxie spirale de milliers d'étoiles vue en profondeur, qui tourne lentement pendant que la caméra plonge dedans.

## Quand l'utiliser
- Intro épique, « l'univers des possibles », musique, espace, IA.

## Technique clé
- Génération : pour chaque étoile, bras `b = i % 3`, rayon `r = random^1.5 * R`, angle `θ = b * 2π/3 + r * 0.9 (enroulement) + bruit`, hauteur `y = gauss * 0.05 * R`.
- Couleur : centre chaud `#FFD9A0` → bords bleu/violet `#6A5CFF` selon `r/R`.
- Rotation : `θ + t * (0.3 / (1 + r))` (le centre tourne plus vite).
- Projection perspective avec caméra inclinée (rotation X de 60°) puis `x' = x * f/(z+d)`.
- 3 000 étoiles : canvas 2D ou `@remotion/three` `<points>` + `PointsMaterial({size, vertexColors, blending: AdditiveBlending})`.

## Snippet Remotion (canvas)
```tsx
import {useEffect, useRef} from 'react';
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

const N = 3000, R = 1;
const STARS = Array.from({length: N}, (_, i) => {
  const r = Math.pow(random(`r${i}`), 1.5) * R;
  return {r, a: (i % 3) * (2 * Math.PI / 3) + r * 5 + (random(`a${i}`) - 0.5) * 0.6, y: (random(`y${i}`) - 0.5) * 0.08, s: random(`s${i}`)};
});

export const Galaxy: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const ref = useRef<HTMLCanvasElement>(null);
  useEffect(() => {
    const ctx = ref.current!.getContext('2d')!;
    ctx.fillStyle = '#02010a'; ctx.fillRect(0, 0, 1080, 1920);
    ctx.globalCompositeOperation = 'lighter';
    const t = frame / 30, tilt = 1.05, d = 2.2 - t * 0.05;
    for (const s of STARS) {
      const a = s.a + t * (0.3 / (1 + s.r * 3));
      const x = Math.cos(a) * s.r, z0 = Math.sin(a) * s.r;
      const y = s.y * Math.cos(tilt) - z0 * Math.sin(tilt), z = s.y * Math.sin(tilt) + z0 * Math.cos(tilt);
      const k = 900 / (z + d);
      const hue = 35 + 220 * s.r;
      ctx.fillStyle = `hsla(${hue},90%,${75 - 25 * s.r}%,${0.5 + 0.5 * s.s})`;
      ctx.fillRect(540 + x * k, 960 + y * k, 1 + s.s * 2, 1 + s.s * 2);
    }
    ctx.globalCompositeOperation = 'source-over';
  }, [frame]);
  return <AbsoluteFill><canvas ref={ref} width={1080} height={1920} /></AbsoluteFill>;
};
```

## Variations
- Noyau lumineux : `radial-gradient` jaune flou au centre.
- Plongée finale : `d` qui diminue vite → traversée des étoiles (effet hyperespace).
- Version `@remotion/three` pour 50 000 particules.

## Pièges
- `d` trop petit → division par ~0 et étoiles géantes : clamp `z + d > 0.1`.
- Le canvas doit être entièrement redessiné à chaque frame (pas d'accumulation).

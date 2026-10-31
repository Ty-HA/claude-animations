# Reaction-Diffusion · `/animation reaction-diffusion`

> Famille : Mouvements organiques · #27

## C'est quoi
Des motifs vivants qui poussent tout seuls : taches de léopard, rayures de zèbre, labyrinthes de corail. Simulation Gray-Scott de deux « produits chimiques ».

## Quand l'utiliser
- Art génératif, pochettes, fonds bio/organique, reveal d'un logo qui « pousse ».

## Technique clé
- Gray-Scott : deux grilles A, B. À chaque pas :
  `A' = A + (Da·∇²A − A·B² + f·(1−A))`, `B' = B + (Db·∇²B + A·B² − (k+f)·B)`, avec `Da = 1.0`, `Db = 0.5`.
- Presets `(f, k)` : corail `(0.0545, 0.062)`, taches `(0.035, 0.065)`, labyrinthe `(0.029, 0.057)`.
- C'est **itératif** : l'image à la frame N dépend des N-1 précédentes. En Remotion, recalculer depuis le début pour chaque frame (OK sur petite grille 200×356) ou précalculer une séquence d'images, ou utiliser un shader ping-pong.
- 10–20 itérations par frame vidéo. Upscale avec `image-rendering: auto` + flou léger pour lisser.

## Snippet Remotion (canvas, grille réduite)
```tsx
import {useEffect, useRef} from 'react';
import {AbsoluteFill, useCurrentFrame} from 'remotion';

const W = 180, H = 320, F = 0.0545, K = 0.062;

const simulate = (steps: number) => {
  let A = new Float32Array(W * H).fill(1), B = new Float32Array(W * H);
  for (let y = H / 2 - 8; y < H / 2 + 8; y++) for (let x = W / 2 - 8; x < W / 2 + 8; x++) B[y * W + x] = 1;
  for (let s = 0; s < steps; s++) {
    const nA = new Float32Array(W * H), nB = new Float32Array(W * H);
    for (let y = 1; y < H - 1; y++) for (let x = 1; x < W - 1; x++) {
      const i = y * W + x;
      const lap = (G: Float32Array) => G[i - 1] + G[i + 1] + G[i - W] + G[i + W] - 4 * G[i];
      const abb = A[i] * B[i] * B[i];
      nA[i] = A[i] + (lap(A) * 1.0 - abb + F * (1 - A[i]));
      nB[i] = B[i] + (lap(B) * 0.5 + abb - (K + F) * B[i]);
    }
    A = nA; B = nB;
  }
  return B;
};

export const ReactionDiffusion: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const ref = useRef<HTMLCanvasElement>(null);
  useEffect(() => {
    const B = simulate(frame * 12);
    const ctx = ref.current!.getContext('2d')!;
    const img = ctx.createImageData(W, H);
    B.forEach((b, i) => { const v = Math.min(255, b * 600); img.data.set([v * 0.2, v * 0.9, v, 255], i * 4); });
    ctx.putImageData(img, 0, 0);
  }, [frame]);
  return (
    <AbsoluteFill style={{background: '#000'}}>
      <canvas ref={ref} width={W} height={H} style={{width: 1080, height: 1920, filter: 'blur(2px) contrast(1.4)'}} />
    </AbsoluteFill>
  );
};
```

## Variations
- Faire pousser depuis la forme d'un texte (B = 1 sur les pixels du texte).
- Shader WebGL ping-pong pour la pleine résolution.

## Pièges
- Recalcul depuis 0 à chaque frame = coût en O(N²) sur la durée : garder des vidéos courtes (≤ 6 s) ou précalculer.
- Pas de temps trop grand → explosion numérique (valeurs NaN). Garder Da/Db ≤ 1.

# Fractal Zoom · `/animation fractal-zoom`

> Famille : Mouvements organiques · #25

## C'est quoi
Un zoom infini dans une fractale (Mandelbrot, Julia) : des motifs qui se répètent à toutes les échelles.

## Quand l'utiliser
- Moment « trip », intro psychédélique, métaphore de l'infini, fond musique électro.

## Technique clé
- Calcul par pixel → **shader** obligatoire en pratique (WebGL via `@remotion/three` ou un `<canvas>` WebGL brut).
- Mandelbrot : `z = z² + c`, `c = centre + (uv - 0.5) * échelle`, 200–500 itérations, couleur = `iter / maxIter` passé dans une palette cosinus (`a + b*cos(2π(c*t + d))`).
- Zoom exponentiel : `échelle = 3 * Math.pow(0.97, frame)` (régulier à l'œil). Centre intéressant : `(-0.743643887, 0.131825904)`.
- Limite float32 : ~zoom 1e5. Au-delà, il faut de la double précision émulée.
- Version légère sans shader : **zoom infini d'images** (même image imbriquée à ×2, scale qui boucle de 1 à 2).

## Snippet Remotion (zoom infini sans shader)
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

// Motif récursif : carrés imbriqués tournés, boucle parfaite quand scale passe de 1 à 2
export const FractalZoom: React.FC<{text?: string; loop?: number}> = ({loop = 60}) => {
  const frame = useCurrentFrame();
  const k = (frame % loop) / loop;
  const scale = Math.pow(2, k);
  return (
    <AbsoluteFill style={{background: '#05010f', justifyContent: 'center', alignItems: 'center', overflow: 'hidden'}}>
      <div style={{transform: `scale(${scale}) rotate(${k * 30}deg)`}}>
        {Array.from({length: 10}, (_, i) => {
          const s = 1600 / Math.pow(2, i / 2);
          return (
            <div key={i} style={{position: 'absolute', width: s, height: s, left: -s / 2, top: -s / 2,
              border: `${Math.max(2, s / 60)}px solid hsl(${(i * 36 + frame * 2) % 360} 90% 60%)`,
              transform: `rotate(${i * 30}deg)`, borderRadius: i % 2 ? '50%' : 0}} />
          );
        })}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Shader Mandelbrot plein écran (le vrai zoom fractal), palette cyan/violet.
- Ensemble de Julia avec `c` qui tourne lentement : la forme se transforme sans zoom.
- Droste effect sur une photo.

## Pièges
- Mandelbrot en JS pur sur 2 millions de pixels par frame = rendu extrêmement lent. Shader ou version légère.
- Zoom linéaire paraît ralentir : toujours exponentiel.

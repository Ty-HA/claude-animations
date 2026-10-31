# Flow Field (Perlin Noise) · `/animation flow-field`

> Famille : Mouvements organiques · #23

## C'est quoi
Des milliers de petits traits qui suivent un champ de courants invisible, comme des cheveux dans le vent ou des courants marins.

## Quand l'utiliser
- Art génératif, fond hypnotique, visuel de musique ou de data « vivante ».

## Technique clé
- Champ d'angle : `angle(x, y, t) = noise(x*0.003, y*0.003, t*0.2) * 2π * 2`. Bruit : `simplex-noise` (`createNoise3D(alea('seed'))`) pour être déterministe.
- Chaque particule trace une **ligne de courant** : partir de (x0, y0), avancer de 6 px dans la direction du champ, 20 pas → un `polyline`.
- Animer : faire glisser le point de départ le long du flux (`offset = frame * vitesse`) ou faire évoluer `t`.
- Couleur selon l'angle (teinte HSL) ou dégradé de marque ; traits de 1,5 px, opacité 0.6.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';
import {createNoise3D} from 'simplex-noise';
import alea from 'alea';

const noise = createNoise3D(alea('flow'));

export const FlowField: React.FC<{text?: string; count?: number}> = ({count = 600}) => {
  const t = useCurrentFrame() / 30;
  return (
    <AbsoluteFill style={{background: '#0a0a12'}}>
      <svg width={1080} height={1920}>
        {Array.from({length: count}, (_, i) => {
          let x = random(`x${i}`) * 1080, y = random(`y${i}`) * 1920;
          const pts = [];
          for (let s = 0; s < 18; s++) {
            const a = noise(x * 0.003, y * 0.003, t * 0.15) * Math.PI * 2;
            x += Math.cos(a) * 7; y += Math.sin(a) * 7;
            pts.push(`${x.toFixed(1)},${y.toFixed(1)}`);
          }
          const hue = 180 + 100 * random(`h${i}`);
          return <polyline key={i} points={pts.join(' ')} fill="none" stroke={`hsl(${hue} 90% 65%)`} strokeWidth={1.5} opacity={0.6} strokeLinecap="round" />;
        })}
      </svg>
    </AbsoluteFill>
  );
};
```
Dépendances : `npm i simplex-noise alea`.

## Variations
- Accumulation façon peinture : dessiner sur canvas en traînées longues (100+ pas) pour une image fixe générative.
- Champ qui épouse un texte : angle tangent aux contours d'un mot.

## Pièges
- `Math.random()` dans le bruit = image différente à chaque frame. Toujours seed (`alea`).
- 600 × 18 points en SVG : OK ; au-delà passer en canvas.

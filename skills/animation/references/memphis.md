# Memphis Style · `/animation memphis`

> Famille : Esthétiques artistiques · #50

## C'est quoi
Le style années 80 du groupe Memphis : zigzags, confettis, squiggles, damiers, triangles, cercles pointillés, couleurs pastel et vives mélangées.

## Quand l'utiliser
- Fun, rétro 80/90, mode, événements, contenus pour ados/jeunes adultes.

## Technique clé
- Fond pastel `#FFF1E6` ou `#B8F2E6`, 15–25 motifs éparpillés : zigzag (polyline), squiggle (path sinusoïdal), triangle, cercle plein, anneau, damier (`conic-gradient` répétée), trames de points.
- Couleurs : `#FF6F91`, `#FFC75F`, `#00C9A7`, `#845EC2`, `#000`.
- Chaque motif : flottement + rotation lente (`rotate(frame * vr)`), entrée en `scale` avec rebond décalé.
- Traits noirs épais (6–8 px), ombres plates décalées.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, spring, useCurrentFrame, useVideoConfig} from 'remotion';

const C = ['#FF6F91', '#FFC75F', '#00C9A7', '#845EC2', '#000'];

const Shape: React.FC<{k: number}> = ({k}) => {
  const type = k % 5, col = C[k % C.length];
  if (type === 0) return <polyline points="0,40 20,0 40,40 60,0 80,40 100,0" fill="none" stroke={col} strokeWidth={8} strokeLinejoin="miter" />;
  if (type === 1) return <path d="M0 20 Q 15 0 30 20 T 60 20 T 90 20 T 120 20" fill="none" stroke={col} strokeWidth={8} strokeLinecap="round" />;
  if (type === 2) return <polygon points="0,70 40,0 80,70" fill={col} stroke="#000" strokeWidth={5} />;
  if (type === 3) return <circle cx={35} cy={35} r={30} fill="none" stroke={col} strokeWidth={8} strokeDasharray="2 12" strokeLinecap="round" />;
  return <circle cx={30} cy={30} r={28} fill={col} />;
};

export const Memphis: React.FC<{text?: string}> = ({text = 'TOTALLY\n80s'}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  return (
    <AbsoluteFill style={{background: '#FFF1E6'}}>
      <svg width={1080} height={1920}>
        {Array.from({length: 22}, (_, i) => {
          const s = spring({frame: frame - i * 2, fps, config: {damping: 10}});
          const x = random(`x${i}`) * 960, y = random(`y${i}`) * 1800 + Math.sin(frame / 20 + i) * 15;
          return <g key={i} transform={`translate(${x} ${y}) rotate(${random(`r${i}`) * 360 + frame * (random(`v${i}`) - 0.5)}) scale(${s * 1.3})`}><Shape k={i} /></g>;
        })}
      </svg>
      <AbsoluteFill style={{justifyContent: 'center', alignItems: 'center'}}>
        <div style={{whiteSpace: 'pre', textAlign: 'center', fontFamily: '"Arial Black"', fontSize: 150, lineHeight: 0.95, color: '#845EC2', textShadow: '8px 8px 0 #FFC75F, 12px 12px 0 #000'}}>{text}</div>
      </AbsoluteFill>
    </AbsoluteFill>
  );
};
```

## Variations
- Damier qui défile en fond (`repeating-conic-gradient(#000 0 25%, #fff 0 50%)`).
- Cadre de texte en forme de nuage BD avec ombres plates.

## Pièges
- Formes placées sous le texte = illisible : réserver une zone libre au centre.
- Trop de motifs = bouillie. 20–25 pour du 9:16.

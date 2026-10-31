# Wave & Topographic · `/animation topographic`

> Famille : Mouvements organiques · #19

## C'est quoi
Des lignes de niveau (carte topographique) ou des vagues mathématiques empilées qui ondulent en boucle.

## Quand l'utiliser
- Fond élégant pour data, outdoor, running, musique (façon pochette « Unknown Pleasures »).

## Technique clé
- **Lignes empilées** : 40 lignes horizontales, chaque point `y = baseY - amp * bump(x) * noise(x, ligne, t)`.
- Bruit : somme de sinus `sin(x*0.012 + t + j*0.4) + 0.5*sin(x*0.031 - t*1.3 + j)`.
- `bump(x)` = gaussienne centrée pour concentrer l'amplitude au milieu.
- Remplissage noir sous chaque ligne pour masquer les lignes derrière (effet 3D).
- Boucle parfaite : `t = (frame / durée) * 2π`.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame, useVideoConfig} from 'remotion';

export const Topographic: React.FC<{text?: string; lines?: number}> = ({lines = 40}) => {
  const frame = useCurrentFrame();
  const {durationInFrames} = useVideoConfig();
  const t = (frame / durationInFrames) * Math.PI * 2;
  const W = 800;
  return (
    <AbsoluteFill style={{background: '#000', justifyContent: 'center', alignItems: 'center'}}>
      <svg width={W} height={1200}>
        {Array.from({length: lines}, (_, j) => {
          const base = 200 + j * 22;
          const pts: string[] = [];
          for (let x = 0; x <= W; x += 8) {
            const bump = Math.exp(-((x - W / 2) ** 2) / (2 * 140 ** 2));
            const n = Math.sin(x * 0.012 + t + j * 0.4) + 0.5 * Math.sin(x * 0.031 - t * 2 + j);
            pts.push(`${x},${base - 70 * bump * (1 + n)}`);
          }
          return <polyline key={j} points={`0,1200 ${pts.join(' ')} ${W},1200`} fill="#000" stroke="#fff" strokeWidth={2} />;
        })}
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- **Courbes de niveau** vraies : marching squares sur un champ de bruit 2D (plus coûteux).
- Lignes colorées en dégradé (cyan → violet selon `j`).
- Synchro audio : amplitude pilotée par le spectre.

## Pièges
- Sans remplissage, les lignes se croisent et l'effet relief disparaît.
- Boucle : utiliser des fréquences de `t` entières (1, 2) sinon saut à la fin.

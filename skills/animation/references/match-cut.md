# Match Cut / Morph Cut · `/animation match-cut`

> Famille : Transitions & caméra · #93

## C'est quoi
Un raccord parfait entre deux plans grâce à une forme, une position ou un mouvement identiques : un ballon devient la Lune, un œil devient un objectif.

## Quand l'utiliser
- Transitions « wow » mémorables, storytelling visuel, montages créatifs.

## Technique clé
- Repérer la **forme commune** et la placer au **même endroit, même taille** dans les deux plans (aligner via `transform` sur chaque plan).
- Le cut se fait sur le temps fort ; pour adoucir, morph de 6–10 frames : interpoler taille/position/couleur/`borderRadius` de la forme A vers la forme B, pendant que les décors font un fondu.
- Mouvement continu : même direction et vitesse avant/après le cut (ex. zoom avant qui continue).
- En motion design pur : un cercle qui grandit devient le soleil de la scène suivante, un carré devient l'écran d'un téléphone.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, interpolateColors, useCurrentFrame} from 'remotion';

export const MatchCut: React.FC<{text?: string; at?: number}> = ({text = 'Le soleil', at = 30}) => {
  const frame = useCurrentFrame();
  const opts = {extrapolateLeft: 'clamp' as const, extrapolateRight: 'clamp' as const, easing: Easing.inOut(Easing.cubic)};
  const p = interpolate(frame, [at, at + 10], [0, 1], opts);
  const size = interpolate(p, [0, 1], [220, 420]);
  const y = interpolate(p, [0, 1], [1200, 760]);
  const color = interpolateColors(p, [0, 1], ['#FF6B35', '#FFD166']);
  return (
    <AbsoluteFill>
      <AbsoluteFill style={{background: '#2E7D32', opacity: 1 - p}}>
        <div style={{position: 'absolute', bottom: 0, height: 600, left: 0, right: 0, background: '#1B5E20'}} />
      </AbsoluteFill>
      <AbsoluteFill style={{background: 'linear-gradient(#FF8C42, #6A2C70)', opacity: p}} />
      <div style={{position: 'absolute', left: 540 - size / 2, top: y - size / 2, width: size, height: size, borderRadius: '50%', background: color,
        boxShadow: `0 0 ${120 * p}px ${color}`,
        backgroundImage: p < 0.5 ? 'repeating-conic-gradient(#fff 0 15deg, transparent 15deg 45deg)' : undefined, backgroundBlendMode: 'overlay'}} />
      <div style={{position: 'absolute', top: 1400, width: '100%', textAlign: 'center', fontFamily: 'Georgia', fontStyle: 'italic', fontSize: 90, color: '#fff', opacity: p}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Vrais plans vidéo : aligner les deux sources via `transform` image par image (keyframes de position).
- Match cut sur mouvement (une main qui lance → une autre main qui attrape).
- Logo qui se forme à partir d'un objet du plan.

## Pièges
- Formes mal alignées d'un plan à l'autre = le cerveau voit un saut. Mesurer au pixel près.
- Trop d'éléments différents autour : isoler la forme commune (fond simple, contraste).

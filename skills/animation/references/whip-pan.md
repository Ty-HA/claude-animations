# Whip Pan · `/animation whip-pan`

> Famille : Transitions & caméra · #82

## C'est quoi
Un panoramique ultra-rapide avec flou de mouvement : la caméra « fouette » d'un plan à l'autre.

## Quand l'utiliser
- Enchaîner deux plans avec énergie, changement de lieu, rythme vlog/sport.

## Technique clé
- Durée 8–12 frames. Scène A sort vers la gauche, scène B entre par la droite, collées : `xA = -p * W`, `xB = (1 - p) * W`.
- Easing très marqué : `Easing.inOut(Easing.exp)` (lent → très rapide → lent).
- **Flou directionnel** proportionnel à la vitesse : SVG `feGaussianBlur stdDeviation="${v} 0"` (flou horizontal seulement) avec `v = |dp/df| * 400`.
- Option : léger `skewX(±8deg)` au pic de vitesse.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame, useVideoConfig} from 'remotion';

const scene = (bg: string, label: string) => (
  <AbsoluteFill style={{background: bg, justifyContent: 'center', alignItems: 'center', fontFamily: 'Inter', fontWeight: 900, fontSize: 160, color: '#fff'}}>{label}</AbsoluteFill>
);

export const WhipPan: React.FC<{text?: string; at?: number; dur?: number}> = ({text = 'PARIS', at = 30, dur = 10}) => {
  const frame = useCurrentFrame();
  const {width: W} = useVideoConfig();
  const ease = Easing.inOut(Easing.exp);
  const opts = {extrapolateLeft: 'clamp' as const, extrapolateRight: 'clamp' as const, easing: ease};
  const p = interpolate(frame, [at, at + dur], [0, 1], opts);
  const v = Math.abs(p - interpolate(frame - 1, [at, at + dur], [0, 1], opts));
  const blur = Math.min(80, v * 400);
  return (
    <AbsoluteFill style={{overflow: 'hidden'}}>
      <svg width={0} height={0}><filter id="mb"><feGaussianBlur stdDeviation={`${blur} 0`} /></filter></svg>
      <AbsoluteFill style={{filter: blur > 0.5 ? 'url(#mb)' : undefined}}>
        <AbsoluteFill style={{transform: `translateX(${-p * W}px)`}}>{scene('#155E75', 'TOKYO')}</AbsoluteFill>
        <AbsoluteFill style={{transform: `translateX(${(1 - p) * W}px)`}}>{scene('#A855F7', text)}</AbsoluteFill>
      </AbsoluteFill>
    </AbsoluteFill>
  );
};
```

## Variations
- Whip vertical (vers le haut) pour un enchaînement « suivant ».
- Avec vraies vidéos : `<OffthreadVideo>` dans chaque scène, le flou masque le raccord.
- Son « whoosh » calé sur le pic de vitesse.

## Pièges
- Flou omnidirectionnel = effet « mauvaise mise au point ». Flou horizontal uniquement.
- Transition trop longue (>15 frames) : perd tout son punch.

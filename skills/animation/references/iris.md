# Iris / Circle Reveal · `/animation iris`

> Famille : Transitions & caméra · #86

## C'est quoi
Une ouverture ou une fermeture en cercle, comme la fin des vieux dessins animés (« That's all folks »), souvent centrée sur un sujet.

## Quand l'utiliser
- Fin de vidéo, focus sur un élément, transition rétro/cartoon, reveal.

## Technique clé
- `clipPath: circle(r at x y)` sur la scène suivante (ouverture) ou sur la scène actuelle (fermeture).
- Rayon en `px` : de 0 à `hypot(W, H)` (≈ 2200 px en 1080×1920).
- Fermeture façon cartoon : se resserrer jusqu'à un petit cercle (120 px) sur le sujet, pause 10 frames, puis 0.
- Easing `Easing.inOut(Easing.cubic)` ; bord net ou léger halo (anneau `boxShadow`).

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

export const Iris: React.FC<{text?: string; x?: number; y?: number}> = ({text = "C'est tout !", x = 540, y = 900}) => {
  const frame = useCurrentFrame();
  const r = interpolate(frame, [0, 25, 40, 50], [2200, 140, 140, 0], {extrapolateRight: 'clamp', easing: Easing.inOut(Easing.cubic)});
  return (
    <AbsoluteFill style={{background: '#000'}}>
      <AbsoluteFill style={{clipPath: `circle(${r}px at ${x}px ${y}px)`, background: 'linear-gradient(#FFD166, #EF476F)', justifyContent: 'center', alignItems: 'center'}}>
        <div style={{fontFamily: '"Bangers", Impact', fontSize: 150, color: '#fff', textShadow: '6px 6px 0 #000'}}>{text}</div>
      </AbsoluteFill>
    </AbsoluteFill>
  );
};
```

## Variations
- Iris en forme de cœur, d'étoile ou de logo (`clipPath: path(...)` ou `mask-image`).
- Ouverture depuis l'œil d'un personnage.
- Double iris (deux cercles) façon jumelles.

## Pièges
- Rayon en % = ellipse sur un écran non carré. Utiliser des px.
- Centre mal placé : cibler précisément le sujet (coordonnées en px du cadre final).

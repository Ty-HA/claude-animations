# Glitch & VHS Rétro · `/animation glitch`

> Famille : Typographie cinétique · #3

## C'est quoi
Distorsions numériques et analogiques : tranches d'image décalées, aberration chromatique (rouge / cyan séparés), lignes de scan et bruit façon cassette VHS.

## Quand l'utiliser
- Intro tech, cyberpunk, « bug », révélation choc.
- Transition sèche entre deux plans (6 à 10 frames de glitch).

## Technique clé
- **Aberration chromatique** : 3 copies du texte, rouge `#FF2A6D` décalée de −x, cyan `#00F0FF` de +x, blanche au centre, `mix-blend-mode: screen`.
- **Tranches** : 4 à 6 bandes horizontales (`clip-path: inset(top% 0 bottom% 0)`) décalées de ±10 à 40 px, re-tirées toutes les 3 frames avec `random(seed + Math.floor(frame/3))`.
- **Rafales** : glitch fort 4 frames toutes les ~20 frames, calme entre deux.
- **VHS** : scanlines `repeating-linear-gradient(0deg, rgba(255,255,255,.05) 0 1px, transparent 1px 3px)` + bruit SVG `feTurbulence` à 0.08 d'opacité.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

export const Glitch: React.FC<{text?: string}> = ({text = 'GLITCH'}) => {
  const frame = useCurrentFrame();
  const tick = Math.floor(frame / 3);
  const burst = frame % 24 < 5 ? 1 : 0.15;
  const off = (random(`o${tick}`) - 0.5) * 30 * burst;
  const style = (color: string, dx: number): React.CSSProperties => ({
    position: 'absolute', color, mixBlendMode: 'screen', transform: `translateX(${dx}px)`,
    fontFamily: 'monospace', fontWeight: 900, fontSize: 180, letterSpacing: 8,
  });
  const slices = Array.from({length: 5}, (_, i) => {
    const top = random(`t${tick}-${i}`) * 90;
    const h = 3 + random(`h${tick}-${i}`) * 10;
    const dx = (random(`d${tick}-${i}`) - 0.5) * 80 * burst;
    return <div key={i} style={{...style('#fff', dx), clipPath: `inset(${top}% 0 ${100 - top - h}% 0)`}}>{text}</div>;
  });
  return (
    <AbsoluteFill style={{background: '#05040d', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{position: 'relative', display: 'flex', justifyContent: 'center', alignItems: 'center'}}>
        <div style={style('#FF2A6D', -6 - off)}>{text}</div>
        <div style={style('#00F0FF', 6 + off)}>{text}</div>
        <div style={{...style('#fff', 0), position: 'relative'}}>{text}</div>
        {burst > 0.5 && slices}
      </div>
      <AbsoluteFill style={{background: 'repeating-linear-gradient(0deg, rgba(255,255,255,.05) 0 1px, transparent 1px 3px)'}} />
    </AbsoluteFill>
  );
};
```

## Variations
- **Glitch d'image** : appliquer les mêmes tranches à une `<Img>` ou une vidéo.
- **Datamosh léger** : `filter: hue-rotate()` aléatoire pendant les rafales.
- **Tracking VHS** : une bande floue qui descend lentement (`top = (frame*4) % 1920`).

## Pièges
- Glitch permanent = fatigue visuelle et risque photosensible : garder des rafales courtes.
- Toujours `random()` de Remotion avec une graine liée à la frame, jamais `Math.random()` (rendu non reproductible).

# Sticker Pop · `/animation sticker-pop`

> Famille : Personnages & fun · #97

## C'est quoi
Des stickers (emojis, badges, mots, photos détourées avec contour blanc) qui surgissent avec un rebond élastique et se collent légèrement de travers.

## Quand l'utiliser
- Vidéos TikTok/Reels dynamiques, réactions, mise en avant de mots, collages fun.

## Technique clé
- Contour blanc épais : `filter: drop-shadow(0 0 0 #fff)` ne suffit pas → empiler 4–8 `drop-shadow(±4px ±4px 0 #fff)` ou utiliser un SVG `feMorphology operator="dilate" radius="6"` + `feFlood` blanc.
- Ombre portée douce sous le sticker.
- Pop : `spring({damping: 8, stiffness: 200})` sur `scale` de 0 → 1 (dépasse ~1.2), rotation finale aléatoire ±12°.
- Collage : légère « respiration » ensuite (`scale 1 ± 0.02`).

## Snippet Remotion
```tsx
import {AbsoluteFill, random, spring, useCurrentFrame, useVideoConfig} from 'remotion';

const STICKERS = [
  {c: '🔥', x: 220, y: 520, at: 5}, {c: 'WOW', x: 720, y: 700, at: 12}, {c: '✨', x: 300, y: 1100, at: 19},
  {c: 'NEW', x: 780, y: 1250, at: 26}, {c: '💜', x: 540, y: 880, at: 33},
];

export const StickerPop: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  return (
    <AbsoluteFill style={{background: 'linear-gradient(160deg,#FFD6E8,#C8E7FF)'}}>
      <svg width={0} height={0}>
        <filter id="outline"><feMorphology in="SourceAlpha" operator="dilate" radius={8} result="d" /><feFlood floodColor="#fff" /><feComposite in2="d" operator="in" result="o" />
          <feMerge><feMergeNode in="o" /><feMergeNode in="SourceGraphic" /></feMerge></filter>
      </svg>
      {STICKERS.map((s, i) => {
        const p = spring({frame: frame - s.at, fps, config: {damping: 8, stiffness: 200}});
        const rot = (random(`r${i}`) - 0.5) * 24;
        const emoji = s.c.length <= 2;
        return (
          <div key={i} style={{position: 'absolute', left: s.x, top: s.y, transform: `translate(-50%,-50%) scale(${p * (1 + 0.02 * Math.sin(frame / 8 + i))}) rotate(${rot * p}deg)`,
            filter: 'url(#outline) drop-shadow(0 10px 14px rgba(0,0,0,.25))', fontSize: emoji ? 200 : 110, fontFamily: '"Archivo Black", "Arial Black"',
            color: '#A855F7', ...(emoji ? {} : {background: '#FFE14D', padding: '10px 30px', borderRadius: 24})}}>{s.c}</div>
        );
      })}
    </AbsoluteFill>
  );
};
```

## Variations
- Photo détourée (PNG transparent) en sticker avec contour blanc.
- Sticker qui « se décolle » (rotation 3D d'un coin) pour sortir.
- Pile de stickers sur un mot-clé synchronisé à la voix.

## Pièges
- `feMorphology` sur de très grands éléments est coûteux : rayon ≤ 10.
- Trop de stickers en même temps : séquencer (6–8 frames d'écart).

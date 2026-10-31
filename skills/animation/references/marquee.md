# Infinite Scroll / Marquee · `/animation marquee`

> Famille : Tableaux de bord & UI · #69

## C'est quoi
Des bandeaux de texte ou de logos qui défilent à l'infini, souvent plusieurs bandes en sens opposés et légèrement inclinées.

## Quand l'utiliser
- Logos clients, mots-clés, annonce « NOUVEAU », ambiance énergique en fond.

## Technique clé
- Contenu dupliqué 2× (ou plus) côte à côte ; `translateX(-((frame * vitesse) % largeurUnité))` → boucle parfaite.
- Mesurer la largeur d'une unité : fixer des éléments de largeur connue, ou estimer et répéter 4×.
- Bandes inclinées (`rotate(-6deg)`), alternance de sens et de couleurs, séparateurs (★, ●, ✦).
- Vitesse : 3–8 px/frame.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

const Band: React.FC<{items: string[]; speed: number; bg: string; color: string; rot: number; top: number}> = ({items, speed, bg, color, rot, top}) => {
  const frame = useCurrentFrame();
  const UNIT = 1600; // largeur d'un bloc (items à largeur fixe)
  const x = -(((frame * speed) % UNIT) + UNIT) % UNIT;
  const block = (k: number) => (
    <div key={k} style={{display: 'flex', width: UNIT, flexShrink: 0}}>
      {items.map((t, i) => <div key={i} style={{width: UNIT / items.length, textAlign: 'center'}}>{t} ✦</div>)}
    </div>
  );
  return (
    <div style={{position: 'absolute', left: -200, right: -200, top, background: bg, color, transform: `rotate(${rot}deg)`, padding: '24px 0', overflow: 'hidden',
      fontFamily: '"Archivo Black", "Arial Black"', fontSize: 64, whiteSpace: 'nowrap'}}>
      <div style={{display: 'flex', transform: `translateX(${x}px)`}}>{[0, 1, 2].map(block)}</div>
    </div>
  );
};

export const Marquee: React.FC<{text?: string}> = ({text = 'COMMENTE ANIMATION'}) => (
  <AbsoluteFill style={{background: '#010012'}}>
    <Band items={[text, '100 STYLES', 'CLAUDE']} speed={6} bg="#00B5C7" color="#010012" rot={-6} top={700} />
    <Band items={['REMOTION', 'TIKTOK', 'REELS', 'SKILL']} speed={-5} bg="#EC4899" color="#fff" rot={5} top={900} />
  </AbsoluteFill>
);
```

## Variations
- Logos en niveaux de gris qui défilent (preuve sociale).
- Marquee vertical (colonnes de cartes qui défilent en sens opposés).
- Vitesse qui accélère sur le beat.

## Pièges
- Unité mal mesurée = saut visible à chaque boucle. Largeurs fixes ou `measureText`.
- Vitesse négative : le modulo JS peut être négatif, d'où le `+ UNIT) % UNIT`.

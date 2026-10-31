# Holographic Foil · `/animation holo-foil`

> Famille : Styles 3D · #43

## C'est quoi
Le reflet irisé arc-en-ciel des cartes holographiques (Pokémon rares, billets, stickers holo) qui change de couleur avec l'angle.

## Quand l'utiliser
- Carte collector, badge, ticket, packaging premium, reveal « rare ».

## Technique clé
- Carte qui s'incline : `perspective(1200px) rotateY(ry) rotateX(rx)` avec `ry = 18 sin(t)`, `rx = 10 cos(t*0.8)`.
- Couche holo : `linear-gradient(115deg, #ff008c, #ffcd00, #00ff9d, #00c8ff, #a000ff, #ff008c)` en `backgroundSize: 300%`, dont `backgroundPosition` suit l'angle (`50 + ry*3 %`), `mixBlendMode: 'color-dodge'`, opacité 0.6.
- Texture de motifs (étoiles, lignes) en `mask-image` pour que l'holo n'apparaisse que sur les motifs.
- Reflet blanc : `radial-gradient` qui suit l'inclinaison, `mixBlendMode: 'overlay'`.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

export const HoloFoil: React.FC<{text?: string}> = ({text = 'RARE'}) => {
  const t = useCurrentFrame() / 30;
  const ry = 18 * Math.sin(t * 1.4), rx = 10 * Math.cos(t * 1.1);
  return (
    <AbsoluteFill style={{background: '#111', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{width: 560, height: 800, borderRadius: 32, position: 'relative', overflow: 'hidden',
        transform: `perspective(1200px) rotateY(${ry}deg) rotateX(${rx}deg)`, background: 'linear-gradient(160deg,#2b2b3a,#14141c)',
        boxShadow: '0 40px 80px rgba(0,0,0,.6)'}}>
        <div style={{position: 'absolute', inset: 0, mixBlendMode: 'color-dodge', opacity: 0.65,
          background: 'linear-gradient(115deg,#ff008c,#ffcd00,#00ff9d,#00c8ff,#a000ff,#ff008c)', backgroundSize: '300% 300%',
          backgroundPosition: `${50 + ry * 3}% ${50 + rx * 3}%`}} />
        <div style={{position: 'absolute', inset: 0, mixBlendMode: 'overlay',
          background: `radial-gradient(circle at ${50 + ry * 2}% ${50 - rx * 3}%, rgba(255,255,255,.8), transparent 45%)`}} />
        <div style={{position: 'absolute', inset: 0, display: 'flex', justifyContent: 'center', alignItems: 'center',
          fontFamily: 'Inter', fontWeight: 900, fontSize: 130, color: '#fff', letterSpacing: 10}}>{text}</div>
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Motifs holo : `maskImage: repeating-linear-gradient(45deg, #000 0 2px, transparent 2px 8px)`.
- Texte holo seul (`background-clip: text`) sur fond noir.
- Combiner avec `card-flip` pour retourner une carte rare.

## Pièges
- `color-dodge` sur fond très clair = blanc brûlé. Fond de carte sombre.
- Inclinaison > 25° : l'effet paraît faux.

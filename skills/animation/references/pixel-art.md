# Pixel Art / 8-Bit · `/animation pixel-art`

> Famille : Esthétiques artistiques · #52

## C'est quoi
L'esthétique des jeux vidéo rétro : gros pixels nets sans lissage, palette limitée, sprites animés image par image.

## Quand l'utiliser
- Gaming, nostalgie, « niveau débloqué », barres de vie, explications ludiques.

## Technique clé
- Dessiner sur une **petite grille** (ex. 64×114) puis agrandir ×16 avec `image-rendering: pixelated`.
- Sprites : tableaux de chaînes (`'..XX..'`) + palette (`{X: '#000', R: '#e43b44'}`) rendus en `<rect>` ou sur canvas.
- Animation : 2 à 4 frames de sprite alternées tous les 6–8 frames vidéo.
- Mouvements par **pas entiers** de pixel : `Math.round(x)`.
- Police pixel : « Press Start 2P » (Google Fonts).
- Palette type PICO-8 : `#1D2B53 #7E2553 #008751 #AB5236 #FF004D #FFA300 #FFEC27 #00E436 #29ADFF #FFF1E8`.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

const PAL: Record<string, string> = {K: '#000', R: '#FF004D', P: '#FFCCAA', B: '#29ADFF', W: '#FFF1E8'};
const HERO = [
  ['...RRRR...', '..RRRRRRR.', '..KKPPKP..', '.KPKPPKPPP', '.KPKKPPKPP', '..PPPPKKK.', '...BBRBB..', '..BBBRBBB.', '.PPBRRRBPP', '...RR.RR..', '..KKK.KKK.'],
  ['...RRRR...', '..RRRRRRR.', '..KKPPKP..', '.KPKPPKPPP', '.KPKKPPKPP', '..PPPPKKK.', '...BBRBB..', '..BBBRBBB.', '.PPBRRRBPP', '....RRR...', '....KKKK..'],
];
const PX = 20;

export const PixelArt: React.FC<{text?: string}> = ({text = 'LEVEL UP!'}) => {
  const frame = useCurrentFrame();
  const sprite = HERO[Math.floor(frame / 6) % 2];
  const x = Math.round(((frame * 4) % 1300) - 200);
  const jump = Math.round(Math.max(0, Math.sin((frame / 30) * Math.PI)) * 8) * PX;
  return (
    <AbsoluteFill style={{background: '#1D2B53', imageRendering: 'pixelated'}}>
      <div style={{position: 'absolute', top: 400, width: '100%', textAlign: 'center', fontFamily: '"Press Start 2P", monospace', fontSize: 64, color: '#FFEC27',
        textShadow: '6px 6px 0 #7E2553', opacity: Math.floor(frame / 10) % 2 ? 1 : 0.4}}>{text}</div>
      <svg width={1080} height={1920} shapeRendering="crispEdges">
        <rect y={1300} width={1080} height={620} fill="#008751" />
        <rect y={1300} width={1080} height={PX} fill="#00E436" />
        <g transform={`translate(${x} ${1300 - sprite.length * PX - jump})`}>
          {sprite.flatMap((row, j) => row.split('').map((c, i) => (c === '.' ? null : <rect key={`${i}-${j}`} x={i * PX} y={j * PX} width={PX} height={PX} fill={PAL[c]} />)))}
        </g>
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Pièces qui tournent (4 frames : rond, ovale, trait, ovale).
- Barre de vie / XP qui se remplit par pas.
- Pixeliser une image : rendre petite sur canvas puis agrandir en `pixelated`.

## Pièges
- Positions non entières = pixels flous ou déformés. Toujours arrondir.
- `shapeRendering="crispEdges"` sur le SVG sinon des fentes apparaissent entre pixels.

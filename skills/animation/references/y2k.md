# Y2K / Chrome Bubble · `/animation y2k`

> Famille : Esthétiques artistiques · #59

## C'est quoi
L'esthétique an 2000 : texte chromé « bulle » gonflé, étoiles scintillantes à 4 branches, papillons, dégradés holographiques, cyber-rose, lettres en métal liquide.

## Quand l'utiliser
- Mode, beauté, musique pop, nostalgie 2000s, contenus Gen Z.

## Technique clé
- Texte « bubble chrome » : police très ronde (« Rubik Bubbles », « Bungee Shade ») + dégradé chrome `linear-gradient(#fff, #c0c6d0 35%, #4b5563 50%, #f3f4f6 55%, #9ca3af 80%, #fff)` en `background-clip: text` + `drop-shadow` rose.
- Étoiles 4 branches : `polygon` (losange étiré en croix) qui scintillent (`scale` 0 → 1 → 0 sur 20 frames, décalées).
- Fond : dégradé `#FF9CEE → #B28DFF → #97A2FF` ou noir avec grille rose.
- Rotation du reflet (`backgroundPosition` animé) pour un chrome vivant.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

const Sparkle: React.FC<{x: number; y: number; s: number}> = ({x, y, s}) => (
  <svg width={120} height={120} style={{position: 'absolute', left: x - 60, top: y - 60, transform: `scale(${s})`}}>
    <polygon points="60,0 70,50 120,60 70,70 60,120 50,70 0,60 50,50" fill="#fff" style={{filter: 'drop-shadow(0 0 10px #FF9CEE)'}} />
  </svg>
);

export const Y2K: React.FC<{text?: string}> = ({text = 'y2k'}) => {
  const frame = useCurrentFrame();
  return (
    <AbsoluteFill style={{background: 'linear-gradient(160deg, #FF9CEE, #B28DFF 50%, #97A2FF)', justifyContent: 'center', alignItems: 'center'}}>
      {Array.from({length: 12}, (_, i) => {
        const ph = ((frame + random(`p${i}`) * 40) % 40) / 40;
        return <Sparkle key={i} x={random(`x${i}`) * 1080} y={random(`y${i}`) * 1920} s={Math.sin(ph * Math.PI) * (0.4 + random(`s${i}`) * 0.8)} />;
      })}
      <div style={{fontFamily: '"Rubik Bubbles", "Arial Rounded MT Bold", sans-serif', fontSize: 300, lineHeight: 1,
        backgroundImage: 'linear-gradient(#fff, #c0c6d0 35%, #4b5563 50%, #f3f4f6 55%, #9ca3af 80%, #fff)',
        backgroundSize: '100% 200%', backgroundPosition: `0 ${50 + 30 * Math.sin(frame / 20)}%`,
        WebkitBackgroundClip: 'text', color: 'transparent', filter: 'drop-shadow(0 8px 0 #FF4FD8) drop-shadow(0 0 30px #fff)'}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Papillons et cœurs chromés qui flottent.
- Interface « téléphone à clapet » / lecteur MP3 en plastique translucide.

## Pièges
- Sans police vraiment ronde, le chrome paraît plat : charger « Rubik Bubbles ».
- Trop d'étoiles = brouillon ; 8–15 suffisent.

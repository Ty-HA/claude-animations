# Skeuomorphism · `/animation skeuomorphism`

> Famille : Styles 3D · #35

## C'est quoi
Des objets d'interface qui imitent le réel : vrais boutons qui s'enfoncent, interrupteurs métalliques, potentiomètres, cuir cousu, bois, LED qui s'allument.

## Quand l'utiliser
- Nostalgie iOS 6, synthés/audio, rétro-tech, « vraie machine ».

## Technique clé
- Métal brossé : `repeating-linear-gradient(90deg, #d9d9d9 0 2px, #c4c4c4 2px 4px)` + `linear-gradient` clair/sombre en surcouche.
- Bouton physique : dégradé vertical + `boxShadow: 0 6px 0 #555, 0 10px 20px rgba(0,0,0,.4)`. Appuyé : `translateY(6px)` et ombre `0 0 0`.
- Potentiomètre (knob) : disque avec stries `conic-gradient`, rotation `-135° → +135°`, encoche indicatrice.
- LED : cercle + `radial-gradient` et halo `boxShadow 0 0 20px` quand allumée.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

export const Skeuomorphism: React.FC<{text?: string}> = ({text = 'REC'}) => {
  const frame = useCurrentFrame();
  const angle = interpolate(frame, [0, 60], [-135, 100], {extrapolateRight: 'clamp', easing: Easing.inOut(Easing.cubic)});
  const pressed = frame > 70 && frame < 80;
  const led = frame > 75;
  return (
    <AbsoluteFill style={{background: 'repeating-linear-gradient(90deg,#3a2a1e 0 6px,#33251a 6px 12px)', justifyContent: 'center', alignItems: 'center', gap: 120}}>
      <div style={{width: 300, height: 300, borderRadius: '50%', transform: `rotate(${angle}deg)`,
        background: 'conic-gradient(from 0deg, #eee, #999, #eee, #888, #eee, #999, #eee)',
        boxShadow: '0 20px 40px rgba(0,0,0,.6), inset 0 4px 6px rgba(255,255,255,.7)', position: 'relative'}}>
        <div style={{position: 'absolute', left: 142, top: 18, width: 16, height: 60, borderRadius: 8, background: '#222'}} />
      </div>
      <div style={{display: 'flex', alignItems: 'center', gap: 40}}>
        <div style={{width: 60, height: 60, borderRadius: '50%', background: led ? 'radial-gradient(circle at 40% 35%, #ffb3b3, #ff2020 60%)' : '#5a1010',
          boxShadow: led ? '0 0 30px #ff2020' : 'inset 0 3px 6px rgba(0,0,0,.6)'}} />
        <div style={{padding: '30px 60px', borderRadius: 18, fontFamily: 'Helvetica', fontWeight: 700, fontSize: 56, color: '#333',
          background: 'linear-gradient(#fafafa, #cfcfcf)', transform: `translateY(${pressed ? 8 : 0}px)`,
          boxShadow: pressed ? '0 0 0 #555, 0 2px 6px rgba(0,0,0,.4)' : '0 8px 0 #555, 0 14px 24px rgba(0,0,0,.5)'}}>{text}</div>
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Calculatrice/Walkman rétro complet, bande magnétique qui tourne.
- Interrupteur à bascule métal (rotation 3D `rotateX ±20°`).
- Cuir cousu : texture bruit + bordure `dashed` blanche.

## Pièges
- Mélanger plusieurs directions de lumière casse l'illusion : lumière toujours venant du haut.
- Trop de textures = chargé. Une matière principale par plan.

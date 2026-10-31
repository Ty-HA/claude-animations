# Confetti / Emoji Rain · `/animation confetti`

> Famille : Personnages & fun · #96

## C'est quoi
Une pluie de confettis colorés (ou d'emojis) qui explose puis retombe en tourbillonnant.

## Quand l'utiliser
- Célébration, palier atteint, annonce, résultat de concours, fin de vidéo.

## Technique clé
- Chaque confetti : position initiale (canon en bas ou pluie en haut), vitesse initiale `vx, vy` aléatoires seedées, gravité `g = 0.5 px/frame²`, frottement de l'air (vitesse terminale) : `y = y0 + vy*t + 0.5*g*t²`, avec `vy` plafonnée.
- Oscillation latérale : `x += sin(t * f + φ) * 30` (feuille qui flotte).
- Rotation 3D : `rotateX(t * vr) rotateZ(t * vr2)` → le confetti « clignote » en tournant.
- Formes : rectangles 14×24, cercles, rubans ; 6 couleurs vives.
- Emoji rain : même physique avec `<span>` 🎉🔥❤️ de 60–90 px.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

const COLORS = ['#FF4D6D', '#FFD60A', '#00B5C7', '#A855F7', '#22C55E', '#FF8C42'];

export const Confetti: React.FC<{text?: string; count?: number; at?: number}> = ({text = 'BRAVO !', count = 160, at = 5}) => {
  const frame = useCurrentFrame();
  const t = Math.max(0, frame - at);
  return (
    <AbsoluteFill style={{background: '#0b0b14', overflow: 'hidden'}}>
      <AbsoluteFill style={{justifyContent: 'center', alignItems: 'center', fontFamily: 'Inter', fontWeight: 900, fontSize: 150, color: '#fff'}}>{text}</AbsoluteFill>
      {frame >= at && Array.from({length: count}, (_, i) => {
        const side = i % 2 ? 1 : -1;
        const vx = side * -(4 + random(`vx${i}`) * 14);
        const vy = -(28 + random(`vy${i}`) * 22);
        const g = 0.9;
        const vyT = Math.min(vy + g * t, 6 + random(`vt${i}`) * 4); // vitesse terminale
        const tPeak = (vyT - vy) / g;
        const y = 1920 + vy * tPeak + 0.5 * g * tPeak * tPeak + vyT * (t - tPeak);
        const x = (side > 0 ? 1080 : 0) + vx * Math.min(t, 40) + Math.sin(t * 0.15 + i) * 30;
        return <div key={i} style={{position: 'absolute', left: x, top: y, width: 16, height: i % 3 ? 26 : 16, borderRadius: i % 3 ? 3 : 8,
          background: COLORS[i % COLORS.length], transform: `rotateX(${t * (8 + random(`r${i}`) * 12)}deg) rotateZ(${t * 6 + i * 20}deg)`}} />;
      })}
    </AbsoluteFill>
  );
};
```

## Variations
- Pluie depuis le haut (vy initiale positive faible, x aléatoire sur la largeur).
- Emoji rain : remplacer les `div` par des emojis, rotation 2D seulement.
- Canon unique au centre avec dispersion en éventail.

## Pièges
- Ne pas accumuler les positions entre frames (Remotion rend dans le désordre) : formule fermée en fonction de `t`.
- 300+ éléments DOM avec 3D = lent : rester ≤ 200 ou passer en canvas.

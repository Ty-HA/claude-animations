# Squash & Stretch · `/animation squash-stretch`

> Famille : Personnages & fun · #98

## C'est quoi
Le principe d'animation n°1 de Disney : un objet s'**étire** quand il va vite et s'**écrase** à l'impact, en conservant son volume. Tout devient élastique et vivant.

## Quand l'utiliser
- Balles, logos qui rebondissent, boutons, mascottes, texte « cartoon ».

## Technique clé
- Conservation du volume : `scaleX = 1 / scaleY`.
- **Étirement** proportionnel à la vitesse : `sy = 1 + k * |v|`, orienté dans la direction du mouvement.
- **Écrasement** à l'impact (2–4 frames) : `sy = 0.6`, `sx = 1/0.6`, `transformOrigin: 'bottom'`.
- Rebond : `y = |sin(t)| * h * decay^n` (hauteur qui diminue) ; anticipation avant le saut (petit écrasement avant de partir).
- Ombre au sol qui rétrécit quand l'objet monte.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

const PERIOD = 30;

export const SquashStretch: React.FC<{text?: string}> = ({text = 'B'}) => {
  const frame = useCurrentFrame();
  const n = Math.floor(frame / PERIOD), ph = (frame % PERIOD) / PERIOD;
  const H = 700 * Math.pow(0.75, n);
  const y = Math.sin(ph * Math.PI) * H;
  const v = Math.cos(ph * Math.PI) * H * Math.PI / PERIOD; // vitesse verticale
  const impact = ph < 0.08 || ph > 0.92;
  const sy = impact ? 0.62 : 1 + Math.min(0.45, Math.abs(v) / 120);
  const sx = 1 / sy;
  return (
    <AbsoluteFill style={{background: '#FFF4E0'}}>
      <div style={{position: 'absolute', left: 540 - 130 * (1 - y / 1000), top: 1400, width: 260 * (1 - y / 1000), height: 40, borderRadius: '50%',
        background: 'rgba(0,0,0,.18)'}} />
      <div style={{position: 'absolute', left: 540 - 130, top: 1420 - 260 - y, width: 260, height: 260, borderRadius: '50%',
        background: 'radial-gradient(circle at 35% 30%, #FF9AA2, #FF4D6D 60%, #C9184A)', transform: `scale(${sx}, ${sy})`, transformOrigin: 'bottom',
        display: 'flex', justifyContent: 'center', alignItems: 'center', fontFamily: '"Arial Black"', fontSize: 140, color: '#fff'}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Texte élastique : chaque lettre rebondit avec un décalage.
- Mouvement horizontal : étirement en X dans le sens de la course.
- Anticipation : écrasement de 4 frames avant chaque saut.

## Pièges
- Oublier la conservation du volume (`sx = 1/sy`) = l'objet gonfle et dégonfle.
- `transformOrigin` au centre à l'impact = l'objet traverse le sol.

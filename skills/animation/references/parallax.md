# Parallax Scrolling · `/animation parallax`

> Famille : Tableaux de bord & UI · #70

## C'est quoi
Des couches qui bougent à des vitesses différentes selon leur profondeur : le fond lentement, le premier plan vite. Crée une illusion de 3D.

## Quand l'utiliser
- Paysages, scroll de landing page, mise en scène d'une photo découpée en calques, intros cinématiques.

## Technique clé
- N couches avec un facteur de profondeur `z` (0 = fond, 1 = premier plan). Déplacement : `translate(camX * z, camY * z)`.
- Mouvement de caméra : `camX = interpolate(frame, [0, D], [0, -400], {easing: Easing.inOut(Easing.sin)})`.
- Premier plan plus grand, légèrement flou (`blur(2px)`) ; fond plus clair/brumeux (perspective atmosphérique).
- **Photo 2.5D** : découper sujet / fond (rembg, Photoshop), remplir le trou du fond, puis animer les deux calques à vitesses différentes + léger zoom.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame, useVideoConfig} from 'remotion';

const LAYERS = [
  {z: 0.1, color: '#F9C6A8', y: 900, h: 260},
  {z: 0.3, color: '#E8907A', y: 1050, h: 320},
  {z: 0.6, color: '#B5546B', y: 1250, h: 360},
  {z: 1.0, color: '#4B2142', y: 1480, h: 440},
];

const hills = (y: number, h: number, seed: number) => {
  let d = `M -600 1920 L -600 ${y}`;
  for (let x = -600; x <= 2400; x += 300) d += ` Q ${x + 150} ${y - h * (0.4 + 0.6 * Math.abs(Math.sin(x * 0.01 + seed)))} ${x + 300} ${y}`;
  return d + ' L 2400 1920 Z';
};

export const Parallax: React.FC<{text?: string}> = ({text = 'PROFONDEUR'}) => {
  const frame = useCurrentFrame();
  const {durationInFrames} = useVideoConfig();
  const cam = interpolate(frame, [0, durationInFrames], [0, -600], {easing: Easing.inOut(Easing.sin)});
  return (
    <AbsoluteFill style={{background: 'linear-gradient(#FFE8D6, #FCD5B5)'}}>
      <div style={{position: 'absolute', left: 620 + cam * 0.05, top: 520, width: 260, height: 260, borderRadius: '50%', background: '#FFF4E6'}} />
      <div style={{position: 'absolute', top: 320, width: '100%', textAlign: 'center', fontFamily: 'Inter', fontWeight: 900, fontSize: 110, color: '#4B2142',
        transform: `translateX(${cam * 0.2}px)`, letterSpacing: 10}}>{text}</div>
      <svg width={1080} height={1920} style={{position: 'absolute', inset: 0}}>
        {LAYERS.map((l, i) => <path key={i} d={hills(l.y, l.h, i * 2)} fill={l.color} transform={`translate(${cam * l.z} 0)`} />)}
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Parallaxe verticale (scroll de page) : `translateY(scroll * z)`.
- Plongée en profondeur : couches qui grossissent (`scale(1 + t * z)`) et sortent de l'écran.

## Pièges
- Couches trop étroites : les bords apparaissent quand elles bougent. Prévoir de la marge (largeur > écran + amplitude × z).
- Trop de couches identiques = pas de lecture de profondeur. Varier couleur et netteté.

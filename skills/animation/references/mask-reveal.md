# Mask Reveal · `/animation mask-reveal`

> Famille : Typographie cinétique · #15

## C'est quoi
Le texte est révélé à travers une forme qui grandit, ou le texte sert lui-même de fenêtre sur une image / vidéo.

## Quand l'utiliser
- Titre géant rempli d'une vidéo (« PARIS » rempli d'images de Paris).
- Reveal élégant d'un logo ou d'un mot.

## Technique clé
- **Texte comme masque** : `background: url(img)` + `WebkitBackgroundClip: 'text'` + `color: transparent`. Animer `backgroundPosition` / `backgroundSize` pour un Ken Burns dans les lettres.
- **Forme qui révèle** : `clipPath: circle(r% at 50% 50%)` avec `r` 0 → 75.
- **Ligne qui balaie** : `clipPath: inset(0 ${100-p}% 0 0)`.
- Pour une vidéo dans le texte : SVG `<mask>` avec `<text fill="white">` appliqué à un `<foreignObject>` ou CSS `mask-image`.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

export const MaskReveal: React.FC<{text?: string; image?: string}> = ({
  text = 'NEON', image = 'https://images.unsplash.com/photo-1550684376-efcbd6e3f031?w=1200',
}) => {
  const frame = useCurrentFrame();
  const r = interpolate(frame, [0, 25], [0, 80], {extrapolateRight: 'clamp', easing: Easing.out(Easing.exp)});
  const zoom = interpolate(frame, [0, 120], [140, 110]);
  return (
    <AbsoluteFill style={{background: '#000', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{
        clipPath: `circle(${r}% at 50% 50%)`,
        fontFamily: 'Impact, "Anton", sans-serif', fontSize: 340, lineHeight: 1,
        backgroundImage: `url(${image})`, backgroundSize: `${zoom}%`, backgroundPosition: 'center',
        WebkitBackgroundClip: 'text', color: 'transparent',
      }}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Zoom à travers la lettre : agrandir le texte (scale 1 → 30) jusqu'à ce que l'image remplisse l'écran (transition).
- Révélation par bandes diagonales (`polygon` animé).
- Remplissage vidéo : `<OffthreadVideo>` + `maskImage` sur un SVG de texte.

## Pièges
- `background-clip: text` exige une police très grasse sinon l'image ne se voit pas.
- En rendu, les images distantes doivent être chargées : utiliser `<Img>`/`staticFile` ou précharger.

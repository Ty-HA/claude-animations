# Duotone · `/animation duotone`

> Famille : Esthétiques artistiques · #64

## C'est quoi
Une photo ou vidéo réduite à **deux couleurs** contrastées (ombres d'une couleur, lumières d'une autre), rendue célèbre par Spotify.

## Quand l'utiliser
- Uniformiser des images disparates, affiches, covers, portraits de marque.

## Technique clé
- Filtre SVG : passer en niveaux de gris puis mapper avec `feComponentTransfer` / `feFuncR|G|B type="table"` : `tableValues="ombreR lumièreR"` (valeurs 0–1).
- Ex. ombres `#1E0B5A` (0.118 0.043 0.353), lumières `#00F5D4` (0 0.96 0.83).
- Animation : interpoler les deux couleurs dans le temps (changement de palette sur le beat), ou Ken Burns sur l'image.
- Contraste : `feComponentTransfer` linéaire `slope 1.3` avant le mapping.

## Snippet Remotion
```tsx
import {AbsoluteFill, Img, interpolate, interpolateColors, useCurrentFrame} from 'remotion';

const hex = (c: string) => c.match(/\w\w/g)!.map((h) => (parseInt(h, 16) / 255).toFixed(3));
const rgbToHex = (c: string) => '#' + c.match(/\d+/g)!.slice(0, 3).map((n) => (+n).toString(16).padStart(2, '0')).join('');

export const Duotone: React.FC<{text?: string; src?: string}> = ({src = 'https://images.unsplash.com/photo-1494790108377-be9c29b29330?w=1080'}) => {
  const frame = useCurrentFrame();
  const dark = rgbToHex(interpolateColors(frame, [0, 60], ['#1E0B5A', '#3A0CA3']));
  const light = rgbToHex(interpolateColors(frame, [0, 60], ['#00F5D4', '#F72585']));
  const [dr, dg, db] = hex(dark), [lr, lg, lb] = hex(light);
  return (
    <AbsoluteFill>
      <svg width={0} height={0}>
        <filter id="duo" colorInterpolationFilters="sRGB">
          <feColorMatrix type="saturate" values="0" />
          <feComponentTransfer>
            <feFuncR type="table" tableValues={`${dr} ${lr}`} />
            <feFuncG type="table" tableValues={`${dg} ${lg}`} />
            <feFuncB type="table" tableValues={`${db} ${lb}`} />
          </feComponentTransfer>
        </filter>
      </svg>
      <Img src={src} style={{width: '100%', height: '100%', objectFit: 'cover', filter: 'url(#duo)',
        transform: `scale(${interpolate(frame, [0, 150], [1.1, 1.25])})`}} />
    </AbsoluteFill>
  );
};
```

## Variations
- Tritone : `tableValues` à 3 valeurs (ombres, tons moyens, lumières).
- Duotone sur vidéo (`<OffthreadVideo>` + même filtre).
- Texte en couleur de lumière sur la zone d'ombre.

## Pièges
- `colorInterpolationFilters="sRGB"` indispensable, sinon couleurs fausses.
- Images peu contrastées = duotone boueux : augmenter le contraste avant.

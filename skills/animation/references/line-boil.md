# Line Boil · `/animation line-boil`

> Famille : Esthétiques artistiques · #58

## C'est quoi
Les contours « tremblent » légèrement en permanence comme dans un dessin animé fait à la main : chaque image est redessinée, donc les traits vibrent.

## Quand l'utiliser
- Donner du charme artisanal à une illustration vectorielle, doodles, explainers sympas.

## Technique clé
- Filtre SVG : `feTurbulence` (baseFrequency 0.02–0.04, numOctaves 2) + `feDisplacementMap` (scale 3–6), avec un **seed qui change toutes les 3–4 frames** et ne prend que 3 valeurs (boucle de 3 poses, comme un vrai boil).
- `seed = [1, 2, 3][Math.floor(frame / 4) % 3]`.
- Traits noirs ronds (`strokeLinecap: round`) 6–10 px, remplissages pastel légèrement décalés du contour.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

export const LineBoil: React.FC<{text?: string}> = ({text = 'coucou !'}) => {
  const frame = useCurrentFrame();
  const seed = [1, 7, 13][Math.floor(frame / 4) % 3];
  return (
    <AbsoluteFill style={{background: '#FFF8E7', justifyContent: 'center', alignItems: 'center'}}>
      <svg width={900} height={1100}>
        <defs>
          <filter id="boil">
            <feTurbulence type="fractalNoise" baseFrequency={0.03} numOctaves={2} seed={seed} />
            <feDisplacementMap in="SourceGraphic" scale={6} xChannelSelector="R" yChannelSelector="G" />
          </filter>
        </defs>
        <g filter="url(#boil)" stroke="#1d1d1d" strokeWidth={8} strokeLinecap="round" strokeLinejoin="round">
          <circle cx={450} cy={420} r={230} fill="#FFD6A5" />
          <circle cx={370} cy={390} r={22} fill="#1d1d1d" />
          <circle cx={530} cy={390} r={22} fill="#1d1d1d" />
          <path d="M360 500 Q 450 580 540 500" fill="none" />
          <path d="M300 160 Q 330 60 380 150 Q 420 50 460 150 Q 510 60 540 160" fill="none" />
          <text x={450} y={860} textAnchor="middle" fontFamily='"Comic Neue", "Comic Sans MS"' fontWeight={700} fontSize={110} fill="#FF6B6B" strokeWidth={4}>{text}</text>
        </g>
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Appliquer le filtre à toute une scène vectorielle (même un logo) pour un effet « dessiné ».
- Boil plus fort (scale 10) pour un style punk/zine.

## Pièges
- Seed qui change à chaque frame = tremblote nerveuse. 3 poses en boucle, 4 frames chacune.
- Scale trop élevé = le texte devient illisible.

# Abstract Chrome / Liquid Metal · `/animation liquid-chrome`

> Famille : Styles 3D · #36

## C'est quoi
Des formes en métal liquide ou en chrome miroir, qui ondulent comme du mercure en reflétant un environnement.

## Quand l'utiliser
- Visuels premium, mode, musique, Y2K, logos chromés.

## Technique clé
- **Chrome 2D** : texte/forme remplis d'un dégradé à fortes bandes contrastées `linear-gradient(180deg, #fff 0%, #9aa 30%, #111 48%, #eef 52%, #777 75%, #fff 100%)` dont la position glisse (`backgroundPosition` animé) → reflet qui bouge.
- Ondulation : `feTurbulence` (baseFrequency 0.01) + `feDisplacementMap` scale 20–40 avec `seed` fixe et `baseFrequency` qui varie doucement.
- Relief : `feSpecularLighting` sur l'alpha flouté (surfaceScale 6, specularExponent 30) composé par-dessus.
- **3D** : `@remotion/three` + `MeshStandardMaterial({metalness: 1, roughness: 0.05})` + `Environment` HDRI + géométrie déformée par bruit.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

export const LiquidChrome: React.FC<{text?: string}> = ({text = 'CHROME'}) => {
  const frame = useCurrentFrame();
  const t = frame / 30;
  return (
    <AbsoluteFill style={{background: 'radial-gradient(circle, #2a2a35, #08080c 70%)', justifyContent: 'center', alignItems: 'center'}}>
      <svg width={1000} height={500}>
        <defs>
          <linearGradient id="chrome" x1="0" y1="0" x2="0" y2="1" gradientTransform={`translate(0 ${0.25 * Math.sin(t)})`}>
            <stop offset="0" stopColor="#ffffff" /><stop offset="0.3" stopColor="#9aa4b0" /><stop offset="0.48" stopColor="#111318" />
            <stop offset="0.52" stopColor="#e8eef8" /><stop offset="0.75" stopColor="#6b7280" /><stop offset="1" stopColor="#ffffff" />
          </linearGradient>
          <filter id="liquid">
            <feTurbulence type="fractalNoise" baseFrequency={0.008 + 0.003 * Math.sin(t * 0.7)} numOctaves={2} seed={3} result="n" />
            <feDisplacementMap in="SourceGraphic" in2="n" scale={30} xChannelSelector="R" yChannelSelector="G" />
          </filter>
        </defs>
        <text x={500} y={320} textAnchor="middle" fontFamily="Impact, sans-serif" fontSize={230} fill="url(#chrome)" filter="url(#liquid)"
          stroke="#fff" strokeOpacity={0.4} strokeWidth={2}>{text}</text>
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Blob chrome 3D qui tourne (sphère déformée par bruit dans `@remotion/three`).
- Chrome irisé : ajouter des stops roses/cyan (voir `holo-foil`).
- Gouttes de mercure qui fusionnent (combiner avec `metaballs`).

## Pièges
- Chrome = reflets : sans contraste noir/blanc fort, ça devient du gris plat.
- `feDisplacementMap` déforme aussi les bords : prévoir une marge dans le SVG.

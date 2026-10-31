# Smoke & Fire Shader · `/animation smoke-fire`

> Famille : Mouvements organiques · #28

## C'est quoi
De la fumée ou des flammes générées en temps réel : volutes qui montent, langues de feu qui dansent.

## Quand l'utiliser
- Titre « en feu », ambiance dramatique, gaming, promo « hot deal ».

## Technique clé
- **SVG (léger)** : texte ou forme + `feTurbulence` (`baseFrequency="0.02 0.06"`, plus haute en vertical) + `feDisplacementMap` scale 30. Changer le `seed` à chaque frame pour un feu nerveux, ou animer `baseFrequency` pour un feu plus doux.
- Couleurs du feu : dégradé vertical `#FFF3B0 → #FFB000 → #FF4D00 → transparent`.
- **Fumée** : grosses particules floues (`blur(30px)`, gris 20 % d'opacité) qui montent, grandissent (`scale 0.5 → 2.5`) et s'estompent.
- **Shader** : fbm (fractal Brownian motion) + gradient vertical, coordonnée `uv.y - time` pour la montée.

## Snippet Remotion (fumée particulaire + flammes SVG)
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

export const SmokeFire: React.FC<{text?: string}> = ({text = 'HOT'}) => {
  const frame = useCurrentFrame();
  return (
    <AbsoluteFill style={{background: '#0a0503', justifyContent: 'center', alignItems: 'center'}}>
      {Array.from({length: 40}, (_, i) => {
        const life = ((frame + random(`o${i}`) * 90) % 90) / 90;
        return <div key={i} style={{position: 'absolute', width: 160, height: 160, borderRadius: '50%', background: '#888',
          left: 540 - 80 + (random(`x${i}`) - 0.5) * 300 + Math.sin(life * 6 + i) * 40, top: 900 - life * 700,
          opacity: 0.18 * (1 - life), transform: `scale(${0.5 + life * 2})`, filter: 'blur(30px)'}} />;
      })}
      <svg width={900} height={600} style={{position: 'absolute', top: 760}}>
        <defs>
          <filter id="fire">
            <feTurbulence type="fractalNoise" baseFrequency="0.02 0.07" numOctaves={3} seed={frame} result="n" />
            <feDisplacementMap in="SourceGraphic" in2="n" scale={28} xChannelSelector="R" yChannelSelector="G" />
          </filter>
          <linearGradient id="fg" x1="0" y1="1" x2="0" y2="0">
            <stop offset="0" stopColor="#FFF3B0" /><stop offset="0.4" stopColor="#FFB000" /><stop offset="0.8" stopColor="#FF4D00" />
          </linearGradient>
        </defs>
        <text x={450} y={400} textAnchor="middle" fontFamily="Impact" fontSize={300} fill="url(#fg)"
          style={{filter: 'url(#fire) drop-shadow(0 0 30px #FF4D00)'}}>{text}</text>
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Braises : petites particules orange qui montent vite avec scintillement.
- Shader fbm plein écran (`@remotion/three`) pour un feu réaliste.

## Pièges
- `seed={frame}` donne un feu « nerveux » (change à chaque frame), c'est voulu ici ; pour plus de douceur, animer `baseFrequency` à la place.
- La fumée floue est coûteuse : limiter à ~40 éléments.

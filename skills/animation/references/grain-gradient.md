# Grain & Noise Gradient · `/animation grain-gradient`

> Famille : Mouvements organiques · #29

## C'est quoi
Un dégradé de couleurs riche avec un grain visible façon affiche imprimée ou photo argentique : le look « poster moderne » très tendance.

## Quand l'utiliser
- Fonds de citations, covers de podcast, posts typographiques, ambiances mode.

## Technique clé
- Base : 2–3 `radial-gradient` superposés qui se déplacent lentement.
- Grain : SVG `feTurbulence type="fractalNoise" baseFrequency="0.85" numOctaves="3"` + `feColorMatrix type="saturate" values="0"`, plaqué en `mix-blend-mode: overlay` à 35–50 % d'opacité.
- **Grain animé** : changer le `seed` toutes les 2 frames (`seed={Math.floor(frame/2)}`) pour l'effet pellicule.
- Palette chaude : `#FF5F2E`, `#FFB4A2`, `#3D1DFF`, ou BTHA : `#00B5C7`, `#A855F7`, `#EC4899`.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

export const GrainGradient: React.FC<{text?: string}> = ({text = 'Moins de bruit.\nPlus de grain.'}) => {
  const frame = useCurrentFrame();
  const t = frame / 30;
  const bg = `radial-gradient(circle at ${30 + 20 * Math.sin(t * 0.4)}% ${30 + 15 * Math.cos(t * 0.3)}%, #FF5F2E, transparent 55%),
    radial-gradient(circle at ${70 + 15 * Math.cos(t * 0.5)}% ${70 + 10 * Math.sin(t * 0.35)}%, #3D1DFF, transparent 60%), #FFB4A2`;
  return (
    <AbsoluteFill style={{background: bg, justifyContent: 'center', padding: 140}}>
      <svg width={1080} height={1920} style={{position: 'absolute', inset: 0, mixBlendMode: 'overlay', opacity: 0.45}}>
        <filter id="grain">
          <feTurbulence type="fractalNoise" baseFrequency="0.85" numOctaves={3} seed={Math.floor(frame / 2)} />
          <feColorMatrix type="saturate" values="0" />
        </filter>
        <rect width="100%" height="100%" filter="url(#grain)" />
      </svg>
      <div style={{position: 'relative', whiteSpace: 'pre-line', fontFamily: 'Georgia, serif', fontSize: 100, lineHeight: 1.05, color: '#1a0f0a'}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Grain fixe (seed constant) pour un rendu plus calme.
- Dégradé « mesh » plus net + grain fort (`opacity 0.6`) pour un look risographe.

## Pièges
- La compression TikTok écrase le grain fin : `baseFrequency` 0.6–0.9 et opacité assez forte.
- Grain animé + compression = fichier lourd. Rendre en `crf` 18–20.

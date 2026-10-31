# Card Flip 3D · `/animation card-flip`

> Famille : Styles 3D · #39

## C'est quoi
Des cartes qui se retournent en perspective pour révéler leur face cachée, une par une ou en cascade.

## Quand l'utiliser
- Avant/après, quiz (question → réponse), révélation de résultats, collection de cartes.

## Technique clé
- Conteneur : `perspective: 1400px`. Carte : `transformStyle: 'preserve-3d'`, `rotateY(angle)`.
- Deux faces en `position: absolute`, `backfaceVisibility: 'hidden'` ; la face arrière a `rotateY(180deg)`.
- Angle : `spring({damping: 14, stiffness: 120})` de 0 → 180°, décalé de 6 frames par carte (cascade).
- Lever pendant le flip : `translateZ(80 * sin(π * p))` et ombre plus diffuse au milieu de la rotation.

## Snippet Remotion
```tsx
import {AbsoluteFill, spring, useCurrentFrame, useVideoConfig} from 'remotion';

const Card: React.FC<{front: string; back: string; delay: number}> = ({front, back, delay}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const p = spring({frame: frame - delay, fps, config: {damping: 14, stiffness: 120}});
  const face: React.CSSProperties = {position: 'absolute', inset: 0, borderRadius: 28, backfaceVisibility: 'hidden',
    display: 'flex', justifyContent: 'center', alignItems: 'center', fontFamily: 'Inter, sans-serif', fontWeight: 800, fontSize: 64};
  return (
    <div style={{width: 180, height: 260, perspective: 1400}}>
      <div style={{position: 'relative', width: '100%', height: '100%', transformStyle: 'preserve-3d',
        transform: `translateZ(${80 * Math.sin(Math.PI * p)}px) rotateY(${180 * p}deg)`}}>
        <div style={{...face, background: 'linear-gradient(135deg,#155E75,#00B5C7)', color: '#fff'}}>{front}</div>
        <div style={{...face, background: '#fff', color: '#A855F7', transform: 'rotateY(180deg)', boxShadow: '0 20px 50px rgba(0,0,0,.4)'}}>{back}</div>
      </div>
    </div>
  );
};

export const CardFlip: React.FC<{text?: string}> = ({text = 'WOW'}) => (
  <AbsoluteFill style={{background: '#010012', justifyContent: 'center', alignItems: 'center', flexDirection: 'row', gap: 24}}>
    {['?', '?', '?', '?'].map((f, i) => <Card key={i} front={f} back={text.split('')[i] ?? '★'} delay={10 + i * 6} />)}
  </AbsoluteFill>
);
```

## Variations
- Flip vertical (`rotateX`) façon calendrier.
- Grille de 9 cartes qui se retournent en vague diagonale (`delay = (row + col) * 4`).
- Carte holographique au verso (voir `holo-foil`).

## Pièges
- Sans `preserve-3d` sur le parent direct, les deux faces se superposent à plat.
- `backfaceVisibility` doit être sur chaque face, pas sur la carte.

# Clay 3D (Argile) · `/animation clay-3d`

> Famille : Styles 3D · #30

## C'est quoi
Des objets qui semblent en pâte à modeler : formes rondes, mates, douces, qui rebondissent avec écrasement (squash) comme en stop-motion.

## Quand l'utiliser
- Explainer ludique, icônes 3D mignonnes, marques jeunes/fun.

## Technique clé
- **Sans moteur 3D** : formes CSS avec `border-radius: 50%`, dégradé radial (`radial-gradient(circle at 35% 30%, clair, couleur 55%, sombre)`), ombres internes `inset -20px -30px 40px rgba(0,0,0,.25), inset 20px 20px 30px rgba(255,255,255,.4)` = rendu argile.
- Rebond : `y = |sin(t*π)| * h` ; squash à l'impact : `scaleX(1.25) scaleY(0.75)` quand y ≈ 0.
- **Stop-motion** : quantifier le temps à 12 i/s (`Math.floor(frame/2.5)`) + micro-variation de position ± 1 px.
- **Avec moteur 3D** (`@remotion/three`) : `MeshStandardMaterial({roughness: 0.9, metalness: 0})`, couleurs pastel, lumière douce + ombre portée.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

const Blob: React.FC<{color: string; light: string; dark: string; x: number; delay: number}> = ({color, light, dark, x, delay}) => {
  const frame = useCurrentFrame();
  const f = Math.floor(frame / 2.5) * 2.5; // 12 i/s
  const ph = ((f + delay) % 30) / 30;
  const y = Math.abs(Math.sin(ph * Math.PI)) * 380;
  const squash = y < 30 ? 1 - y / 30 : 0;
  const jitter = (random(`j${Math.floor(frame / 2.5)}${x}`) - 0.5) * 2;
  return (
    <div style={{position: 'absolute', left: x - 110 + jitter, bottom: 520 + y, width: 220, height: 220, borderRadius: '50%',
      background: `radial-gradient(circle at 35% 30%, ${light}, ${color} 55%, ${dark})`,
      boxShadow: 'inset -20px -30px 40px rgba(0,0,0,.25), inset 20px 20px 30px rgba(255,255,255,.35)',
      transform: `scale(${1 + 0.25 * squash}, ${1 - 0.25 * squash})`, transformOrigin: 'bottom'}} />
  );
};

export const Clay3D: React.FC<{text?: string}> = () => (
  <AbsoluteFill style={{background: '#F7E6D4'}}>
    <div style={{position: 'absolute', left: 0, right: 0, bottom: 0, height: 520, background: '#EBCFB2'}} />
    <Blob x={300} delay={0} color="#FF8A65" light="#FFC1A8" dark="#C75B3C" />
    <Blob x={560} delay={10} color="#7FD1B9" light="#C4F0E2" dark="#4A9D85" />
    <Blob x={800} delay={20} color="#9C8CFF" light="#D2CBFF" dark="#6A5ACD" />
  </AbsoluteFill>
);
```

## Variations
- Visage simple (2 yeux noirs + joues roses) sur les blobs = mascottes.
- Empreintes de doigts : texture bruit très léger en `overlay`.
- 3D réelle avec `@remotion/three` + `RoundedBox` de drei.

## Pièges
- Mouvement trop fluide = perte du charme stop-motion : quantifier à 12 i/s.
- Ombres trop dures = plastique, pas argile : garder des ombres larges et douces.

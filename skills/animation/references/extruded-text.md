# 3D Extruded Text · `/animation extruded-text`

> Famille : Styles 3D · #38

## C'est quoi
Un texte en volume, épais, avec faces latérales, lumière et ombre portée, qui pivote dans l'espace.

## Quand l'utiliser
- Titres « bloc », logos, annonces fortes, look rétro années 80 ou cartoon.

## Technique clé
- **Faux 3D CSS** (rapide) : empiler 20–40 `textShadow` décalés de 1 px chacun, de plus en plus sombres : `Array.from({length: 24}, (_, i) => \`${i}px ${i}px 0 hsl(h s ${40 - i}%)\`)`.
- Direction de l'extrusion qui bouge avec la rotation : décalage `(cos a, sin a)` par couche.
- Rotation : `perspective(1200px) rotateY(a) rotateX(b)` sur le texte.
- **Vraie 3D** : `@remotion/three` + `Text3D` (drei) avec une police JSON (typeface), `bevelEnabled`, `height: 0.3`.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

export const ExtrudedText: React.FC<{text?: string; depth?: number}> = ({text = 'BOOM', depth = 28}) => {
  const frame = useCurrentFrame();
  const a = Math.sin(frame / 25) * 0.6;
  const dx = Math.cos(a + 0.8), dy = Math.sin(a + 0.8);
  const shadow = Array.from({length: depth}, (_, i) => `${(dx * i).toFixed(1)}px ${(dy * i).toFixed(1)}px 0 hsl(280 70% ${45 - i}%)`)
    .concat(`${dx * depth + 10}px ${dy * depth + 20}px 30px rgba(0,0,0,.5)`).join(',');
  return (
    <AbsoluteFill style={{background: '#FFD23F', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{fontFamily: '"Arial Black", Impact, sans-serif', fontSize: 230, color: '#FF5DD8', textShadow: shadow,
        transform: `perspective(1200px) rotateY(${a * 25}deg) rotateX(${Math.cos(frame / 30) * 8}deg)`}}>
        {text}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Lettres qui tombent une par une avec rebond, chacune en 3D.
- Style rétro 80's : face chromée + extrusion dégradée magenta/cyan.
- Vraie 3D avec `Text3D` pour des rotations complètes à 360°.

## Pièges
- `textShadow` ne fait que du faux 3D : rotation au-delà de ±30° = illusion cassée.
- Plus de 40 couches = rendu lent sur long texte.

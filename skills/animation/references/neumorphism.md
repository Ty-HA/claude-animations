# Neumorphism · `/animation neumorphism`

> Famille : Styles 3D · #33

## C'est quoi
Des éléments qui semblent moulés dans le fond lui-même, uniquement par un jeu d'ombre claire et d'ombre foncée. Effet « plastique doux ».

## Quand l'utiliser
- Démo d'interface : boutons, interrupteurs, curseurs, lecteur audio.
- Look minimal, monochrome, apaisant.

## Technique clé
- Fond et éléments de **la même couleur** (ex. `#E0E5EC`).
- Relief sorti : `boxShadow: '12px 12px 24px #a3b1c6, -12px -12px 24px #ffffff'`.
- Enfoncé : `inset 8px 8px 16px #a3b1c6, inset -8px -8px 16px #ffffff`.
- Animation de clic : interpoler la distance des ombres de 12 → 0 → inset en 6 frames.
- Accent unique (icône ou texte) en couleur vive `#6C63FF`.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, useCurrentFrame} from 'remotion';

const BG = '#E0E5EC', D = '#a3b1c6', L = '#ffffff';

export const Neumorphism: React.FC<{text?: string}> = ({text = 'ON'}) => {
  const frame = useCurrentFrame();
  const press = interpolate(frame % 60, [20, 26, 32, 38], [1, -1, -1, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});
  const d = Math.abs(press) * 14;
  const shadow = press >= 0
    ? `${d}px ${d}px ${2 * d}px ${D}, -${d}px -${d}px ${2 * d}px ${L}`
    : `inset ${d}px ${d}px ${2 * d}px ${D}, inset -${d}px -${d}px ${2 * d}px ${L}`;
  const on = frame % 120 >= 26 && frame % 120 < 86;
  return (
    <AbsoluteFill style={{background: BG, justifyContent: 'center', alignItems: 'center'}}>
      <div style={{width: 320, height: 320, borderRadius: '50%', background: BG, boxShadow: shadow,
        display: 'flex', justifyContent: 'center', alignItems: 'center',
        fontFamily: 'Inter, sans-serif', fontWeight: 800, fontSize: 80, color: on ? '#6C63FF' : '#a3b1c6'}}>
        {text}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Interrupteur (toggle) : pastille en relief qui glisse dans une rainure enfoncée.
- Curseur de volume / lecteur audio complet.
- Neumorphism sombre : fond `#2a2d32`, ombres `#1d1f23` / `#373b41`.

## Pièges
- Contraste très faible = accessibilité médiocre : accent coloré obligatoire pour l'état actif.
- Fond différent de la couleur de l'élément = l'effet disparaît.

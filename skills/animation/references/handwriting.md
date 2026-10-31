# Handwriting / Signature · `/animation handwriting`

> Famille : Typographie cinétique · #10

## C'est quoi
Le texte se trace comme écrit à la main : le trait progresse le long du chemin, lettre après lettre.

## Quand l'utiliser
- Signature de fin, prénom, mot doux, titre élégant.
- Annoter une image (flèche + mot manuscrit).

## Technique clé
- Il faut un **chemin SVG** (`<path d=...>`) : exporter le texte en contours depuis Figma/Illustrator, ou dessiner une signature en monoline.
- Animation du trait : `strokeDasharray = L`, `strokeDashoffset = L * (1 - progress)` où `L` = longueur du chemin (`path.getTotalLength()` ou valeur fixe mesurée).
- Progression : `interpolate(frame, [0, 60], [0, 1], {easing: Easing.inOut(Easing.cubic)})`.
- Plusieurs chemins : les enchaîner avec un décalage cumulé proportionnel à leur longueur.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

// Exemple : une signature monoline (remplace d par ton propre tracé)
const D = 'M60 220 C 120 60, 180 60, 160 200 S 220 300, 260 160 S 330 80, 340 200 C 350 260, 400 250, 430 180 S 520 120, 540 210 C 560 280, 640 240, 700 150';
const LEN = 1400; // longueur approx. du chemin (mesurer avec getTotalLength)

export const Handwriting: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const p = interpolate(frame, [0, 60], [0, 1], {extrapolateRight: 'clamp', easing: Easing.inOut(Easing.cubic)});
  return (
    <AbsoluteFill style={{background: '#F6F1E7', justifyContent: 'center', alignItems: 'center'}}>
      <svg width={760} height={360} viewBox="0 0 760 360">
        <path d={D} fill="none" stroke="#1B2A6B" strokeWidth={9} strokeLinecap="round" strokeLinejoin="round"
          strokeDasharray={LEN} strokeDashoffset={LEN * (1 - p)} />
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Police manuscrite + masque : afficher le texte en police script et le révéler derrière un tracé épais animé (`<mask>`).
- Effet encre : légère variation `strokeWidth` et `filter: url(#rough)` avec `feTurbulence` + `feDisplacementMap` (scale 2).
- Plume qui suit le trait : `path.getPointAtLength(L * p)` pour positionner une icône.

## Pièges
- Un texte converti en contours a des formes fermées (double trait) : préférer une police **monoline** ou un tracé à la main.
- `getTotalLength` n'existe qu'au rendu DOM : mesurer une fois et coder la valeur.

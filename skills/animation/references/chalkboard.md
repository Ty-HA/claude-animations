# Chalkboard / Sketch · `/animation chalkboard`

> Famille : Esthétiques artistiques · #62

## C'est quoi
Un dessin ou un texte qui se trace à la craie sur un tableau noir (ou au crayon sur papier), avec un trait granuleux et irrégulier.

## Quand l'utiliser
- Éducation, explication pas à pas, schéma, « retour à l'école », tutoriel.

## Technique clé
- Fond tableau `#2F3B33` + texture (bruit + traces d'effacement : grandes ellipses blanches à 3 % floutées).
- Trait craie : `stroke="#F1F1E8"`, 6–8 px, `strokeLinecap: round` + filtre granuleux : `feTurbulence` (0.9) → `feComposite operator="in"` puis `feColorMatrix` alpha pour trouer le trait.
- Tracé : `pathLength={1}` + `strokeDashoffset 1 → 0`, chaque élément à la suite.
- Texte manuscrit : police « Gloria Hallelujah » / « Caveat », révélé par un masque qui balaie de gauche à droite.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, useCurrentFrame} from 'remotion';

const PATHS = ['M200 900 L 880 900', 'M540 900 L 540 500', 'M540 500 m -120 0 a 120 120 0 1 0 240 0 a 120 120 0 1 0 -240 0', 'M300 1100 Q 540 1000 780 1100'];

export const Chalkboard: React.FC<{text?: string}> = ({text = 'E = mc²'}) => {
  const frame = useCurrentFrame();
  const textP = interpolate(frame, [80, 110], [0, 100], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});
  return (
    <AbsoluteFill style={{background: 'radial-gradient(circle at 50% 40%, #3A4A40, #222b25 80%)', border: '30px solid #6B4423'}}>
      <svg width={1080} height={1920}>
        <defs>
          <filter id="chalk">
            <feTurbulence type="fractalNoise" baseFrequency={0.9} numOctaves={1} seed={2} result="n" />
            <feColorMatrix in="n" type="matrix" values="0 0 0 0 1  0 0 0 0 1  0 0 0 0 1  0 0 0 -2.2 1.6" result="holes" />
            <feComposite in="SourceGraphic" in2="holes" operator="in" />
          </filter>
        </defs>
        <g filter="url(#chalk)" stroke="#F1F1E8" strokeWidth={9} strokeLinecap="round" fill="none">
          {PATHS.map((d, i) => (
            <path key={i} d={d} pathLength={1} strokeDasharray={1}
              strokeDashoffset={interpolate(frame, [i * 18, i * 18 + 20], [1, 0], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'})} />
          ))}
        </g>
      </svg>
      <div style={{position: 'absolute', top: 1250, width: '100%', textAlign: 'center', fontFamily: '"Gloria Hallelujah", "Caveat", cursive',
        fontSize: 130, color: '#F1F1E8', filter: 'url(#chalk)', clipPath: `inset(0 ${100 - textP}% 0 0)`}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Craie de couleur (jaune `#F7E479`, rose `#F7A1C4`) pour les mots-clés.
- Main/éponge qui efface une zone (masque qui grandit) pour passer à l'étape suivante.
- Version crayon : papier blanc, trait gris graphite `#555`, hachures.

## Pièges
- Filtre trop agressif = trait invisible : ajuster les 2 derniers chiffres de la matrice (`-2.2 1.6`).
- Tracer tout en même temps : l'intérêt est la séquence.

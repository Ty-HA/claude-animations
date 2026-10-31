# Variable Font Morph · `/animation variable-font`

> Famille : Typographie cinétique · #12

## C'est quoi
Les lettres respirent : leur graisse, leur largeur ou leur inclinaison changent en continu grâce aux polices variables.

## Quand l'utiliser
- Titre vivant, logo animé, effet « battement » sur le rythme.
- Vague qui traverse un mot lettre par lettre.

## Technique clé
- Police variable avec axes `wght` (100–900), `wdth` (75–125), `slnt`. Ex. Google Fonts : **Roboto Flex**, **Inter**, **Recursive**, **Anybody**.
- `fontVariationSettings: \`'wght' ${w}, 'wdth' ${wd}\``.
- Vague : par lettre `w = 500 + 400 * Math.sin(frame * 0.15 - i * 0.6)`.
- Charger la police avec `@remotion/google-fonts/RobotoFlex` (axes inclus) pour un rendu garanti.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';
import {loadFont} from '@remotion/google-fonts/RobotoFlex';

const {fontFamily} = loadFont();

export const VariableFont: React.FC<{text?: string}> = ({text = 'RESPIRE'}) => {
  const frame = useCurrentFrame();
  return (
    <AbsoluteFill style={{background: '#F2EDE4', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{fontFamily, fontSize: 170, color: '#111', display: 'flex'}}>
        {text.split('').map((c, i) => {
          const k = Math.sin(frame * 0.15 - i * 0.6);
          return (
            <span key={i} style={{fontVariationSettings: `'wght' ${500 + 400 * k}, 'wdth' ${100 + 25 * k}`}}>{c}</span>
          );
        })}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Synchro audio : `wght` piloté par le volume (`visualizeAudio` de `@remotion/media-utils`).
- Survol interactif sur le web : `wght` selon la distance au curseur.
- Morph « fin → gras » unique sur 20 frames pour une entrée de titre.

## Pièges
- Police non variable : rien ne bouge. Vérifier que l'axe existe pour la police choisie.
- Changer la largeur fait bouger tout le mot : centrer et prévoir de la marge.

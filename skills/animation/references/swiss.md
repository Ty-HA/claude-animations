# Swiss Design · `/animation swiss`

> Famille : Esthétiques artistiques · #45

## C'est quoi
Le style typographique international suisse : grille stricte asymétrique, grandes sans-serif (Helvetica), peu de couleurs, transitions géométriques nettes.

## Quand l'utiliser
- Contenu sérieux et élégant : architecture, design, conférences, data éditoriale.

## Technique clé
- Grille 12 colonnes, marges 140 px, tout aligné à gauche sur la grille.
- Typo : Helvetica Neue / Inter, très grand titre (200+ px) en graisse 700, interlignage serré (0.9), texte courant petit.
- Couleurs : blanc, noir, **un** rouge `#E30613`.
- Animations : pas de rebond. Glissements linéaires ou `Easing.bezier(0.7, 0, 0.3, 1)`, blocs de couleur qui balaient (`scaleX 0 → 1`), lignes qui se tracent.
- Rythme : changements secs sur la grille, 10–15 frames.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

const ease = Easing.bezier(0.7, 0, 0.3, 1);

export const Swiss: React.FC<{text?: string}> = ({text = 'Form\nfollows\nfunction.'}) => {
  const frame = useCurrentFrame();
  const bar = interpolate(frame, [0, 15], [0, 1], {extrapolateRight: 'clamp', easing: ease});
  const lines = text.split('\n');
  return (
    <AbsoluteFill style={{background: '#F4F4F0', fontFamily: '"Helvetica Neue", Inter, sans-serif'}}>
      <div style={{position: 'absolute', left: 0, top: 260, width: 1080 * 0.58, height: 680, background: '#E30613', transform: `scaleX(${bar})`, transformOrigin: 'left'}} />
      {lines.map((l, i) => {
        const p = interpolate(frame, [10 + i * 6, 26 + i * 6], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp', easing: ease});
        return (
          <div key={i} style={{position: 'absolute', left: 140, top: 300 + i * 200, fontSize: 200, fontWeight: 700, lineHeight: 0.9, letterSpacing: -6,
            color: i === 2 ? '#111' : '#fff', clipPath: `inset(0 ${100 - p * 100}% 0 0)`}}>{l}</div>
        );
      })}
      <div style={{position: 'absolute', left: 140, top: 1100, width: 300, fontSize: 28, lineHeight: 1.35, color: '#111'}}>
        Grille. Hiérarchie. Clarté. Rien d'autre.
      </div>
      <div style={{position: 'absolute', left: 140, right: 140, top: 1060, height: 4, background: '#111', transform: `scaleX(${bar})`, transformOrigin: 'left'}} />
    </AbsoluteFill>
  );
};
```

## Variations
- Affiche cinétique : formes géométriques (cercle, barre) qui se repositionnent sur la grille au rythme.
- Numéros géants (01, 02…) pour structurer une liste.

## Pièges
- Centrer les textes = ce n'est plus suisse. Alignement à gauche.
- Trop d'effets (glow, rebond, dégradé) cassent la rigueur.

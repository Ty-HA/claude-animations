# Highlighter Sweep · `/animation highlighter`

> Famille : Typographie cinétique · #13

## C'est quoi
Un coup de surligneur (ou un soulignement) qui se dessine sous un mot important, comme fait à la main.

## Quand l'utiliser
- Mettre en valeur un mot dans une phrase, un chiffre, un bénéfice.
- Slides éducatives, citations, tutos.

## Technique clé
- Bloc derrière le mot (`position: absolute; z-index: -1`) avec `transform: scaleX(p)` et `transform-origin: left`.
- `p = interpolate(frame, [start, start+12], [0,1], {easing: Easing.out(Easing.cubic)})`.
- Look marqueur : hauteur 45 % du mot, collé en bas, léger `rotate(-1.5deg)`, `border-radius: 4px 12px 6px 10px`, couleur jaune `#FFE14D` ou rose à 80 %.
- Soulignement manuscrit : path SVG ondulé + `strokeDashoffset`.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

const Mark: React.FC<{children: React.ReactNode; start: number; color?: string}> = ({children, start, color = '#FFE14D'}) => {
  const frame = useCurrentFrame();
  const p = interpolate(frame, [start, start + 12], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp', easing: Easing.out(Easing.cubic)});
  return (
    <span style={{position: 'relative', display: 'inline-block', zIndex: 0}}>
      <span style={{position: 'absolute', left: -8, right: -8, bottom: '6%', height: '45%', background: color, zIndex: -1,
        transform: `scaleX(${p}) rotate(-1.5deg)`, transformOrigin: 'left', borderRadius: '4px 12px 6px 10px'}} />
      {children}
    </span>
  );
};

export const Highlighter: React.FC<{text?: string}> = ({text = 'animer'}) => (
  <AbsoluteFill style={{background: '#fff', justifyContent: 'center', padding: 140}}>
    <div style={{fontFamily: 'Georgia, serif', fontSize: 80, lineHeight: 1.3, color: '#111'}}>
      Claude sait <Mark start={10}>{text}</Mark> à peu près <Mark start={30} color="#7CF2D4">tout</Mark>.
    </div>
  </AbsoluteFill>
);
```

## Variations
- Entourage au feutre : ellipse SVG imparfaite tracée avec `strokeDashoffset`.
- Barré : trait horizontal au milieu pour « avant → après ».
- Mode sombre : surligneur néon cyan avec `mix-blend-mode: screen`.

## Pièges
- Le surlignage ne suit pas un retour à la ligne : mettre `white-space: nowrap` sur le mot.
- `z-index: -1` nécessite un parent qui crée un contexte (`zIndex: 0` sur le span).

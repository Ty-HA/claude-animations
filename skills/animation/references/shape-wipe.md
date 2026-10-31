# Shape Wipe · `/animation shape-wipe`

> Famille : Transitions & caméra · #85

## C'est quoi
Une transition où une forme (bande diagonale, chevron, losange, cercles en cascade) balaie l'écran et révèle la scène suivante.

## Quand l'utiliser
- Transitions de marque, chapitres, passage entre deux sujets avec du style.

## Technique clé
- Couche de couleur de marque qui couvre puis découvre : `clipPath: polygon(...)` animé, ou plusieurs bandes décalées.
- **Bandes diagonales** : 5 bandes `skewX(-20deg)` qui traversent de gauche à droite avec 2 frames de décalage ; la scène change quand l'écran est couvert (milieu).
- Durée totale 16–24 frames, `Easing.inOut(Easing.cubic)`.
- Couleurs : accent + accent secondaire pour un effet de couches.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

const COLORS = ['#00B5C7', '#67E8F9', '#A855F7', '#EC4899', '#010012'];

export const ShapeWipe: React.FC<{text?: string; at?: number}> = ({text = 'CHAPITRE 2', at = 20}) => {
  const frame = useCurrentFrame();
  const swapped = frame >= at + 12;
  return (
    <AbsoluteFill style={{overflow: 'hidden'}}>
      <AbsoluteFill style={{background: swapped ? '#010012' : '#F4F4F0', justifyContent: 'center', alignItems: 'center',
        fontFamily: 'Inter', fontWeight: 900, fontSize: 130, color: swapped ? '#67E8F9' : '#010012'}}>{swapped ? text : 'CHAPITRE 1'}</AbsoluteFill>
      {COLORS.map((c, i) => {
        const p = interpolate(frame, [at + i * 2, at + i * 2 + 22], [-1.6, 1.6], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp', easing: Easing.inOut(Easing.cubic)});
        return <div key={i} style={{position: 'absolute', top: -200, bottom: -200, left: '-30%', width: '160%', background: c,
          transform: `translateX(${p * 100}%) skewX(-20deg)`}} />;
      })}
    </AbsoluteFill>
  );
};
```

## Variations
- Cercle qui grandit depuis un point (voir `iris`).
- Grille de carrés qui se retournent en cascade.
- Logo géant qui traverse l'écran et sert de masque.
- `@remotion/transitions` avec `wipe()` / `slide()` pour un `TransitionSeries` propre.

## Pièges
- Changer de scène avant que l'écran soit couvert = flash de la nouvelle scène.
- Bandes trop étroites : on voit entre elles. Assez larges + superposées.

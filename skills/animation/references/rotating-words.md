# Rotating Words · `/animation rotating-words`

> Famille : Typographie cinétique · #14

## C'est quoi
Une phrase fixe dont un seul mot change en boucle, en roulant comme un rouleau : « Claude crée des **vidéos / sites / jeux** ».

## Quand l'utiliser
- Hero de landing page, intro « tout ce que X sait faire ».
- Lister des usages sans changer de plan.

## Technique clé
- Fenêtre à hauteur fixe (`overflow: hidden`, hauteur = 1 ligne) contenant la pile de mots.
- Index : `i = Math.floor(frame / hold)` ; transition : `spring` sur `translateY` de `-i*h` à `-(i+1)*h` pendant ~10 frames.
- Largeur du mot qui change : animer la largeur ou aligner la phrase à gauche.
- Mot rotatif en couleur d'accent / dégradé (`background-clip: text`).

## Snippet Remotion
```tsx
import {AbsoluteFill, spring, useCurrentFrame, useVideoConfig} from 'remotion';

export const RotatingWords: React.FC<{text?: string; words?: string[]; hold?: number}> = ({
  text = 'Claude crée des', words = ['vidéos', 'sites', 'jeux', 'animations'], hold = 30,
}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const H = 120;
  const i = Math.floor(frame / hold);
  const s = spring({frame: frame - i * hold, fps, config: {damping: 14}});
  const y = -(i - 1 + s) * H;
  const list = [...words, ...words, ...words];
  return (
    <AbsoluteFill style={{background: '#010012', justifyContent: 'center', padding: 140, fontFamily: 'Poppins, sans-serif', fontWeight: 800, fontSize: 96, color: '#fff'}}>
      <div>{text}</div>
      <div style={{height: H, overflow: 'hidden'}}>
        <div style={{transform: `translateY(${y}px)`}}>
          {list.map((w, k) => (
            <div key={k} style={{height: H, lineHeight: `${H}px`, background: 'linear-gradient(90deg,#A855F7,#EC4899)',
              WebkitBackgroundClip: 'text', color: 'transparent'}}>{w}</div>
          ))}
        </div>
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Rotation 3D (cube) : `rotateX` au lieu de `translateY`, `perspective: 600px`.
- Fondu + flou (`filter: blur`) entre les mots.
- Machine à écrire : effacer puis retaper le mot (voir `typewriter`).

## Pièges
- La liste doit être assez longue pour la durée : la dupliquer ou utiliser un modulo.
- Hauteur de ligne ≠ hauteur de fenêtre = mots coupés : fixer `lineHeight` = `H`.

# Kinetic Stacking · `/animation stacking`

> Famille : Typographie cinétique · #7

## C'est quoi
Les mots tombent et s'empilent à un rythme frénétique, chaque ligne avec une taille différente pour remplir un bloc justifié.

## Quand l'utiliser
- Manifeste, liste d'arguments, paroles de musique à fort rythme.
- Fin de vidéo : tout le message empilé en un seul bloc.

## Technique clé
- 1 mot par ligne (ou 2), une nouvelle ligne toutes les 6 à 8 frames.
- Taille calculée pour que chaque ligne fasse la même largeur : `fontSize = largeurCible / (nbLettres * 0.62)` pour une police condensée grasse.
- Entrée par le haut : `translateY(-120 → 0)` avec `spring({damping: 15})`, et léger rebond de tout le bloc (`translateY(+8 → 0)`) à chaque impact.
- Alternance blanc / accent toutes les 2–3 lignes.

## Snippet Remotion
```tsx
import {AbsoluteFill, spring, useCurrentFrame, useVideoConfig} from 'remotion';

export const Stacking: React.FC<{text?: string; every?: number}> = ({text = 'ON EMPILE LES MOTS SANS JAMAIS RALENTIR', every = 7}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const words = text.split(' ');
  return (
    <AbsoluteFill style={{background: '#0a0a0a', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{width: 800, display: 'flex', flexDirection: 'column', alignItems: 'stretch'}}>
        {words.map((w, i) => {
          const s = spring({frame: frame - i * every, fps, config: {damping: 15, stiffness: 180}});
          if (frame < i * every) return null;
          return (
            <div key={i} style={{
              fontFamily: 'Impact, "Anton", sans-serif', textTransform: 'uppercase', lineHeight: 0.92, textAlign: 'center',
              fontSize: Math.min(260, 800 / (w.length * 0.55)), color: i % 3 === 1 ? '#00B5C7' : '#fff',
              transform: `translateY(${(1 - s) * -120}px)`, opacity: s,
            }}>{w}</div>
          );
        })}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Pile qui dépasse l'écran : faire remonter le bloc entier (`translateY` négatif) au fil des lignes.
- Lignes qui s'insèrent latéralement (gauche/droite alternées).
- Synchro voix : une ligne par mot aligné.

## Pièges
- Police non condensée : les mots longs deviennent minuscules. Utiliser Anton, Bebas Neue, Oswald.
- Au-delà de ~10 lignes, le bloc déborde en 9:16 : prévoir le scroll ou découper.

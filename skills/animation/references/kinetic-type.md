# Kinetic Type (Headline) · `/animation kinetic-type`

> Famille : Typographie cinétique · #1

## C'est quoi
Un texte percutant affiché **mot par mot**, synchronisé à la voix : chaque mot claque à l'écran avec un zoom sec, puis laisse la place au suivant.

## Quand l'utiliser
- Hook des 3 premières secondes d'un TikTok / Reel.
- Citation forte, punchline, chiffre clé.

## Technique clé
- Découper le texte en mots, 1 mot = 8 à 12 frames à 30 i/s (ou caler sur l'alignement de la voix).
- Entrée : `spring({damping: 12, stiffness: 200})` sur `scale` de 1.6 → 1 + `opacity` 0 → 1 en 3 frames.
- Mots-clés en couleur d'accent (ex. `#00B5C7`), le reste en blanc. Police très grasse (900), majuscules.
- Petit « punch » de caméra : `scale` global 1 → 1.04 sur les mots accentués.

## Snippet Remotion
```tsx
import {AbsoluteFill, spring, useCurrentFrame, useVideoConfig} from 'remotion';

export const KineticType: React.FC<{text?: string; perWord?: number}> = ({text = 'Claude anime TOUT ce que tu veux', perWord = 10}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const words = text.split(' ');
  const i = Math.min(words.length - 1, Math.floor(frame / perWord));
  const local = frame - i * perWord;
  const s = spring({frame: local, fps, config: {damping: 12, stiffness: 200}});
  const accent = words[i] === words[i].toUpperCase();
  return (
    <AbsoluteFill style={{background: '#010012', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{
        fontFamily: 'Inter, sans-serif', fontWeight: 900, fontSize: 150, textTransform: 'uppercase',
        color: accent ? '#00B5C7' : '#fff', transform: `scale(${1.6 - 0.6 * s})`, opacity: Math.min(1, (local + 1) / 3),
        textShadow: accent ? '0 0 30px rgba(0,181,199,.6)' : 'none',
      }}>
        {words[i]}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- **Empilé** : garder les mots précédents en petit au-dessus (voir `stacking`).
- **Shake** : ajouter 2 frames de `translateX(±6px)` à l'impact.
- **Synchro voix** : remplacer `perWord` par les timings d'un JSON d'alignement (start/end par mot).

## Pièges
- Trop de mots par seconde = illisible : max ~3 mots/s.
- Mots longs : réduire `fontSize` selon la longueur (`Math.min(150, 1400 / mot.length)`).
- En 9:16, garder 140 px de marge latérale pour l'interface TikTok.

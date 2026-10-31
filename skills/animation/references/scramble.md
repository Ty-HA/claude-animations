# Scramble / Text Reveal · `/animation scramble`

> Famille : Typographie cinétique · #6

## C'est quoi
Des lettres aléatoires défilent à toute vitesse, comme un code en cours de décryptage, puis se figent une à une sur le vrai mot.

## Quand l'utiliser
- Révéler un nom, un mot-clé, un mot de passe, un titre tech.
- Remplacer un simple fondu par quelque chose de plus « IA ».

## Technique clé
- Charset : `'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789#$%&@!?'`.
- Chaque lettre `i` se fige à la frame `start + i * 2` (révélation de gauche à droite).
- Avant, caractère aléatoire re-tiré toutes les 2 frames : `random(\`${i}-${Math.floor(frame/2)}\`)`.
- Lettres non résolues en couleur d'accent atténuée (`#00B5C7` à 60 %), lettres résolues en blanc.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

const CHARS = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789#$%&@!?';

export const Scramble: React.FC<{text?: string; start?: number; stagger?: number}> = ({text = 'DÉCRYPTÉ', start = 8, stagger = 3}) => {
  const frame = useCurrentFrame();
  return (
    <AbsoluteFill style={{background: '#010012', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{fontFamily: 'monospace', fontWeight: 800, fontSize: 130, letterSpacing: 6}}>
        {text.split('').map((c, i) => {
          const done = frame >= start + i * stagger || c === ' ';
          const r = CHARS[Math.floor(random(`${i}-${Math.floor(frame / 2)}`) * CHARS.length)];
          return (
            <span key={i} style={{color: done ? '#fff' : 'rgba(0,181,199,.65)'}}>{done ? c : r}</span>
          );
        })}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Révélation aléatoire (ordre tiré au sort) plutôt que gauche → droite.
- Scramble entre deux mots (« AVANT » → « APRÈS ») : partir des lettres du premier mot.
- Petit flash `textShadow` cyan sur la lettre au moment où elle se fige.

## Pièges
- Police non monospace : la largeur saute à chaque frame. Toujours monospace (ou largeur fixe par `span`).
- Trop lent : au-delà de ~1,5 s le spectateur décroche.

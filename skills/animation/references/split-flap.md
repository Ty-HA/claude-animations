# Split-Flap Display · `/animation split-flap`

> Famille : Typographie cinétique · #2

## C'est quoi
Le panneau d'affichage des gares et aéroports : chaque case fait défiler des caractères en claquant avant de se fixer sur la bonne lettre.

## Quand l'utiliser
- Annonce (« DÉPART : LUNDI »), reveal d'un nom, d'une destination, d'un prix.
- Ambiance rétro, voyage, compte à rebours.

## Technique clé
- Alphabet de défilement : `' ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789'`.
- Chaque case démarre avec un décalage de 2 frames et fait 8 à 20 tours avant de se stabiliser.
- Un tour = 2 frames. Pendant le flip, la moitié haute se replie : `rotateX(0 → -90deg)` avec `perspective: 400px`.
- Case : fond `#1b1b1f`, ligne médiane noire 2 px, coins arrondis 8 px, police mono grasse jaune `#FFD23F` ou blanche.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

const CHARS = ' ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';

const Flap: React.FC<{target: string; delay: number; seed: number}> = ({target, delay, seed}) => {
  const frame = useCurrentFrame();
  const spins = 8 + Math.floor(random(`f${seed}`) * 12);
  const step = Math.max(0, Math.floor((frame - delay) / 2));
  const done = step >= spins;
  const ch = done ? target : CHARS[(CHARS.indexOf(target) - spins + step + CHARS.length * 4) % CHARS.length];
  const flip = done ? 0 : ((frame - delay) % 2) / 2;
  return (
    <div style={{position: 'relative', width: 90, height: 130, background: '#1b1b1f', borderRadius: 8, perspective: 400,
      display: 'flex', alignItems: 'center', justifyContent: 'center', fontFamily: 'monospace', fontWeight: 800, fontSize: 96, color: '#FFD23F'}}>
      <span style={{transform: `rotateX(${-flip * 90}deg)`}}>{ch}</span>
      <div style={{position: 'absolute', left: 0, right: 0, top: 64, height: 2, background: '#000'}} />
    </div>
  );
};

export const SplitFlap: React.FC<{text?: string}> = ({text = 'DEPART 07H'}) => (
  <AbsoluteFill style={{background: '#0b0b0e', justifyContent: 'center', alignItems: 'center', gap: 8, flexDirection: 'row', flexWrap: 'wrap'}}>
    {text.toUpperCase().split('').map((c, i) => <Flap key={i} target={CHARS.includes(c) ? c : ' '} delay={i * 2} seed={i} />)}
  </AbsoluteFill>
);
```

## Variations
- Plusieurs lignes (tableau de départs complet) avec un décalage par ligne de 6 frames.
- Son de « clac » à chaque step (un tick audio court par tour).
- Version moderne : fond noir, chiffres blancs, accent cyan sur la case finale.

## Pièges
- Lettres accentuées absentes de l'alphabet : les normaliser (`É` → `E`) ou les ajouter à `CHARS`.
- Trop de cases sur 1080 px : max ~10 cases de 90 px par ligne.

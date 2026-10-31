# Karaoke Captions · `/animation karaoke`

> Famille : Typographie cinétique · #9

## C'est quoi
Les sous-titres façon TikTok : 2 à 4 mots affichés à la fois, le mot en train d'être prononcé s'allume (couleur, fond, zoom).

## Quand l'utiliser
- Toute vidéo avec voix off ou face cam : +accessibilité, +rétention.

## Technique clé
- Il faut les **timings par mot** : alignement forcé (ElevenLabs `/v1/forced-alignment`, Whisper avec `word_timestamps`) → JSON `[{text, start, end}]`.
- Regrouper par « chunks » de 3 mots (couper aussi sur la ponctuation).
- Mot actif : `frame/fps` entre `start` et `end` → couleur `#FFE14D` ou fond pastille, `scale(1.12)`.
- Contour lisible : `WebkitTextStroke: 8px black` + `paintOrder: 'stroke fill'`.
- Position : entre 60 % et 72 % de la hauteur (au-dessus de l'interface TikTok).

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame, useVideoConfig} from 'remotion';

type Word = {text: string; start: number; end: number};

const DEMO: Word[] = "Commente ANIMATION et je t'envoie la liste".split(' ').map((t, i) => ({text: t, start: i * 0.35, end: i * 0.35 + 0.33}));

export const Karaoke: React.FC<{words?: Word[]; perChunk?: number}> = ({words = DEMO, perChunk = 3}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const t = frame / fps;
  const chunks: Word[][] = [];
  words.forEach((w, i) => (i % perChunk === 0 ? chunks.push([w]) : chunks[chunks.length - 1].push(w)));
  const chunk = chunks.find((c) => t >= c[0].start && t < (chunks[chunks.indexOf(c) + 1]?.[0].start ?? Infinity));
  return (
    <AbsoluteFill style={{background: '#222', alignItems: 'center'}}>
      <div style={{position: 'absolute', top: '62%', left: 140, right: 140, textAlign: 'center',
        fontFamily: 'Montserrat, sans-serif', fontWeight: 900, fontSize: 84, textTransform: 'uppercase'}}>
        {chunk?.map((w) => {
          const on = t >= w.start && t < w.end;
          return (
            <span key={w.start} style={{display: 'inline-block', margin: '0 10px', color: on ? '#FFE14D' : '#fff',
              transform: `scale(${on ? 1.12 : 1})`, WebkitTextStroke: '8px #000', paintOrder: 'stroke fill'}}>{w.text}</span>
          );
        })}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Pastille : fond arrondi coloré derrière le mot actif au lieu de changer la couleur du texte.
- Emoji automatique après certains mots-clés.
- Un seul mot à la fois (style « MrBeast »).

## Pièges
- Comparaison de mots : attention à la casse et aux apostrophes si tu cherches des mots dans le JSON.
- Ne jamais deviner les timings à la main sur une vraie voix : toujours un alignement.

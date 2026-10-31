# Lip-Sync Avatar · `/animation lip-sync`

> Famille : Personnages & fun · #94

## C'est quoi
Un avatar (dessiné ou vectoriel) dont la bouche bouge en rythme avec une voix off.

## Quand l'utiliser
- Vidéos faceless avec mascotte, explainer avec narrateur, avatar de marque.

## Technique clé
- **Simple (amplitude)** : `@remotion/media-utils` → `useAudioData(src)` + `visualizeAudio({fps, frame, audioData, numberOfSamples: 16})` → volume moyen → ouverture de bouche 0–1. Lisser : moyenne sur 3 frames.
- **Mieux (visèmes)** : timings par mot/phonème (alignement forcé, Rhubarb Lip Sync) → formes de bouche A (fermée), E (ouverte), O (ronde), M (lèvres serrées)… → afficher la forme correspondante.
- Bouche : un `path` dont la hauteur dépend de l'ouverture, ou 4–6 sprites PNG échangés.
- Vie : clignements toutes les ~3 s (2 frames fermé), léger balancement de la tête.

## Snippet Remotion
```tsx
import {AbsoluteFill, Audio, staticFile, useCurrentFrame, useVideoConfig} from 'remotion';
import {useAudioData, visualizeAudio} from '@remotion/media-utils';

export const LipSync: React.FC<{text?: string; src?: string}> = ({src = staticFile('voix.mp3')}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const audio = useAudioData(src);
  const level = (f: number) => {
    if (!audio || f < 0) return 0;
    const v = visualizeAudio({fps, frame: f, audioData: audio, numberOfSamples: 16});
    return v.slice(0, 6).reduce((a, b) => a + b, 0) / 6;
  };
  const open = Math.min(1, ((level(frame) + level(frame - 1) + level(frame - 2)) / 3) * 6);
  const blink = frame % 95 > 92;
  return (
    <AbsoluteFill style={{background: '#0b1020', justifyContent: 'center', alignItems: 'center'}}>
      <Audio src={src} />
      <svg width={600} height={600} viewBox="0 0 600 600" style={{transform: `rotate(${Math.sin(frame / 20) * 2}deg)`}}>
        <circle cx={300} cy={300} r={240} fill="#FFD6A5" />
        {[220, 380].map((x) => (blink
          ? <line key={x} x1={x - 25} x2={x + 25} y1={260} y2={260} stroke="#222" strokeWidth={8} strokeLinecap="round" />
          : <ellipse key={x} cx={x} cy={260} rx={22} ry={30} fill="#222" />))}
        <ellipse cx={300} cy={400} rx={60 - open * 15} ry={6 + open * 45} fill="#7A2B35" />
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Sprites de bouche dessinés (PNG transparents) choisis selon `open` (3 niveaux).
- Visèmes précis via Rhubarb (export JSON) pour un rendu dessin animé.
- Sans lip-sync : poses fixes qui changent sur les phrases (moins cher, souvent suffisant).

## Pièges
- `useAudioData` est asynchrone : gérer `null` au début (bouche fermée).
- Bouche qui bouge sur les silences/respirations : seuil minimal (ex. `open < 0.15 → 0`).

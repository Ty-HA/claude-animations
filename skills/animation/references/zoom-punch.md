# Zoom Punch · `/animation zoom-punch`

> Famille : Transitions & caméra · #83

## C'est quoi
Un zoom brutal et bref sur le temps fort (un mot, un beat, une réaction) puis retour, comme un coup de poing visuel.

## Quand l'utiliser
- Mettre l'accent sur une phrase-clé, synchro sur un drop musical, réaction drôle.

## Technique clé
- Impact en 2–3 frames : `scale 1 → 1.25`, puis retour en 8–12 frames avec un spring (`damping: 12`) → petit rebond.
- Centre du zoom sur le sujet (`transformOrigin` sur le visage / le mot).
- Accompagner de : flash blanc 1 frame à 30 %, légère rotation (±2°), aberration chromatique 2 frames.
- Calé sur l'audio : liste de frames d'impact (beats) → pour chaque frame, prendre le dernier impact passé.

## Snippet Remotion
```tsx
import {AbsoluteFill, spring, useCurrentFrame, useVideoConfig} from 'remotion';

export const ZoomPunch: React.FC<{text?: string; beats?: number[]}> = ({text = 'BOOM', beats = [15, 45, 75]}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const last = [...beats].reverse().find((b) => frame >= b);
  const since = last === undefined ? 999 : frame - last;
  const settle = spring({frame: since - 2, fps, config: {damping: 12, stiffness: 220}});
  const scale = since < 2 ? 1 + 0.125 * (since + 1) : 1 + 0.25 * (1 - settle);
  return (
    <AbsoluteFill style={{background: '#111', overflow: 'hidden'}}>
      <AbsoluteFill style={{transform: `scale(${scale}) rotate(${since < 6 ? (since % 2 ? 2 : -2) : 0}deg)`, transformOrigin: '50% 45%',
        background: 'linear-gradient(135deg,#EC4899,#A855F7)', justifyContent: 'center', alignItems: 'center'}}>
        <div style={{fontFamily: 'Impact, "Anton"', fontSize: 240, color: '#fff'}}>{text}</div>
      </AbsoluteFill>
      <AbsoluteFill style={{background: '#fff', opacity: since === 0 ? 0.35 : 0}} />
    </AbsoluteFill>
  );
};
```

## Variations
- Zoom punch + changement de cadrage (cut sur un plan plus serré).
- Zoom progressif lent (`1 → 1.08` sur toute la phrase) puis punch final.
- Combiner avec `camera-shake`.

## Pièges
- Punch à chaque phrase = fatigue. Le réserver aux 2–3 temps forts.
- Zoom sur une vidéo basse résolution = pixellisation : sources nettes.

# Speed Ramp · `/animation speed-ramp`

> Famille : Transitions & caméra · #91

## C'est quoi
Une vidéo qui ralentit fortement sur le moment clé puis accélère brusquement (ou l'inverse) : effet « slow-mo épique » des montages sport et action.

## Quand l'utiliser
- Sport (saut, frappe, arrivée), danse, transition rythmée sur un drop.

## Technique clé
- Remotion : mapper le temps de sortie → temps source. Avec `<OffthreadVideo>`, piloter `startFrom` n'est pas dynamique ; utiliser plutôt une **courbe de vitesse** intégrée : `sourceFrame(f) = Σ vitesse(i)` pour i < f, et `<Sequence>` n'aide pas. Solution : `<OffthreadVideo>` dans un composant qui reçoit `trimBefore` = `sourceFrame` à chaque frame via `<Freeze frame={...}>`.
- Courbe de vitesse type : 1.0 → 0.25 (ralenti) sur 15 frames, palier, puis 3.0 (accéléré) sur 10 frames, retour à 1.
- Source idéale : filmée en 60/120 i/s pour un ralenti fluide.
- Ajouter flou de mouvement pendant l'accélération et un son « whoosh ».

## Snippet Remotion
```tsx
import {AbsoluteFill, Freeze, interpolate, OffthreadVideo, staticFile, useCurrentFrame} from 'remotion';

// Vitesse de lecture par frame de sortie
const speedAt = (f: number) => interpolate(f, [0, 20, 35, 60, 70, 80], [1, 1, 0.25, 0.25, 3, 1], {extrapolateRight: 'clamp'});

// Frame source = intégrale de la vitesse
const sourceFrame = (f: number) => {
  let s = 0;
  for (let i = 0; i < f; i++) s += speedAt(i);
  return Math.round(s);
};

export const SpeedRamp: React.FC<{text?: string; src?: string}> = ({src = staticFile('saut.mp4')}) => {
  const frame = useCurrentFrame();
  const v = speedAt(frame);
  return (
    <AbsoluteFill style={{background: '#000'}}>
      <Freeze frame={sourceFrame(frame)}>
        <OffthreadVideo src={src} muted style={{width: '100%', height: '100%', objectFit: 'cover', filter: v > 1.5 ? 'blur(2px)' : undefined}} />
      </Freeze>
      <div style={{position: 'absolute', top: 260, right: 140, fontFamily: 'monospace', fontSize: 40, color: '#fff', opacity: 0.8}}>×{v.toFixed(2)}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Freeze frame total (vitesse 0) + zoom + texte, puis reprise.
- Reverse (vitesse négative) pour rejouer l'action.
- Pré-traitement ffmpeg `setpts` + `minterpolate` pour un ralenti plus fluide sur une source 30 i/s.

## Pièges
- Ralenti sur une source 30 i/s = saccadé (frames répétées). Filmer en 60/120 i/s.
- Couper l'audio pendant le ramp (`muted`) et mettre une musique/SFX à la place.

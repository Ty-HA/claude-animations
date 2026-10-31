# Ink / Paint Transition · `/animation ink-transition`

> Famille : Transitions & caméra · #87

## C'est quoi
Une tache d'encre ou de peinture qui éclabousse puis envahit l'écran pour révéler la scène suivante, avec des bords organiques.

## Quand l'utiliser
- Transitions artistiques, mode, musique, branding créatif.

## Technique clé
- Masque organique : cercles (ou blobs) qui grandissent + `feTurbulence` + `feDisplacementMap` (scale 80–150) → bord déchiqueté.
- Utiliser ce SVG comme **masque** de la scène B (`<mask>` + `<foreignObject>`), ou comme couche de couleur pleine entre les deux scènes.
- Plusieurs éclaboussures décalées dans le temps (2–4 frames) et l'espace, qui fusionnent.
- Vraie vidéo d'encre (luma matte) : `mix-blend` ou `maskImage` depuis une vidéo noir/blanc → le plus réaliste.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

const SPLATS = [{x: 300, y: 600, d: 0}, {x: 760, y: 1100, d: 4}, {x: 420, y: 1500, d: 8}, {x: 800, y: 400, d: 6}];

export const InkTransition: React.FC<{text?: string; at?: number}> = ({text = 'NOUVELLE SCÈNE', at = 10}) => {
  const frame = useCurrentFrame();
  return (
    <AbsoluteFill style={{background: '#F4F1EA', justifyContent: 'center', alignItems: 'center', fontFamily: 'Georgia', fontSize: 100, color: '#111'}}>
      Scène A
      <svg width={1080} height={1920} style={{position: 'absolute', inset: 0}}>
        <defs>
          <filter id="splat" x="-50%" y="-50%" width="200%" height="200%">
            <feTurbulence type="fractalNoise" baseFrequency={0.02} numOctaves={3} seed={11} result="n" />
            <feDisplacementMap in="SourceGraphic" in2="n" scale={140} xChannelSelector="R" yChannelSelector="G" />
          </filter>
        </defs>
        <g filter="url(#splat)" fill="#111">
          {SPLATS.map((s, i) => {
            const r = interpolate(frame, [at + s.d, at + s.d + 22], [0, 1300], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp', easing: Easing.in(Easing.cubic)});
            return <circle key={i} cx={s.x} cy={s.y} r={r} />;
          })}
        </g>
      </svg>
      <div style={{position: 'absolute', color: '#fff', fontWeight: 700, opacity: interpolate(frame, [at + 26, at + 34], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'})}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Encre colorée (couleur de marque) au lieu du noir.
- Révéler une vidéo : mettre le groupe SVG dans un `<mask>` appliqué à la scène B.
- Coup de pinceau horizontal : rectangle étiré + displacement (« brush stroke »).

## Pièges
- `feDisplacementMap` déborde : agrandir la zone du filtre (`x/y/width/height`) sinon les bords sont coupés net.
- Grand scale + grands cercles = rendu lent : tester sur quelques frames.

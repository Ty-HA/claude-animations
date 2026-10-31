# Timeline · `/animation timeline`

> Famille : Tableaux de bord & UI · #75

## C'est quoi
Une frise chronologique qui avance : la ligne se trace, chaque étape (date + titre + icône) apparaît à son tour, la caméra défile.

## Quand l'utiliser
- Histoire d'une marque, parcours perso, roadmap, étapes d'un projet, « comment j'en suis arrivé là ».

## Technique clé
- Frise **verticale** en 9:16 (lisible au scroll) : ligne centrale ou à gauche, étapes espacées de 320–400 px.
- Ligne : `height = progress * total` ou path avec `strokeDashoffset`.
- Étape `i` apparaît quand la ligne l'atteint : point qui pop (`spring`), carte qui glisse depuis le côté (alternance gauche/droite).
- Caméra : `translateY(-max(0, lineHead - 1100))` pour garder la tête de ligne dans le cadre.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, spring, useCurrentFrame, useVideoConfig} from 'remotion';

const STEPS = [
  {d: '2019', t: 'Premier site'}, {d: '2021', t: 'Freelance'}, {d: '2023', t: 'BTHA System'}, {d: '2025', t: '10k abonnés'}, {d: '2026', t: '100 animations'},
];
const GAP = 360, TOP = 400;

export const Timeline: React.FC<{text?: string}> = ({text = 'Mon parcours'}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const head = interpolate(frame, [0, 150], [0, (STEPS.length - 1) * GAP + 40], {extrapolateRight: 'clamp'});
  const cam = Math.max(0, TOP + head - 1250);
  return (
    <AbsoluteFill style={{background: '#010012', fontFamily: 'Inter, sans-serif', color: '#fff'}}>
      <div style={{position: 'absolute', top: 220, left: 140, fontSize: 64, fontWeight: 800}}>{text}</div>
      <div style={{position: 'absolute', inset: 0, transform: `translateY(${-cam}px)`}}>
        <div style={{position: 'absolute', left: 538, top: TOP, width: 4, height: head, background: 'linear-gradient(#00B5C7, #A855F7)'}} />
        {STEPS.map((s, i) => {
          const at = (i * GAP) / ((STEPS.length - 1) * GAP + 40) * 150;
          const p = spring({frame: frame - at, fps, config: {damping: 13}});
          const left = i % 2 === 0;
          return (
            <div key={i}>
              <div style={{position: 'absolute', left: 540 - 18, top: TOP + i * GAP - 18, width: 36, height: 36, borderRadius: 18, background: '#67E8F9',
                transform: `scale(${p})`, boxShadow: '0 0 20px #00B5C7'}} />
              <div style={{position: 'absolute', top: TOP + i * GAP - 60, [left ? 'right' : 'left']: 600, width: 330, opacity: p,
                transform: `translateX(${(left ? -1 : 1) * (1 - p) * 60}px)`, textAlign: left ? 'right' : 'left'}}>
                <div style={{fontSize: 34, color: '#67E8F9', fontWeight: 700}}>{s.d}</div>
                <div style={{fontSize: 44, fontWeight: 800}}>{s.t}</div>
              </div>
            </div>
          );
        })}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Frise horizontale qui défile (format 16:9).
- Photos dans des cartes, icônes animées par étape.
- Étape « aujourd'hui » qui pulse en fin de frise.

## Pièges
- Trop d'étapes en 9:16 sans caméra = minuscule. Faire défiler.
- Textes longs : 2–4 mots par étape, pas de paragraphe.

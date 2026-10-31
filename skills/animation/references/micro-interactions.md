# Micro-interactions UI · `/animation micro-interactions`

> Famille : Tableaux de bord & UI · #68

## C'est quoi
Les petites animations d'interface qui donnent vie à une app : bouton qui s'enfonce avec effet ripple, cœur qui explose en « like », toggle qui glisse, icône hamburger qui devient une croix.

## Quand l'utiliser
- Démo d'app, mockups, tutos UX, présentation d'un design system.

## Technique clé
- **Ripple** : cercle qui part du point de clic, `scale 0 → 4`, opacité `0.35 → 0` en 18 frames.
- **Press** : `scale 1 → 0.94 → 1` en 8 frames.
- **Like** : cœur `scale 0 → 1.3 → 1` (spring damping 8) + 8 petites particules en cercle qui s'éloignent et disparaissent.
- **Toggle** : pastille `translateX` + fond qui change de couleur (`interpolateColors`).
- **Hamburger → X** : barres du haut/bas `rotate(±45deg)` + `translateY`, barre du milieu `scaleX(0)`.
- Curseur visible (petit rond) pour montrer où « on clique ».

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, interpolateColors, spring, useCurrentFrame, useVideoConfig} from 'remotion';

export const MicroInteractions: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const like = spring({frame: frame - 15, fps, config: {damping: 8}});
  const burst = interpolate(frame, [15, 35], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});
  const tog = spring({frame: frame - 50, fps, config: {damping: 14}});
  const ham = spring({frame: frame - 80, fps, config: {damping: 14}});
  const bar = (y: number, r: number, sx = 1): React.CSSProperties => ({position: 'absolute', left: 0, top: y, width: 120, height: 14, borderRadius: 7, background: '#fff',
    transform: `translateY(${r === 0 ? 0 : (r > 0 ? 1 : -1) * 36 * ham}px) rotate(${r * ham}deg) scaleX(${sx})`});
  return (
    <AbsoluteFill style={{background: '#111827', justifyContent: 'center', alignItems: 'center', gap: 160}}>
      <div style={{position: 'relative', fontSize: 180}}>
        <span style={{display: 'inline-block', transform: `scale(${frame < 15 ? 1 : like})`, color: frame < 15 ? '#4B5563' : '#F43F5E'}}>♥</span>
        {Array.from({length: 8}, (_, i) => (
          <div key={i} style={{position: 'absolute', left: 70, top: 110, width: 18, height: 18, borderRadius: '50%', background: '#F43F5E', opacity: 1 - burst,
            transform: `rotate(${i * 45}deg) translateY(${-burst * 160}px) scale(${1 - burst})`}} />
        ))}
      </div>
      <div style={{width: 260, height: 140, borderRadius: 70, background: interpolateColors(tog, [0, 1], ['#374151', '#22C55E']), position: 'relative'}}>
        <div style={{position: 'absolute', top: 14, left: 14 + 120 * tog, width: 112, height: 112, borderRadius: '50%', background: '#fff'}} />
      </div>
      <div style={{position: 'relative', width: 120, height: 100}}>
        <div style={bar(0, 45)} /><div style={bar(43, 0, 1 - ham)} /><div style={bar(86, -45)} />
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Bouton « Envoyer » → spinner → check vert.
- Pull-to-refresh, swipe pour supprimer, menu qui se déplie.

## Pièges
- Des springs trop mous paraissent lents : `damping` 8–15 pour l'UI.
- Animer toutes les interactions en même temps : séquencer pour qu'on lise chacune.

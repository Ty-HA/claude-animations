# Ken Burns · `/animation ken-burns`

> Famille : Transitions & caméra · #89

## C'est quoi
Un zoom lent et un léger panoramique sur une photo fixe pour lui donner vie, comme dans les documentaires.

## Quand l'utiliser
- Diaporama, photos de voyage, archives, récit, vlog avec photos.

## Technique clé
- Zoom `1.0 → 1.15` (ou l'inverse) sur toute la durée du plan (3–6 s), **linéaire** ou très légèrement easé.
- Pan simultané : `translate` de quelques %, vers le point d'intérêt (`transformOrigin` sur le sujet).
- Alterner zoom avant / arrière et directions d'une photo à l'autre.
- Fondu enchaîné 10–15 frames entre photos (`<Sequence>` qui se chevauchent).
- `objectFit: 'cover'` pour remplir le 9:16.

## Snippet Remotion
```tsx
import {AbsoluteFill, Img, interpolate, Sequence, useCurrentFrame} from 'remotion';

const PHOTOS = [
  {src: 'https://images.unsplash.com/photo-1502602898657-3e91760cbb34?w=1600', from: 1, to: 1.15, ox: '30%', oy: '40%'},
  {src: 'https://images.unsplash.com/photo-1499856871958-5b9627545d1a?w=1600', from: 1.18, to: 1.02, ox: '70%', oy: '50%'},
];
const DUR = 120, XF = 15;

const Shot: React.FC<{p: (typeof PHOTOS)[number]}> = ({p}) => {
  const frame = useCurrentFrame();
  const s = interpolate(frame, [0, DUR], [p.from, p.to]);
  const o = interpolate(frame, [0, XF, DUR - XF, DUR], [0, 1, 1, 0], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});
  return (
    <AbsoluteFill style={{opacity: o}}>
      <Img src={p.src} style={{width: '100%', height: '100%', objectFit: 'cover', transform: `scale(${s})`, transformOrigin: `${p.ox} ${p.oy}`}} />
    </AbsoluteFill>
  );
};

export const KenBurns: React.FC<{text?: string}> = () => (
  <AbsoluteFill style={{background: '#000'}}>
    {PHOTOS.map((p, i) => (
      <Sequence key={i} from={i * (DUR - XF)} durationInFrames={DUR}><Shot p={p} /></Sequence>
    ))}
  </AbsoluteFill>
);
```

## Variations
- Parallaxe 2.5D (sujet détouré qui bouge plus vite que le fond) : voir `parallax`.
- Titre qui apparaît en bas pendant le zoom.
- Rythme rapide (1 s par photo) calé sur la musique.

## Pièges
- Photos trop petites : zoomer agrandit les pixels. Prévoir ≥ 1,3× la résolution de sortie.
- Zoom trop fort/rapide = mal de mer. Rester sous +20 % par plan.

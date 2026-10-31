# Manga Speed Lines · `/animation manga-speed-lines`

> Famille : Esthétiques artistiques · #60

## C'est quoi
Les lignes de vitesse et de concentration du manga : traits noirs qui convergent vers le centre (focus) ou filent horizontalement (vitesse), avec onomatopées japonaises « ドン ! ».

## Quand l'utiliser
- Moment de choc, révélation, réaction intense, « attaque spéciale ».

## Technique clé
- **Lignes de concentration** : 120–180 triangles très fins partant du bord vers un cercle central vide (rayon intérieur 280–380 px). Chaque triangle : base 6–14 px au bord, pointe vers le centre.
- Redessin toutes les 2 frames avec des longueurs aléatoires (`random(seed + Math.floor(frame/2))`) → vibration.
- Fond blanc ou trame grise (screentone : points 6 px).
- Onomatopée : katakana gras noir avec contour blanc, rotation −10°, apparition `scale 2 → 1` en 4 frames.
- Bonus : flash blanc 2 frames + zoom punch.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, random, useCurrentFrame} from 'remotion';

export const MangaSpeedLines: React.FC<{text?: string; sfx?: string}> = ({text = 'QUOI ?!', sfx = 'ドン'}) => {
  const frame = useCurrentFrame();
  const tick = Math.floor(frame / 2);
  const cx = 540, cy = 900, N = 150;
  const s = interpolate(frame, [0, 4], [2, 1], {extrapolateRight: 'clamp'});
  return (
    <AbsoluteFill style={{background: '#fff'}}>
      <svg width={1080} height={1920}>
        {Array.from({length: N}, (_, i) => {
          const a = (i / N) * Math.PI * 2 + random(`a${i}`) * 0.03;
          const inner = 300 + random(`r${i}-${tick}`) * 160;
          const w = 0.008 + random(`w${i}`) * 0.012;
          const R = 1400;
          const p = (ang: number, r: number) => `${cx + Math.cos(ang) * r},${cy + Math.sin(ang) * r}`;
          return <polygon key={i} points={`${p(a - w, R)} ${p(a + w, R)} ${p(a, inner)}`} fill="#000" />;
        })}
      </svg>
      <div style={{position: 'absolute', left: 0, right: 0, top: cy - 90, textAlign: 'center', fontFamily: '"Bangers", Impact', fontSize: 150, color: '#000',
        transform: `scale(${s})`}}>{text}</div>
      <div style={{position: 'absolute', right: 120, top: 300, fontSize: 200, fontWeight: 900, color: '#000', WebkitTextStroke: '10px #fff', paintOrder: 'stroke fill',
        transform: `rotate(-10deg) scale(${s})`}}>{sfx}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- **Lignes de vitesse horizontales** : traits fins qui défilent de droite à gauche derrière un personnage.
- Fond noir + lignes blanches pour une scène dramatique.
- Combiner avec un avatar manga au centre (réaction « wow »).

## Pièges
- Lignes d'épaisseur uniforme = rendu mécanique : varier largeur et longueur.
- Sans vibration (tick), l'effet est figé : redessiner toutes les 2 frames.

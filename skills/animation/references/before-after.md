# Before / After Slider · `/animation before-after`

> Famille : Tableaux de bord & UI · #81

## C'est quoi
Deux images superposées séparées par un curseur vertical qui glisse et révèle progressivement l'« après ».

## Quand l'utiliser
- Retouche photo, rénovation, redesign d'un site, transformation physique, avant/après IA.

## Technique clé
- Image « après » en dessous, image « avant » au-dessus avec `clipPath: inset(0 ${100 - x}% 0 0)`.
- Poignée : ligne blanche 6 px + rond 90 px avec flèches ‹ ›, ombre portée.
- Mouvement du curseur : aller-retour scénarisé (`interpolate` multi-points : 50 % → 15 % → 85 % → 50 %) avec `Easing.inOut(Easing.cubic)`.
- Labels « AVANT » / « APRÈS » qui s'estompent quand le curseur passe dessus.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, Img, interpolate, useCurrentFrame} from 'remotion';

export const BeforeAfter: React.FC<{text?: string; before?: string; after?: string}> = ({
  before = 'https://images.unsplash.com/photo-1484154218962-a197022b5858?w=1080&sat=-100',
  after = 'https://images.unsplash.com/photo-1484154218962-a197022b5858?w=1080',
}) => {
  const frame = useCurrentFrame();
  const x = interpolate(frame, [0, 30, 70, 110, 140], [50, 12, 88, 50, 50], {extrapolateRight: 'clamp', easing: Easing.inOut(Easing.cubic)});
  const img: React.CSSProperties = {position: 'absolute', inset: 0, width: '100%', height: '100%', objectFit: 'cover'};
  const label = (t: string, side: 'left' | 'right', o: number) => (
    <div style={{position: 'absolute', top: 260, [side]: 140, padding: '14px 26px', borderRadius: 14, background: 'rgba(0,0,0,.6)', color: '#fff',
      fontFamily: 'Inter', fontWeight: 800, fontSize: 40, opacity: o}}>{t}</div>
  );
  return (
    <AbsoluteFill>
      <Img src={after} style={img} />
      <Img src={before} style={{...img, clipPath: `inset(0 ${100 - x}% 0 0)`, filter: 'grayscale(1) brightness(.8)'}} />
      {label('AVANT', 'left', interpolate(x, [20, 35], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'}))}
      {label('APRÈS', 'right', interpolate(x, [65, 80], [1, 0], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'}))}
      <div style={{position: 'absolute', top: 0, bottom: 0, left: `${x}%`, width: 6, marginLeft: -3, background: '#fff', boxShadow: '0 0 20px rgba(0,0,0,.4)'}} />
      <div style={{position: 'absolute', top: 915, left: `${x}%`, width: 90, height: 90, marginLeft: -45, borderRadius: 45, background: '#fff',
        display: 'flex', justifyContent: 'center', alignItems: 'center', fontSize: 40, color: '#111', boxShadow: '0 8px 20px rgba(0,0,0,.4)'}}>‹ ›</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Slider horizontal (haut = avant, bas = après) en 9:16.
- Avant/après vidéo : deux `<OffthreadVideo>` synchronisées.
- Main qui « tire » la poignée (voir `cursor-demo`).

## Pièges
- Les deux images doivent être exactement alignées (même cadrage), sinon l'effet est raté.
- Pas de faux avant/après (transformation santé/beauté) présenté comme réel.

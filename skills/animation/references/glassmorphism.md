# Glassmorphism · `/animation glassmorphism`

> Famille : Styles 3D · #31

## C'est quoi
Des panneaux de verre dépoli semi-transparents qui floutent ce qui passe derrière, avec un liseré lumineux : l'esthétique iOS / visionOS.

## Quand l'utiliser
- Cartes d'interface, stats, notifications, mockups d'app premium.
- Toujours **devant un fond coloré qui bouge** (sinon le verre ne se voit pas).

## Technique clé
- Panneau : `background: rgba(255,255,255,.08)`, `backdropFilter: 'blur(24px) saturate(160%)'`, `border: 1px solid rgba(255,255,255,.25)`, `borderRadius: 32`, `boxShadow: 0 20px 60px rgba(0,0,0,.35)`.
- Reflet : `linear-gradient(135deg, rgba(255,255,255,.35), transparent 40%)` en surcouche, qui glisse lentement.
- Fond : formes colorées vives (voir `aurora`) qui passent derrière.
- Entrée : `spring` sur `translateY(80 → 0)` + `scale(0.94 → 1)` + opacité.

## Snippet Remotion
```tsx
import {AbsoluteFill, spring, useCurrentFrame, useVideoConfig} from 'remotion';

export const Glassmorphism: React.FC<{text?: string}> = ({text = '+248 %'}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const t = frame / 30;
  const s = spring({frame: frame - 5, fps, config: {damping: 16}});
  return (
    <AbsoluteFill style={{background: '#0b0a1f', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{position: 'absolute', width: 420, height: 420, borderRadius: '50%', background: '#EC4899', left: 160 + 140 * Math.sin(t), top: 620}} />
      <div style={{position: 'absolute', width: 360, height: 360, borderRadius: '50%', background: '#00B5C7', left: 520 + 120 * Math.cos(t * 0.8), top: 980}} />
      <div style={{
        position: 'relative', width: 720, height: 460, borderRadius: 36, overflow: 'hidden',
        background: 'rgba(255,255,255,.08)', backdropFilter: 'blur(24px) saturate(160%)',
        border: '1px solid rgba(255,255,255,.25)', boxShadow: '0 20px 60px rgba(0,0,0,.35)',
        transform: `translateY(${(1 - s) * 80}px) scale(${0.94 + 0.06 * s})`, opacity: s,
        display: 'flex', flexDirection: 'column', justifyContent: 'center', padding: 60, color: '#fff', fontFamily: 'Inter, sans-serif',
      }}>
        <div style={{position: 'absolute', inset: 0, background: `linear-gradient(135deg, rgba(255,255,255,.3), transparent ${30 + 10 * Math.sin(t)}%)`}} />
        <div style={{fontSize: 34, opacity: 0.8}}>Vues cette semaine</div>
        <div style={{fontSize: 140, fontWeight: 800}}>{text}</div>
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Pile de cartes en verre décalées en profondeur (`translateZ` + `perspective`).
- Verre teinté (rgba cyan) pour coller à une marque.
- Bouton/notification iOS en verre qui tombe du haut.

## Pièges
- `backdrop-filter` n'a rien à flouter sur un fond uni : il faut du mouvement derrière.
- Texte blanc fin sur verre clair = illisible : texte gras + légère ombre.

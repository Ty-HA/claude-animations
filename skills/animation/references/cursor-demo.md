# Cursor Demo · `/animation cursor-demo`

> Famille : Tableaux de bord & UI · #80

## C'est quoi
Un curseur de souris qui se déplace, survole et clique dans une interface : la démo produit « regarde comme c'est simple ».

## Quand l'utiliser
- Tutos SaaS, présentation d'une feature, onboarding, avant/après d'un workflow.

## Technique clé
- Script de positions-clés `[{f, x, y, click?}]`. Position : interpolation entre keyframes avec `Easing.inOut(Easing.cubic)` (ou trajectoire légèrement courbe via un point de contrôle).
- Clic : curseur `scale(0.85)` 4 frames + onde circulaire (ripple) au point de clic + effet sur l'élément visé (état « pressé », puis changement d'UI).
- Survol : élément qui s'éclaire quand le curseur est dedans (hit-test simple sur rectangles).
- **Zoom caméra** sur la zone d'action : `scale(1.6)` centré sur le curseur, puis retour.
- Curseur SVG flèche noire contour blanc, ombre portée.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

const KEYS = [{f: 0, x: 900, y: 1500}, {f: 30, x: 540, y: 1000, click: true}, {f: 70, x: 540, y: 1000}, {f: 100, x: 760, y: 700, click: true}];

const at = (frame: number, k: 'x' | 'y') => interpolate(frame, KEYS.map((q) => q.f), KEYS.map((q) => q[k]),
  {extrapolateRight: 'clamp', easing: Easing.inOut(Easing.cubic)});

export const CursorDemo: React.FC<{text?: string}> = ({text = 'Générer'}) => {
  const frame = useCurrentFrame();
  const x = at(frame, 'x'), y = at(frame, 'y');
  const clicks = KEYS.filter((k) => k.click).map((k) => k.f);
  const last = clicks.filter((f) => frame >= f).pop();
  const since = last === undefined ? 99 : frame - last;
  const done = frame >= 30;
  return (
    <AbsoluteFill style={{background: '#F3F4F6', fontFamily: 'Inter, sans-serif'}}>
      <div style={{position: 'absolute', left: 340, top: 940, width: 400, height: 120, borderRadius: 24, display: 'flex', justifyContent: 'center', alignItems: 'center',
        fontSize: 44, fontWeight: 700, color: '#fff', background: done ? '#22C55E' : '#6366F1', transform: `scale(${since < 4 ? 0.95 : 1})`}}>
        {done ? '✓ Terminé' : text}
      </div>
      {since < 18 && <div style={{position: 'absolute', left: x - 60, top: y - 60, width: 120, height: 120, borderRadius: '50%',
        border: '4px solid #6366F1', opacity: 1 - since / 18, transform: `scale(${0.3 + since / 10})`}} />}
      <svg width={60} height={80} viewBox="0 0 24 32" style={{position: 'absolute', left: x, top: y, transform: `scale(${since < 4 ? 0.85 : 1})`, transformOrigin: 'top left',
        filter: 'drop-shadow(0 4px 6px rgba(0,0,0,.35))'}}>
        <path d="M2 2 L2 26 L8 20 L12 30 L16 28 L12 18 L20 18 Z" fill="#111" stroke="#fff" strokeWidth={2} strokeLinejoin="round" />
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Main/doigt (tap mobile) au lieu du curseur.
- Texte tapé dans un champ après le clic (voir `typewriter`).
- Zoom automatique sur chaque clic.

## Pièges
- Mouvements linéaires = robotique : toujours un easing, idéalement une légère courbe.
- Clic trop rapide après l'arrivée : laisser 3–5 frames d'arrêt avant de cliquer.

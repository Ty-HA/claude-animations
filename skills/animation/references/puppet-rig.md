# Puppet Rig · `/animation puppet-rig`

> Famille : Personnages & fun · #95

## C'est quoi
Un personnage articulé comme une marionnette : tête, bras, avant-bras, jambes pivotent autour de leurs articulations (hiérarchie parent → enfant), avec clignements et expressions.

## Quand l'utiliser
- Mascotte animée, explainer avec personnage, avatar qui gesticule (coucou, pointer, danser).

## Technique clé
- **Hiérarchie** : chaque membre est un `<g transform="translate(articulation) rotate(angle)">` qui contient l'enfant → l'avant-bras suit le bras.
- Pivot = point d'articulation (épaule, coude) placé à l'origine du groupe.
- Angles pilotés par des fonctions du temps : `épaule = -120 + 20 * sin(t*6)` (coucou), ou keyframes `interpolate` pour des poses.
- **Overlapping action** : l'avant-bras suit avec un retard (`sin(t*6 - 0.6)`) → mouvement naturel.
- Clignement : `scaleY(0.1)` sur les yeux 3 frames toutes les ~90 frames.
- Pour des illustrations existantes : découper en calques PNG (tête, torse, bras…) et les assembler pareil.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

const Limb: React.FC<{len: number; w: number; color: string; children?: React.ReactNode; angle: number; x?: number; y?: number}> = ({len, w, color, children, angle, x = 0, y = 0}) => (
  <g transform={`translate(${x} ${y}) rotate(${angle})`}>
    <line x1={0} y1={0} x2={0} y2={len} stroke={color} strokeWidth={w} strokeLinecap="round" />
    <g transform={`translate(0 ${len})`}>{children}</g>
  </g>
);

export const PuppetRig: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const t = frame / 30;
  const blink = frame % 90 > 86 ? 0.1 : 1;
  return (
    <AbsoluteFill style={{background: '#EAF4FF'}}>
      <svg width={1080} height={1920}>
        <g transform={`translate(540 ${900 + Math.abs(Math.sin(t * 3)) * -10})`}>
          <Limb x={-40} y={260} len={140} w={44} color="#2D3047" angle={8}><Limb len={140} w={40} color="#2D3047" angle={-6} /></Limb>
          <Limb x={40} y={260} len={140} w={44} color="#2D3047" angle={-8}><Limb len={140} w={40} color="#2D3047" angle={6} /></Limb>
          <rect x={-90} y={0} width={180} height={280} rx={60} fill="#FF6B6B" />
          <Limb x={-80} y={40} len={120} w={36} color="#FFB4A2" angle={20 + 5 * Math.sin(t * 2)}><Limb len={110} w={32} color="#FFB4A2" angle={-10} /></Limb>
          <Limb x={80} y={40} len={120} w={36} color="#FFB4A2" angle={-150 + 20 * Math.sin(t * 6)}>
            <Limb len={110} w={32} color="#FFB4A2" angle={-30 + 25 * Math.sin(t * 6 - 0.6)} />
          </Limb>
          <g transform={`translate(0 -20) rotate(${4 * Math.sin(t * 2)})`}>
            <circle cx={0} cy={-90} r={100} fill="#FFB4A2" />
            {[-35, 35].map((x) => <ellipse key={x} cx={x} cy={-100} rx={12} ry={18 * blink} fill="#2D3047" />)}
            <path d="M -30 -50 Q 0 -25 30 -50" fill="none" stroke="#2D3047" strokeWidth={8} strokeLinecap="round" />
          </g>
        </g>
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Bibliothèque de poses (keyframes d'angles) et transitions `spring` entre elles.
- Rig sur une illustration découpée en PNG (même hiérarchie, `<image>` au lieu de `<line>`).
- Cinématique inverse simple (2 os) pour poser la main à un point précis.

## Pièges
- Pivot mal placé = membre qui tourne autour de son centre. L'origine du groupe = articulation.
- Ordre de dessin : bras arrière avant le torse, bras avant après.

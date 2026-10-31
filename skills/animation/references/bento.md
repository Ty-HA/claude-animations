# Bento Grid · `/animation bento`

> Famille : Tableaux de bord & UI · #66

## C'est quoi
Une grille de cases asymétriques (comme une boîte bento japonaise ou les keynotes Apple) qui apparaissent et s'animent chacune avec un contenu : chiffre, icône, mini-graphe, photo.

## Quand l'utiliser
- Résumer les features d'un produit, un récap d'année, un profil, un portfolio.

## Technique clé
- CSS Grid : `gridTemplateColumns: 'repeat(4, 1fr)'`, cases avec `gridColumn: 'span 2'`, `gridRow: 'span 2'`, gap 20 px, rayons 32 px.
- Entrée en cascade : chaque case `spring` sur `scale 0.85 → 1` + opacité, ordre en diagonale (`delay = (col + row) * 4`).
- Contenu vivant dans chaque case (compteur, barre, icône qui tourne) dès qu'elle est apparue.
- Style : fond `#0B0B10`, cases `#16161F` avec bordure 1 px `rgba(255,255,255,.08)`, une case héros en couleur.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, spring, useCurrentFrame, useVideoConfig} from 'remotion';

const CELLS = [
  {c: 'span 2', r: 'span 2', bg: 'linear-gradient(135deg,#A855F7,#EC4899)', label: 'Vues', val: 1200000, row: 0, col: 0},
  {c: 'span 2', r: 'span 1', bg: '#16161F', label: 'Abonnés', val: 48200, row: 0, col: 2},
  {c: 'span 1', r: 'span 1', bg: '#16161F', label: 'Vidéos', val: 132, row: 1, col: 2},
  {c: 'span 1', r: 'span 1', bg: '#00B5C7', label: 'Likes', val: 98000, row: 1, col: 3},
  {c: 'span 4', r: 'span 1', bg: '#16161F', label: 'Partages', val: 22400, row: 2, col: 0},
];

export const Bento: React.FC<{text?: string}> = ({text = 'Mon année 2026'}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  return (
    <AbsoluteFill style={{background: '#0B0B10', padding: '300px 140px', fontFamily: 'Inter, sans-serif', color: '#fff'}}>
      <div style={{fontSize: 64, fontWeight: 800, marginBottom: 40}}>{text}</div>
      <div style={{display: 'grid', gridTemplateColumns: 'repeat(4, 1fr)', gridAutoRows: 190, gap: 20}}>
        {CELLS.map((cell, i) => {
          const d = (cell.row + cell.col) * 4;
          const s = spring({frame: frame - d, fps, config: {damping: 15}});
          const n = Math.floor(interpolate(frame, [d, d + 40], [0, cell.val], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'}));
          return (
            <div key={i} style={{gridColumn: cell.c, gridRow: cell.r, background: cell.bg, borderRadius: 32, border: '1px solid rgba(255,255,255,.08)',
              padding: 28, display: 'flex', flexDirection: 'column', justifyContent: 'flex-end', transform: `scale(${0.85 + 0.15 * s})`, opacity: s}}>
              <div style={{fontSize: 26, opacity: 0.7}}>{cell.label}</div>
              <div style={{fontSize: 56, fontWeight: 800}}>{n.toLocaleString('fr-FR')}</div>
            </div>
          );
        })}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Cases qui se réorganisent (changement de layout) avec interpolation de position.
- Photo/vidéo dans la case héros, mini-graphe dans une autre.

## Pièges
- Trop de cases sur mobile : 5–7 max en 9:16.
- `toLocaleString` dépend de la locale du rendu : forcer `'fr-FR'`.

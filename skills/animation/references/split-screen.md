# Split Screen · `/animation split-screen`

> Famille : Transitions & caméra · #90

## C'est quoi
L'écran se divise en 2, 3 ou 4 zones qui s'ouvrent et montrent plusieurs plans en même temps.

## Quand l'utiliser
- Comparaison (avant/après, toi vs moi), montrer plusieurs angles, réactions, multitâche.

## Technique clé
- Zones avec `clipPath: inset(...)` ou conteneurs flex dont la taille s'anime.
- Ouverture : la ligne de séparation part du centre (`height: 0 → 50%` pour chaque moitié) ou les volets glissent depuis les bords.
- Séparateur fin (6–10 px) en couleur d'accent ou blanc.
- En 9:16 : division **horizontale** (haut / bas) plus lisible ; 3 bandes possibles.
- Labels dans chaque zone (« AVANT » / « APRÈS »).

## Snippet Remotion
```tsx
import {AbsoluteFill, spring, useCurrentFrame, useVideoConfig} from 'remotion';

export const SplitScreen: React.FC<{text?: string; top?: string; bottom?: string}> = ({top = 'AVANT', bottom = 'APRÈS'}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const p = spring({frame: frame - 5, fps, config: {damping: 16}});
  const half = 960 * p;
  const zone = (bg: string): React.CSSProperties => ({
    position: 'absolute', left: 0, right: 0, background: bg, overflow: 'hidden', display: 'flex', justifyContent: 'center', alignItems: 'center',
    fontFamily: 'Inter', fontWeight: 900, fontSize: 120, color: '#fff',
  });
  return (
    <AbsoluteFill style={{background: '#000'}}>
      <div style={{...zone('linear-gradient(#475569,#1e293b)'), top: 960 - half, height: half}}>{top}</div>
      <div style={{...zone('linear-gradient(#00B5C7,#A855F7)'), top: 960, height: half}}>{bottom}</div>
      <div style={{position: 'absolute', left: 0, right: 0, top: 955, height: 10, background: '#fff', transform: `scaleX(${p})`}} />
    </AbsoluteFill>
  );
};
```

## Variations
- 4 cases (grille 2×2) qui s'ouvrent en cascade.
- Split diagonal (`clipPath: polygon`).
- La séparation se déplace pour donner plus de place à la zone qui parle.

## Pièges
- Vidéos en 9:16 coupées en deux = sujets coupés. Recadrer chaque source (`objectFit: cover` + `objectPosition`).
- Trop de zones en 9:16 : 2–3 max.

# Data Storytelling · `/animation data-story`

> Famille : Tableaux de bord & UI · #65

## C'est quoi
Des données qui racontent une histoire : barres qui montent, courbe qui se trace, point clé mis en évidence avec annotation.

## Quand l'utiliser
- Résultats, évolution, avant/après chiffré, preuve sociale, rapport annuel animé.

## Technique clé
- **Barres** : hauteur = `valeur * spring({damping: 18})`, décalage 4 frames par barre ; valeur affichée au-dessus qui compte en même temps.
- **Courbe** : path SVG (polyline lissée), `pathLength={1}` + `strokeDashoffset 1 → 0` en 40 frames, aire sous la courbe avec dégradé qui apparaît en fondu.
- **Focus** : point final qui pulse + étiquette (« +312 % ») qui pop.
- Axes discrets (gris 20 %), pas de grille lourde. Une couleur d'accent pour la donnée héros.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, spring, useCurrentFrame, useVideoConfig} from 'remotion';

const DATA = [12, 18, 15, 26, 34, 31, 48, 62];

export const DataStory: React.FC<{text?: string}> = ({text = '+417 %'}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const W = 800, H = 600, X0 = 140, Y0 = 1300, max = 70;
  const pts = DATA.map((v, i) => [X0 + (i / (DATA.length - 1)) * W, Y0 - (v / max) * H]);
  const line = interpolate(frame, [20, 60], [1, 0], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});
  const pop = spring({frame: frame - 60, fps, config: {damping: 10}});
  const [lx, ly] = pts[pts.length - 1];
  return (
    <AbsoluteFill style={{background: '#010012', fontFamily: 'Inter, sans-serif'}}>
      <svg width={1080} height={1920}>
        <defs><linearGradient id="area" x1="0" y1="0" x2="0" y2="1"><stop offset="0" stopColor="#00B5C7" stopOpacity={0.45} /><stop offset="1" stopColor="#00B5C7" stopOpacity={0} /></linearGradient></defs>
        {DATA.map((v, i) => {
          const h = (v / max) * H * spring({frame: frame - i * 4, fps, config: {damping: 18}});
          return <rect key={i} x={pts[i][0] - 30} y={Y0 - h} width={60} height={h} rx={10} fill="#155E75" opacity={0.5} />;
        })}
        <polygon points={`${X0},${Y0} ${pts.map((p) => p.join(',')).join(' ')} ${X0 + W},${Y0}`} fill="url(#area)" opacity={1 - line} />
        <polyline points={pts.map((p) => p.join(',')).join(' ')} fill="none" stroke="#67E8F9" strokeWidth={8} strokeLinejoin="round"
          pathLength={1} strokeDasharray={1} strokeDashoffset={line} />
        <circle cx={lx} cy={ly} r={14 + 6 * Math.sin(frame / 5) * pop} fill="#EC4899" opacity={pop} />
      </svg>
      <div style={{position: 'absolute', left: lx - 260, top: ly - 170, fontSize: 96, fontWeight: 900, color: '#fff', transform: `scale(${pop})`}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Comparaison 2 séries (toi vs moyenne) avec la tienne en accent.
- Camembert / donut qui se remplit (`strokeDasharray` sur un cercle), voir `progress-rings`.
- Carte choroplèthe qui se colore région par région.

## Pièges
- Trop de données = illisible en 9:16. 5–10 points max, un message par graphe.
- Échelle qui commence ailleurs qu'à 0 sur des barres = trompeur.

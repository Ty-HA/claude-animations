# Bar Chart Race · `/animation bar-race`

> Famille : Tableaux de bord & UI · #72

## C'est quoi
Un classement de barres horizontales qui grandissent dans le temps et **se doublent** pour changer de rang, avec la date qui défile.

## Quand l'utiliser
- Évolution d'un classement : réseaux sociaux, langages, pays, marques, années.

## Technique clé
- Données par période (keyframes). Valeur continue à la frame : interpolation linéaire entre deux keyframes.
- Rang continu : trier par valeur interpolée → rang entier, puis **interpoler la position Y** du rang pour un dépassement fluide (`spring` vers la nouvelle position ou rang fractionnaire calculé à partir des keyframes).
- Barres : largeur = `valeur / max(valeurs courantes) * W`, couleur fixe par entité, label à gauche, valeur à droite (count-up).
- Compteur de date en gros en bas, en filigrane.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, useCurrentFrame} from 'remotion';

const YEARS = [2020, 2022, 2024, 2026];
const DATA: Record<string, {c: string; v: number[]}> = {
  Python: {c: '#3776AB', v: [30, 45, 70, 95]}, JavaScript: {c: '#F7DF1E', v: [60, 65, 72, 80]}, TypeScript: {c: '#3178C6', v: [15, 35, 60, 88]},
  Java: {c: '#E76F00', v: [55, 52, 50, 48]}, Rust: {c: '#DEA584', v: [5, 12, 25, 40]},
};
const PER = 40; // frames par période
const BAR = 130;

const valueAt = (v: number[], f: number) => {
  const i = Math.min(v.length - 2, Math.floor(f / PER));
  return interpolate(f, [i * PER, (i + 1) * PER], [v[i], v[i + 1]], {extrapolateRight: 'clamp'});
};

export const BarRace: React.FC<{text?: string}> = ({text = 'Langages les plus utilisés'}) => {
  const frame = useCurrentFrame();
  const rows = Object.entries(DATA).map(([k, d]) => ({k, c: d.c, v: valueAt(d.v, frame), prev: valueAt(d.v, frame - 8)}));
  const rankOf = (key: 'v' | 'prev', k: string) => [...rows].sort((a, b) => b[key] - a[key]).findIndex((r) => r.k === k);
  const max = Math.max(...rows.map((r) => r.v));
  const year = Math.round(interpolate(frame, [0, PER * (YEARS.length - 1)], [YEARS[0], YEARS[YEARS.length - 1]], {extrapolateRight: 'clamp'}));
  return (
    <AbsoluteFill style={{background: '#0B0B10', fontFamily: 'Inter, sans-serif', color: '#fff'}}>
      <div style={{position: 'absolute', top: 300, left: 140, fontSize: 52, fontWeight: 800}}>{text}</div>
      {rows.map((r) => {
        const y = 480 + ((rankOf('v', r.k) + rankOf('prev', r.k)) / 2) * BAR;
        return (
          <div key={r.k} style={{position: 'absolute', left: 140, top: y, height: BAR - 24, width: (r.v / max) * 700, background: r.c, borderRadius: 12,
            display: 'flex', alignItems: 'center', justifyContent: 'space-between', padding: '0 20px', fontSize: 34, fontWeight: 700, color: '#0B0B10', minWidth: 220}}>
            <span>{r.k}</span><span>{Math.round(r.v)}</span>
          </div>
        );
      })}
      <div style={{position: 'absolute', bottom: 560, right: 140, fontSize: 160, fontWeight: 900, opacity: 0.2}}>{year}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Logos/drapeaux au bout des barres.
- Mettre en évidence une entité (les autres en gris).
- Annotations d'événements (« sortie de ChatGPT ») qui apparaissent à leur date.

## Pièges
- Rang qui saute d'un coup : moyenner le rang sur plusieurs frames (ici `v` et `prev`) ou animer Y avec un spring.
- Trop d'entités : 5–8 barres en 9:16.

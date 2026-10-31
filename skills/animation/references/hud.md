# HUD & Terminal Sci-Fi · `/animation hud`

> Famille : Typographie cinétique · #4

## C'est quoi
Interface futuriste façon cockpit ou film de SF : police monospace, cercles de visée qui tournent, compteurs qui défilent, coins en équerre et lignes de scan.

## Quand l'utiliser
- Présenter des stats, un profil, un « scan » de produit.
- Esthétique cyberpunk, tech, gaming, IA.

## Technique clé
- Palette : fond `#010012`, traits cyan `#00B5C7` / `#67E8F9`, alertes `#FF3D71`. Traits 1–2 px avec `drop-shadow(0 0 6px cyan)`.
- **Réticule** : 2 cercles SVG en pointillés (`strokeDasharray="4 10"`) qui tournent en sens inverse (`rotate(frame*1.5)` / `rotate(-frame)`).
- **Compteurs** : `Math.floor(interpolate(frame,[0,45],[0,target]))` avec padding zéro (`toString().padStart(4,'0')`).
- **Lignes de log** qui apparaissent une par une toutes les 6 frames, préfixées `>`.
- Coins en équerre : 4 `div` avec `borderTop` + `borderLeft` 3 px tournés de 0/90/180/270°.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, useCurrentFrame} from 'remotion';

const CYAN = '#67E8F9';

export const Hud: React.FC<{text?: string}> = ({text = 'SCAN COMPLETE'}) => {
  const frame = useCurrentFrame();
  const pct = Math.floor(interpolate(frame, [0, 60], [0, 100], {extrapolateRight: 'clamp'}));
  const logs = ['> init neural core', '> link BTHA.SYSTEM', '> calibrate optics', `> ${text.toLowerCase()}`];
  return (
    <AbsoluteFill style={{background: '#010012', fontFamily: 'monospace', color: CYAN}}>
      <svg width={1080} height={1080} style={{position: 'absolute', top: 280}}>
        <g transform="translate(540 540)" stroke={CYAN} fill="none" style={{filter: `drop-shadow(0 0 6px ${CYAN})`}}>
          <circle r={300} strokeDasharray="4 10" transform={`rotate(${frame * 1.5})`} />
          <circle r={240} strokeWidth={3} strokeDasharray="120 40" transform={`rotate(${-frame})`} />
          <line x1={-340} x2={-200} /><line x1={200} x2={340} /><line y1={-340} y2={-200} /><line y1={200} y2={340} />
        </g>
        <text x={540} y={560} textAnchor="middle" fill={CYAN} fontSize={90} fontWeight={700}>{String(pct).padStart(3, '0')}%</text>
      </svg>
      <div style={{position: 'absolute', left: 140, top: 1350, fontSize: 34, lineHeight: 1.6}}>
        {logs.slice(0, Math.floor(frame / 12) + 1).map((l) => <div key={l}>{l}</div>)}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Ajouter un graphe sinusoïdal qui défile (`path` recalculé par frame).
- Version « alerte » : tout passe en rouge `#FF3D71` avec clignotement 4 frames.
- Combiner avec `scramble` pour les titres.

## Pièges
- Trop d'éléments qui bougent = bruit. Une seule chose « héros » par plan (le réticule ou le chiffre).
- Les traits de 1 px scintillent en compression TikTok : préférer 2 px minimum.

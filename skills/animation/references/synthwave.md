# Synthwave / Vaporwave · `/animation synthwave`

> Famille : Esthétiques artistiques · #51

## C'est quoi
L'esthétique rétro-futuriste années 80 : grille néon infinie qui défile vers l'horizon, soleil couchant rayé en dégradé, montagnes filaires, ciel violet.

## Quand l'utiliser
- Musique, gaming, nostalgie, intros « rétro-tech », tout ce qui doit avoir du style.

## Technique clé
- Ciel : `linear-gradient(#0d0221 0%, #2a0a4a 45%, #ff2e97 100%)` au-dessus de l'horizon (y ≈ 55 %).
- Soleil : disque `linear-gradient(#ffe259, #ff2e97)` + bandes horizontales transparentes de plus en plus épaisses vers le bas (`mask-image: repeating-linear-gradient`).
- **Grille** : sol en perspective. Lignes horizontales à `y_k = horizon + f / (k - offset)` avec `offset = (frame * vitesse) % 1` → défilement infini. Lignes verticales en éventail depuis le point de fuite.
- Couleur grille `#ff2e97` ou `#00f0ff`, glow `drop-shadow`.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

export const Synthwave: React.FC<{text?: string}> = ({text = 'OUTRUN'}) => {
  const frame = useCurrentFrame();
  const HZ = 1050, W = 1080;
  const off = (frame * 0.04) % 1;
  const hLines = Array.from({length: 24}, (_, k) => HZ + 260 / (k + 1 - off) - 10).filter((y) => y > HZ && y < 1920);
  return (
    <AbsoluteFill style={{background: 'linear-gradient(#0d0221 0%, #2a0a4a 40%, #ff2e97 55%, #0d0221 55%)'}}>
      <div style={{position: 'absolute', left: 290, top: HZ - 420, width: 500, height: 500, borderRadius: '50%',
        background: 'linear-gradient(#ffe259 10%, #ff2e97 90%)', boxShadow: '0 0 80px #ff2e97',
        WebkitMaskImage: 'linear-gradient(#000 50%, transparent 50%), repeating-linear-gradient(#000 0 22px, transparent 22px 34px)',
        clipPath: 'inset(0 0 80px 0)'}} />
      <svg width={W} height={1920} style={{position: 'absolute', inset: 0, filter: 'drop-shadow(0 0 6px #ff2e97)'}}>
        {hLines.map((y, i) => <line key={i} x1={0} x2={W} y1={y} y2={y} stroke="#ff2e97" strokeWidth={2.5} />)}
        {Array.from({length: 25}, (_, i) => {
          const x = (i - 12) * 220;
          return <line key={`v${i}`} x1={W / 2 + x * 0.05} y1={HZ} x2={W / 2 + x * 4} y2={1920} stroke="#ff2e97" strokeWidth={2.5} />;
        })}
      </svg>
      <div style={{position: 'absolute', top: 340, width: '100%', textAlign: 'center', fontFamily: '"Monoton", "Arial Black"', fontSize: 150,
        color: '#00f0ff', fontStyle: 'italic', textShadow: '0 0 20px #00f0ff, 6px 6px 0 #ff2e97'}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Montagnes filaires (polyline en zigzag) sur l'horizon.
- Voiture/palmier en silhouette, étoiles qui scintillent.
- Vaporwave : buste grec, fenêtres Windows 95, palette rose/cyan pastel.

## Pièges
- Lignes horizontales espacées linéairement = pas de perspective. Toujours en `1/k`.
- Le défilement doit boucler (`% 1`) sans saut : vérifier la première et la dernière frame.

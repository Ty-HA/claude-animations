# ASCII Art Animation · `/animation ascii`

> Famille : Typographie cinétique · #5

## C'est quoi
Une image ou une forme dessinée uniquement avec des caractères (`@#%*+=-:. `) qui changent en temps réel selon la luminosité.

## Quand l'utiliser
- Ambiance hacker, rétro-informatique, « matrice ».
- Reveal d'un logo ou d'un visage en caractères.

## Technique clé
- Grille de N colonnes × M lignes (ex. 48 × 80 en 9:16). Pour chaque case, calculer une luminosité 0–1 et la mapper sur `RAMP = ' .:-=+*#%@'`.
- Luminosité procédurale : ondes `sin(x*0.3 + frame*0.15) * cos(y*0.2 - frame*0.1)`, ou distance à un cercle qui pulse.
- Pour une vraie image : la dessiner sur un `<canvas>` caché, lire `getImageData`, échantillonner (à faire dans `useEffect` + `delayRender`).
- Police monospace, `line-height: 1`, `letter-spacing` réglé pour des cases carrées. Couleur verte `#33FF66` ou cyan.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

const RAMP = ' .:-=+*#%@';
const COLS = 48, ROWS = 84;

export const Ascii: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const t = frame / 30;
  const lines: string[] = [];
  for (let y = 0; y < ROWS; y++) {
    let row = '';
    for (let x = 0; x < COLS; x++) {
      const dx = x - COLS / 2, dy = (y - ROWS / 2) * 0.55;
      const d = Math.sqrt(dx * dx + dy * dy);
      const v = 0.5 + 0.5 * Math.sin(d * 0.45 - t * 4) * Math.cos(Math.atan2(dy, dx) * 3 + t);
      row += RAMP[Math.min(RAMP.length - 1, Math.floor(v * RAMP.length))];
    }
    lines.push(row);
  }
  return (
    <AbsoluteFill style={{background: '#000', justifyContent: 'center', alignItems: 'center'}}>
      <pre style={{margin: 0, fontFamily: 'monospace', fontSize: 22, lineHeight: '22px', letterSpacing: 0.4, color: '#33FF66'}}>
        {lines.join('\n')}
      </pre>
    </AbsoluteFill>
  );
};
```

## Variations
- Écrire un mot : rendre le texte sur un canvas caché, puis convertir en ASCII.
- Couleur par caractère (dégradé cyan → violet selon la ligne).
- Mélanger avec `scramble` : les caractères se « décodent » vers l'image finale.

## Pièges
- Une grille trop fine = texte illisible et rendu lent. Rester sous ~5 000 caractères.
- Les polices non monospace cassent l'alignement.

# Comic Book / Cel Shading · `/animation comic`

> Famille : Esthétiques artistiques · #53

## C'est quoi
Le style bande dessinée : contours noirs épais, aplats de couleurs, trame de points Ben-Day (Pop Art), cases, bulles et onomatopées « POW! ».

## Quand l'utiliser
- Moment d'impact, storytelling en cases, pop art, humour.

## Technique clé
- Trame Ben-Day : `radial-gradient(circle, couleur 30%, transparent 32%)` en `backgroundSize: 18px 18px`.
- Onomatopée : texte très gras italique, contour noir (`WebkitTextStroke: 10px #000` + `paintOrder: stroke fill`), dans une étoile d'explosion (polygon à 16 pointes alternées).
- Entrée « impact » : `scale 0 → 1.3 → 1` en 6 frames + rotation −8°, shake 3 frames.
- Cases : grille avec gouttière blanche 20 px, bordure noire 8 px, apparition case par case.
- Cel shading : 2 tons par couleur (base + ombre en aplat net), pas de dégradé.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, useCurrentFrame} from 'remotion';

const burst = (n: number, R: number, r: number) =>
  Array.from({length: n * 2}, (_, i) => {
    const a = (i / (n * 2)) * Math.PI * 2, rad = i % 2 ? r : R;
    return `${Math.cos(a) * rad},${Math.sin(a) * rad}`;
  }).join(' ');

export const Comic: React.FC<{text?: string}> = ({text = 'POW!'}) => {
  const frame = useCurrentFrame();
  const s = interpolate(frame, [4, 8, 12], [0, 1.3, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});
  const shake = frame > 8 && frame < 12 ? (frame % 2 ? 8 : -8) : 0;
  return (
    <AbsoluteFill style={{background: '#FFD400', backgroundImage: 'radial-gradient(circle, #FF3D3D 30%, transparent 32%)', backgroundSize: '18px 18px',
      justifyContent: 'center', alignItems: 'center', transform: `translateX(${shake}px)`}}>
      <svg width={900} height={900} style={{position: 'absolute', transform: `scale(${s}) rotate(-8deg)`}}>
        <polygon points={burst(16, 430, 300)} transform="translate(450 450)" fill="#fff" stroke="#000" strokeWidth={12} strokeLinejoin="round" />
      </svg>
      <div style={{position: 'relative', transform: `scale(${s}) rotate(-8deg)`, fontFamily: '"Bangers", "Impact"', fontStyle: 'italic', fontSize: 230,
        color: '#FF3D3D', WebkitTextStroke: '12px #000', paintOrder: 'stroke fill', textShadow: '10px 10px 0 #000'}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Planche de 3 cases qui apparaissent une à une, caméra qui glisse de case en case.
- Bulle de dialogue avec texte qui s'écrit (voir `typewriter`).
- Lignes d'action derrière un personnage (voir `manga-speed-lines`).

## Pièges
- Police de BD : charger « Bangers » ou « Comic Neue » (Google Fonts), sinon Impact.
- Trame trop fine = moirage après compression : points de 14–20 px.

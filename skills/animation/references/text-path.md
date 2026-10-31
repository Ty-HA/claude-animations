# Text on Path · `/animation text-path`

> Famille : Typographie cinétique · #11

## C'est quoi
Un texte qui suit une courbe : cercle qui tourne, vague, spirale, contour d'un logo.

## Quand l'utiliser
- Badge rotatif (« NOUVEAU • NOUVEAU • »), sceau, bouton CTA.
- Texte qui serpente autour d'un objet ou d'un personnage.

## Technique clé
- SVG `<textPath href="#id">` sur un `<path>` défini dans `<defs>`.
- Défilement : animer `startOffset` (en %) : `startOffset={\`${(frame * 0.5) % 100}%\`}`, ou faire tourner tout le groupe (`rotate(frame * 0.8)`).
- Cercle en path : `M cx-r,cy a r,r 0 1,1 2r,0 a r,r 0 1,1 -2r,0`.
- Répéter le texte (`' • '.join`) pour couvrir tout le périmètre (2πr).

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

export const TextPath: React.FC<{text?: string}> = ({text = 'COMMENTE ANIMATION'}) => {
  const frame = useCurrentFrame();
  const r = 300;
  const label = Array(3).fill(text).join(' • ') + ' • ';
  return (
    <AbsoluteFill style={{background: '#010012', justifyContent: 'center', alignItems: 'center'}}>
      <svg width={800} height={800} viewBox="0 0 800 800">
        <defs>
          <path id="circ" d={`M ${400 - r},400 a ${r},${r} 0 1,1 ${2 * r},0 a ${r},${r} 0 1,1 ${-2 * r},0`} />
        </defs>
        <g transform={`rotate(${frame * 0.8} 400 400)`}>
          <text fill="#67E8F9" fontFamily="monospace" fontWeight={700} fontSize={44} letterSpacing={6}>
            <textPath href="#circ">{label}</textPath>
          </text>
        </g>
        <circle cx={400} cy={400} r={150} fill="#00B5C7" opacity={0.15} />
      </svg>
    </AbsoluteFill>
  );
};
```

## Variations
- Vague : path `M0,400 Q200,250 400,400 T800,400` + `startOffset` animé.
- Spirale : générer le path en polaire avec r qui croît.
- Texte qui s'écrit le long du path : combiner avec un masque `strokeDashoffset`.

## Pièges
- Texte plus long que le chemin : il est tronqué. Calculer `2πr` vs largeur estimée (≈ 0,6 × fontSize × caractères).
- `href` vs `xlinkHref` : les navigateurs récents acceptent `href`.

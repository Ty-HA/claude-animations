# Art Deco · `/animation art-deco`

> Famille : Esthétiques artistiques · #63

## C'est quoi
Le luxe géométrique des années 1920 (Gatsby) : lignes dorées fines, symétrie, éventails, rayons de soleil, chevrons, typographies hautes et étroites sur fond noir ou vert profond.

## Quand l'utiliser
- Événement chic, soirée, luxe, hôtellerie, invitation, annonce élégante.

## Technique clé
- Fond `#0E0E0E` ou `#0B2B26`, or : dégradé `linear-gradient(#F5E6A8, #C9A227 50%, #8C6A16)` ou couleur `#D4AF37`.
- Motif éventail : arcs concentriques + rayons depuis un point bas central.
- Cadre : double ligne dorée avec coins en escalier (`polyline`).
- Tracé symétrique : animer les lignes depuis l'axe central vers l'extérieur (`pathLength` + `strokeDashoffset`), rayons qui se déploient avec un décalage en éventail.
- Typo : « Poiret One », « Limelight », « Josefin Sans » capitales très espacées (`letterSpacing: 16`).

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

const GOLD = '#D4AF37';

export const ArtDeco: React.FC<{text?: string}> = ({text = 'THE GALA'}) => {
  const frame = useCurrentFrame();
  const p = (a: number, b: number) => interpolate(frame, [a, b], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp', easing: Easing.inOut(Easing.cubic)});
  return (
    <AbsoluteFill style={{background: '#0E0E0E'}}>
      <svg width={1080} height={1920}>
        <g stroke={GOLD} fill="none" strokeWidth={3}>
          {Array.from({length: 17}, (_, i) => {
            const a = Math.PI + (i / 16) * Math.PI;
            const k = p(i * 1.5, i * 1.5 + 20);
            return <line key={i} x1={540} y1={1150} x2={540 + Math.cos(a) * 420 * k} y2={1150 + Math.sin(a) * 420 * k} />;
          })}
          {[180, 260, 340].map((r, i) => (
            <path key={r} d={`M ${540 - r} 1150 A ${r} ${r} 0 0 1 ${540 + r} 1150`} pathLength={1} strokeDasharray={1} strokeDashoffset={1 - p(20 + i * 6, 45 + i * 6)} />
          ))}
          <path d="M140 300 L 940 300 L 940 1650 L 140 1650 Z" pathLength={1} strokeDasharray={1} strokeDashoffset={1 - p(0, 40)} />
          <path d="M165 325 L 915 325 L 915 1625 L 165 1625 Z" pathLength={1} strokeDasharray={1} strokeDashoffset={1 - p(10, 50)} strokeWidth={1.5} />
        </g>
      </svg>
      <div style={{position: 'absolute', top: 1260, width: '100%', textAlign: 'center', fontFamily: '"Poiret One", "Didot", serif', fontSize: 100, letterSpacing: 18,
        backgroundImage: 'linear-gradient(#F5E6A8, #C9A227 50%, #8C6A16)', WebkitBackgroundClip: 'text', color: 'transparent', opacity: p(50, 70)}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Chevrons dorés qui défilent verticalement en fond.
- Reflet brillant qui balaie le texte doré (bande blanche en `background-position`).

## Pièges
- Asymétrie = perd l'esprit Art Déco. Tout centré, tout miroir.
- Or plat jaune = cheap : utiliser un dégradé.

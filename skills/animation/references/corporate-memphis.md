# Corporate Memphis · `/animation corporate-memphis`

> Famille : Esthétiques artistiques · #56

## C'est quoi
Le style d'illustration « Big Tech » : personnages flat aux membres étirés, petites têtes, couleurs vives non réalistes (peau violette, bleue…), formes simples, poses dynamiques.

## Quand l'utiliser
- Explainers SaaS, onboarding, illustrations d'équipe, présentation de service (avec un clin d'œil assumé).

## Technique clé
- Personnage en formes simples SVG : tête ronde petite, torse trapèze, bras et jambes = **paths épais à bouts ronds** (`strokeWidth` 40–60, `strokeLinecap: round`) → faciles à animer.
- Animation par **rotation des membres** autour des articulations (`rotate(angle, x, y)`) avec sinus : marche, coucou, saut.
- Couleurs plates, aucune ombre ou une seule ombre plate. Palette : `#6C63FF`, `#FF6584`, `#F9A826`, `#2EC4B6`, peau `#8C7AE6`.
- Décor : plantes en blobs, formes abstraites flottantes.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

export const CorporateMemphis: React.FC<{text?: string}> = ({text = 'On collabore !'}) => {
  const t = useCurrentFrame() / 30;
  const wave = Math.sin(t * 6) * 25;
  const step = Math.sin(t * 4) * 18;
  return (
    <AbsoluteFill style={{background: '#FFF7EC'}}>
      <svg width={1080} height={1920}>
        <ellipse cx={540} cy={1560} rx={360} ry={40} fill="#FFE1C2" />
        <path d="M200 1500 C 150 1300, 300 1250, 260 1100 C 360 1200, 330 1400, 280 1500 Z" fill="#2EC4B6" />
        <g strokeLinecap="round" fill="none">
          <path d={`M520 1180 L ${470 - step} 1540`} stroke="#3D3D6B" strokeWidth={60} />
          <path d={`M560 1180 L ${610 + step} 1540`} stroke="#3D3D6B" strokeWidth={60} />
          <path d="M460 900 L 620 900 L 600 1200 L 480 1200 Z" fill="#FF6584" stroke="none" />
          <path d="M470 930 L 360 1130" stroke="#8C7AE6" strokeWidth={44} />
          <g transform={`rotate(${-150 + wave} 610 930)`}><path d="M610 930 L 830 930" stroke="#8C7AE6" strokeWidth={44} /></g>
          <line x1={540} y1={860} x2={540} y2={900} stroke="#8C7AE6" strokeWidth={40} />
        </g>
        <circle cx={540} cy={820} r={56} fill="#8C7AE6" />
        <path d="M484 800 Q 540 720 600 800" fill="#1E1E3F" />
      </svg>
      <div style={{position: 'absolute', top: 360, width: '100%', textAlign: 'center', fontFamily: 'Poppins, sans-serif', fontWeight: 800, fontSize: 90, color: '#3D3D6B'}}>{text}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Scène d'équipe : 3 personnages qui se passent un objet (idée = ampoule).
- Personnages qui sortent d'un écran de laptop géant.

## Pièges
- Rotations autour du mauvais point = membres qui se détachent. Toujours `rotate(a, xArticulation, yArticulation)`.
- Ombres réalistes ou dégradés = hors style.

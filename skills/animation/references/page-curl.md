# Page Curl · `/animation page-curl`

> Famille : Transitions & caméra · #92

## C'est quoi
Une page qui se tourne comme dans un livre ou un magazine pour passer à la suivante.

## Quand l'utiliser
- Storytelling façon livre, carrousel, catalogue, recette, journal de bord.

## Technique clé
- **Version 3D simple** : la page pivote autour de son bord gauche : `transformOrigin: 'left center'`, `rotateY(0 → -180deg)` avec `perspective: 2500px`. Recto/verso via `backfaceVisibility`.
- Ombre dynamique : dégradé sombre sur la page dessous dont l'opacité suit `sin(π p)`, et dégradé sur la page qui tourne (pli).
- Courbure (curl réaliste) : découper la page en 10–20 bandes verticales qui tournent chacune un peu plus (`angle_i = p * 180 * (1 + i * 0.04)`, clampé) → effet de courbe.
- Durée 20–30 frames, `Easing.inOut(Easing.sin)`.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

const Page: React.FC<{bg: string; label: string}> = ({bg, label}) => (
  <AbsoluteFill style={{background: bg, justifyContent: 'center', alignItems: 'center', fontFamily: 'Georgia, serif', fontSize: 110, color: '#2b2118'}}>{label}</AbsoluteFill>
);

export const PageCurl: React.FC<{text?: string; at?: number}> = ({text = 'Chapitre 2', at = 15}) => {
  const frame = useCurrentFrame();
  const p = interpolate(frame, [at, at + 28], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp', easing: Easing.inOut(Easing.sin)});
  const shade = Math.sin(Math.PI * p);
  return (
    <AbsoluteFill style={{background: '#1a1410', perspective: 2500}}>
      <Page bg="#F3E9D2" label={text} />
      <AbsoluteFill style={{background: `linear-gradient(90deg, rgba(0,0,0,${0.45 * shade}), transparent 60%)`}} />
      <AbsoluteFill style={{transformOrigin: 'left center', transformStyle: 'preserve-3d', transform: `rotateY(${-180 * p}deg)`}}>
        <AbsoluteFill style={{backfaceVisibility: 'hidden'}}>
          <Page bg="#FBF5E6" label="Chapitre 1" />
          <AbsoluteFill style={{background: `linear-gradient(270deg, rgba(0,0,0,${0.35 * shade}), transparent 40%)`}} />
        </AbsoluteFill>
        <AbsoluteFill style={{backfaceVisibility: 'hidden', transform: 'rotateY(180deg)', background: '#E8DCC0'}} />
      </AbsoluteFill>
    </AbsoluteFill>
  );
};
```

## Variations
- Coin qui se soulève (curl partiel) en teaser avant de tourner.
- Livre ouvert en double page (format paysage).
- Flipbook rapide : 10 pages tournées en 1 seconde.

## Pièges
- `perspective` trop faible = déformation extrême. 2000–3000 px.
- Pas de verso défini = page transparente quand elle se retourne.

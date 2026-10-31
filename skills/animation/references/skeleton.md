# Skeleton Loading · `/animation skeleton`

> Famille : Tableaux de bord & UI · #71

## C'est quoi
Des formes grises qui pulsent ou scintillent (shimmer) à la place du contenu pendant le chargement, puis le vrai contenu apparaît.

## Quand l'utiliser
- Mockups d'app réalistes, transition « chargement → résultat », tease de contenu.

## Technique clé
- Blocs gris `#E5E7EB` (ou `#1F2937` en sombre) avec rayons, aux dimensions du futur contenu (avatar rond, lignes de texte de longueurs variées, image).
- **Shimmer** : `linear-gradient(90deg, transparent, rgba(255,255,255,.6), transparent)` large de 40 %, `translateX(-100% → 200%)` en boucle de 40 frames.
- Ou **pulse** : opacité `0.6 + 0.4 * sin(frame * 0.15)`.
- Transition vers le contenu : fondu croisé 10 frames, contenu qui monte de 10 px.

## Snippet Remotion
```tsx
import {AbsoluteFill, interpolate, useCurrentFrame} from 'remotion';

export const Skeleton: React.FC<{text?: string; loadAt?: number}> = ({text = 'Ty — BTHA System', loadAt = 70}) => {
  const frame = useCurrentFrame();
  const shimmer = ((frame % 40) / 40) * 300 - 100;
  const loaded = interpolate(frame, [loadAt, loadAt + 10], [0, 1], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp'});
  const bone = (w: number | string, h: number, r = 14): React.CSSProperties => ({width: w, height: h, borderRadius: r, background: '#E5E7EB', position: 'relative', overflow: 'hidden'});
  const Shine = () => <div style={{position: 'absolute', inset: 0, width: '40%', transform: `translateX(${shimmer}%)`,
    background: 'linear-gradient(90deg, transparent, rgba(255,255,255,.7), transparent)'}} />;
  return (
    <AbsoluteFill style={{background: '#fff', padding: '400px 140px', fontFamily: 'Inter, sans-serif'}}>
      <div style={{position: 'absolute', left: 140, right: 140, top: 400, opacity: 1 - loaded}}>
        <div style={{display: 'flex', gap: 30, alignItems: 'center'}}><div style={bone(140, 140, 70)}><Shine /></div>
          <div style={{flex: 1, display: 'flex', flexDirection: 'column', gap: 18}}><div style={bone('80%', 34)}><Shine /></div><div style={bone('50%', 28)}><Shine /></div></div></div>
        <div style={{...bone('100%', 500, 28), marginTop: 50}}><Shine /></div>
        {[100, 92, 70].map((w) => <div key={w} style={{...bone(`${w}%`, 28), marginTop: 24}}><Shine /></div>)}
      </div>
      <div style={{position: 'absolute', left: 140, right: 140, top: 400 + 10 * (1 - loaded), opacity: loaded}}>
        <div style={{display: 'flex', gap: 30, alignItems: 'center'}}>
          <div style={{width: 140, height: 140, borderRadius: 70, background: 'linear-gradient(135deg,#00B5C7,#A855F7)'}} />
          <div style={{fontSize: 44, fontWeight: 800}}>{text}</div>
        </div>
        <div style={{height: 500, borderRadius: 28, marginTop: 50, background: 'linear-gradient(135deg,#010012,#155E75)'}} />
        <div style={{fontSize: 34, marginTop: 24, lineHeight: 1.5, color: '#374151'}}>Le contenu est arrivé. Le squelette a fait son job.</div>
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Grille de cartes produit en skeleton qui se remplissent une par une.
- Version sombre avec shimmer cyan très subtil.

## Pièges
- Skeleton qui ne correspond pas à la forme du contenu final = saut visuel désagréable.
- Shimmer trop rapide = agressif. 1–1,5 s par passage.

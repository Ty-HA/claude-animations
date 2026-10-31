# Frutiger Aero · `/animation frutiger-aero`

> Famille : Esthétiques artistiques · #55

## C'est quoi
L'esthétique optimiste des années 2005–2012 (Windows Vista/7, iOS 1–6) : verre bleu-vert brillant, bulles d'eau, ciel bleu, herbe verte, reflets glossy, poissons et nature « tech ».

## Quand l'utiliser
- Nostalgie Y2K/2000s, écologie + tech, ambiance fraîche et positive.

## Technique clé
- Ciel `linear-gradient(#4FC3F7, #B3E5FC 60%, #E8F5E9)` + herbe `#7CB342`.
- **Bouton glossy** : fond `linear-gradient(#5EE7FF, #0288D1)` + reflet supérieur `linear-gradient(rgba(255,255,255,.85), rgba(255,255,255,.1))` sur la moitié haute, `borderRadius: 999`, ombre douce bleue.
- **Bulles** : cercles `radial-gradient(circle at 30% 30%, #fff 0 8%, rgba(255,255,255,.15) 30%, rgba(79,195,247,.25) 70%, rgba(255,255,255,.6))` qui montent en oscillant.
- Typo : Segoe UI / Frutiger / Myriad, blanc avec ombre bleutée.

## Snippet Remotion
```tsx
import {AbsoluteFill, random, useCurrentFrame} from 'remotion';

export const FrutigerAero: React.FC<{text?: string}> = ({text = 'Bienvenue'}) => {
  const frame = useCurrentFrame();
  const t = frame / 30;
  return (
    <AbsoluteFill style={{background: 'linear-gradient(#29B6F6, #B3E5FC 55%, #E8F5E9 70%, #8BC34A 70%, #558B2F)'}}>
      {Array.from({length: 26}, (_, i) => {
        const size = 40 + random(`s${i}`) * 160;
        const y = ((random(`y${i}`) * 2200 - t * (40 + random(`v${i}`) * 80)) % 2200 + 2200) % 2200 - 150;
        return <div key={i} style={{position: 'absolute', left: random(`x${i}`) * 1000 + Math.sin(t * 1.5 + i) * 25, top: y, width: size, height: size, borderRadius: '50%',
          background: 'radial-gradient(circle at 30% 30%, #fff 0 8%, rgba(255,255,255,.15) 30%, rgba(79,195,247,.25) 70%, rgba(255,255,255,.6))',
          border: '1px solid rgba(255,255,255,.6)'}} />;
      })}
      <AbsoluteFill style={{justifyContent: 'center', alignItems: 'center'}}>
        <div style={{position: 'relative', padding: '40px 110px', borderRadius: 999, overflow: 'hidden',
          background: 'linear-gradient(#5EE7FF, #0288D1)', boxShadow: '0 12px 40px rgba(2,136,209,.5), inset 0 -6px 16px rgba(0,0,0,.15)',
          fontFamily: '"Segoe UI", "Frutiger", sans-serif', fontSize: 80, color: '#fff', textShadow: '0 2px 6px rgba(0,60,120,.5)'}}>
          <div style={{position: 'absolute', left: 20, right: 20, top: 6, height: '48%', borderRadius: 999, background: 'linear-gradient(rgba(255,255,255,.85), rgba(255,255,255,.1))'}} />
          <span style={{position: 'relative'}}>{text}</span>
        </div>
      </AbsoluteFill>
    </AbsoluteFill>
  );
};
```

## Variations
- Fenêtre Aero : panneau verre bleuté avec barre de titre et boutons ronds brillants.
- Poissons tropicaux et feuilles qui flottent, aurores vertes.
- Orbe glossy (icône sphérique avec reflet).

## Pièges
- Sans reflet glossy, ça devient du flat moderne : le reflet est la signature.
- Couleurs trop sombres ou désaturées : rester lumineux et bleu-vert.

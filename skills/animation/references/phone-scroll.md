# Phone Mockup Scroll · `/animation phone-scroll`

> Famille : Tableaux de bord & UI · #78

## C'est quoi
Un écran d'app ou de site qui défile à l'intérieur d'un téléphone (mockup), parfois en légère perspective 3D.

## Quand l'utiliser
- Présenter une app, un site, un portfolio, une boutique, un feed.

## Technique clé
- Cadre téléphone : `div` 600×1240, `borderRadius: 90`, bordure noire 18 px, encoche/Dynamic Island (pilule noire en haut), reflet léger.
- Contenu : capture d'écran longue (`<Img>` pleine largeur) ou interface codée ; `translateY(-scroll)` avec scroll par **paliers** (scroll → pause → scroll) via `interpolate` multi-points + `Easing.inOut(Easing.cubic)`.
- Perspective : `perspective(2000px) rotateY(-18deg) rotateX(6deg)` qui revient à 0 pendant l'intro.
- Ombre portée large `0 60px 120px rgba(0,0,0,.5)`.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, Img, interpolate, spring, useCurrentFrame, useVideoConfig} from 'remotion';

export const PhoneScroll: React.FC<{text?: string; screenshot?: string}> = ({text = 'Mon app', screenshot}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const intro = spring({frame, fps, config: {damping: 18}});
  const scroll = interpolate(frame, [30, 55, 80, 105, 130, 155], [0, 700, 700, 1400, 1400, 2100], {extrapolateLeft: 'clamp', extrapolateRight: 'clamp', easing: Easing.inOut(Easing.cubic)});
  return (
    <AbsoluteFill style={{background: 'radial-gradient(circle at 50% 40%, #1e293b, #020617 70%)', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{width: 600, height: 1240, borderRadius: 90, border: '18px solid #0a0a0a', background: '#fff', overflow: 'hidden', position: 'relative',
        boxShadow: '0 60px 120px rgba(0,0,0,.5), 0 0 0 3px #333', transform: `perspective(2000px) rotateY(${-18 * (1 - intro)}deg) rotateX(${6 * (1 - intro)}deg) scale(${0.9 + 0.1 * intro})`}}>
        <div style={{transform: `translateY(${-scroll}px)`}}>
          {screenshot ? <Img src={screenshot} style={{width: '100%'}} /> : Array.from({length: 10}, (_, i) => (
            <div key={i} style={{margin: 24, height: 300, borderRadius: 28, background: `linear-gradient(135deg, hsl(${190 + i * 15} 80% 55%), hsl(${270 + i * 10} 70% 60%))`,
              display: 'flex', alignItems: 'flex-end', padding: 24, color: '#fff', fontFamily: 'Inter', fontSize: 36, fontWeight: 800}}>{text} #{i + 1}</div>
          ))}
        </div>
        <div style={{position: 'absolute', top: 18, left: 210, width: 180, height: 50, borderRadius: 25, background: '#0a0a0a'}} />
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Doigt/curseur qui « tape » sur un élément (voir `cursor-demo`).
- 3 téléphones en éventail avec des écrans différents.
- Laptop mockup pour un site desktop.

## Pièges
- Utiliser `<Img>` de Remotion (pas `<img>`) pour une vraie capture, afin d'attendre le chargement au rendu.
- Scroll continu sans pause = on ne lit rien. Toujours des paliers.

# Neobrutalism · `/animation neobrutalism`

> Famille : Esthétiques artistiques · #48

## C'est quoi
Le style web brut et assumé : contours noirs épais, ombres dures décalées (sans flou), couleurs criardes, typographie grasse. Ça claque.

## Quand l'utiliser
- Startups fun, SaaS jeunes, mèmes, contenus énergiques et décalés.

## Technique clé
- Bloc : `border: 4px solid #000`, `boxShadow: '8px 8px 0 #000'`, `borderRadius: 12` (ou 0), fonds `#FFDE59`, `#FF6B6B`, `#7AF0C6`, `#A0A0FF`.
- Interaction animée : au « clic », l'élément se déplace de `translate(8px, 8px)` et l'ombre passe à `0 0 0` (en 4 frames).
- Entrées : `spring` très nerveux (`damping: 9`), légère rotation de ±3°.
- Typo : Space Grotesk / Archivo Black, gros, noir.

## Snippet Remotion
```tsx
import {AbsoluteFill, spring, useCurrentFrame, useVideoConfig} from 'remotion';

const Box: React.FC<{bg: string; delay: number; rot: number; children: React.ReactNode}> = ({bg, delay, rot, children}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const s = spring({frame: frame - delay, fps, config: {damping: 9, stiffness: 180}});
  const pressed = frame > delay + 30 && frame < delay + 36;
  return (
    <div style={{background: bg, border: '5px solid #000', borderRadius: 14, padding: '34px 46px',
      boxShadow: pressed ? '0 0 0 #000' : '10px 10px 0 #000', fontFamily: '"Archivo Black", "Arial Black"', fontSize: 70, color: '#000',
      transform: `translate(${pressed ? 10 : 0}px, ${pressed ? 10 : 0}px) scale(${s}) rotate(${rot * s}deg)`}}>{children}</div>
  );
};

export const Neobrutalism: React.FC<{text?: string}> = ({text = 'CLIQUE ICI'}) => (
  <AbsoluteFill style={{background: '#FFF4E0', justifyContent: 'center', alignItems: 'center', gap: 50,
    backgroundImage: 'radial-gradient(#000 1.5px, transparent 1.5px)', backgroundSize: '36px 36px'}}>
    <Box bg="#FFDE59" delay={0} rot={-3}>NO BS.</Box>
    <Box bg="#7AF0C6" delay={6} rot={2}>JUSTE DU STYLE</Box>
    <Box bg="#FF6B6B" delay={12} rot={-1}>{text} →</Box>
  </AbsoluteFill>
);
```

## Variations
- Fenêtres d'OS rétro brutales (barre de titre + boutons carrés).
- Stickers qui s'empilent (voir `sticker-pop`).
- Marquee de bandeaux colorés avec contours.

## Pièges
- Ombre floue = ce n'est plus brutal. `blur` = 0 toujours.
- Trop de couleurs : 3–4 max + noir.

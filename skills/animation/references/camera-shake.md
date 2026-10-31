# Camera Shake · `/animation camera-shake`

> Famille : Transitions & caméra · #84

## C'est quoi
Un tremblement de caméra à l'impact (explosion, chute, basse qui frappe) ou un léger tremblement permanent « caméra à l'épaule ».

## Quand l'utiliser
- Impact, tension, action, effet « séisme », drop musical ; version douce pour du réalisme.

## Technique clé
- Bruit lisse (pas `random` brut à chaque frame) : somme de sinus à fréquences non multiples, ou `noise2D` de `@remotion/noise` : `noise2D('x', frame * 0.3, 0)`.
- Amplitude qui décroît après l'impact : `A = A0 * exp(-(frame - impact) / 6)`.
- Translation ±30 px + rotation ±2° ; légère surscale (`1.06`) pour ne pas voir les bords.
- Handheld permanent : amplitude 4–8 px, fréquence lente (0.05).

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';
import {noise2D} from '@remotion/noise';

export const CameraShake: React.FC<{text?: string; impact?: number}> = ({text = 'IMPACT', impact = 20}) => {
  const frame = useCurrentFrame();
  const since = frame - impact;
  const A = since < 0 ? 4 : 4 + 40 * Math.exp(-since / 6);
  const f = since < 0 ? 0.05 : 0.45;
  const x = noise2D('x', frame * f, 0) * A;
  const y = noise2D('y', frame * f, 0) * A;
  const r = noise2D('r', frame * f, 0) * A * 0.06;
  return (
    <AbsoluteFill style={{background: '#000', overflow: 'hidden'}}>
      <AbsoluteFill style={{transform: `translate(${x}px, ${y}px) rotate(${r}deg) scale(1.06)`,
        background: 'radial-gradient(circle at 50% 50%, #FF7A00, #3b0a00 70%)', justifyContent: 'center', alignItems: 'center'}}>
        <div style={{fontFamily: 'Impact', fontSize: 220, color: '#fff'}}>{text}</div>
      </AbsoluteFill>
    </AbsoluteFill>
  );
};
```
Dépendance : `npm i @remotion/noise`.

## Variations
- Shake uniquement vertical (pas de course, battement de cœur).
- Shake + flou de mouvement léger pendant le pic.
- Shake piloté par l'amplitude audio (`visualizeAudio`).

## Pièges
- `random()` par frame = tremblement stroboscopique, pas réaliste. Utiliser du bruit lisse.
- Sans surscale, les bords noirs apparaissent.

# Count-up / Odometer · `/animation odometer`

> Famille : Tableaux de bord & UI · #67

## C'est quoi
Un compteur qui défile jusqu'à sa valeur finale. Version odomètre : chaque chiffre est un rouleau vertical qui tourne, comme un compteur kilométrique.

## Quand l'utiliser
- Chiffre clé (abonnés, CA, km courus, vues), compte à rebours, score.

## Technique clé
- **Count-up simple** : `n = interpolate(frame, [0, 45], [0, cible], {easing: Easing.out(Easing.cubic)})`, affiché avec `Math.round` et séparateurs (`toLocaleString('fr-FR')`).
- **Odomètre** : pour chaque chiffre de rang `k`, valeur continue `v = n / 10^k`, position du rouleau `translateY(-(v % 10) * h)`. Le rouleau contient `0..9,0` empilés (le 0 final pour boucler).
- Fenêtre `overflow: hidden`, hauteur = 1 chiffre. Chiffres tabulaires (`fontVariantNumeric: 'tabular-nums'`).
- Fin : petit `scale 1 → 1.08 → 1` + flash couleur.

## Snippet Remotion
```tsx
import {AbsoluteFill, Easing, interpolate, useCurrentFrame} from 'remotion';

const H = 200;

const Digit: React.FC<{value: number; place: number}> = ({value, place}) => {
  const v = (value / Math.pow(10, place)) % 10;
  return (
    <div style={{height: H, overflow: 'hidden'}}>
      <div style={{transform: `translateY(${-v * H}px)`}}>
        {[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 0].map((d, i) => <div key={i} style={{height: H, lineHeight: `${H}px`}}>{d}</div>)}
      </div>
    </div>
  );
};

export const Odometer: React.FC<{text?: string; target?: number}> = ({text = 'abonnés', target = 48250}) => {
  const frame = useCurrentFrame();
  const n = interpolate(frame, [0, 60], [0, target], {extrapolateRight: 'clamp', easing: Easing.out(Easing.cubic)});
  const places = String(target).length;
  return (
    <AbsoluteFill style={{background: '#010012', justifyContent: 'center', alignItems: 'center', color: '#fff', fontFamily: 'Inter, sans-serif'}}>
      <div style={{display: 'flex', fontSize: 180, fontWeight: 800, fontVariantNumeric: 'tabular-nums', textShadow: '0 0 30px rgba(0,181,199,.6)'}}>
        {Array.from({length: places}, (_, i) => <Digit key={i} value={n} place={places - 1 - i} />)}
      </div>
      <div style={{fontSize: 48, color: '#67E8F9', letterSpacing: 8, marginTop: 20}}>{text.toUpperCase()}</div>
    </AbsoluteFill>
  );
};
```

## Variations
- Rouleaux qui s'arrêtent un par un de droite à gauche (machine à sous).
- Compte à rebours : `interpolate(frame, [0, 90], [10, 0])`.
- Préfixe/suffixe (€, km, %) fixes à côté des rouleaux.

## Pièges
- Valeur continue (non arrondie) pour les rouleaux = défilement fluide ; pour un count-up texte, arrondir.
- Police proportionnelle = chiffres qui dansent. `tabular-nums`.

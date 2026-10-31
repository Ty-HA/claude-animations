# Notification Stack · `/animation notifications`

> Famille : Tableaux de bord & UI · #77

## C'est quoi
Des notifications (iOS/Android) qui tombent du haut de l'écran et s'empilent rapidement : ventes, likes, messages… effet « ça explose ».

## Quand l'utiliser
- Preuve sociale (« Nouvelle commande » ×20), succès d'un lancement, hook de vidéo.

## Technique clé
- Carte notif : verre dépoli (`rgba(30,30,30,.75)` + `backdropFilter: blur(30px)`), rayon 36 px, icône d'app 80 px, titre gras + texte + « maintenant ».
- Chaque notif `i` arrive à `at = i * intervalle` (intervalle qui **diminue** pour l'accélération : `at_i = 40 * (1 - 0.85^i) / 0.15`).
- Entrée : `translateY(-200 → 0)` + `scale 0.9 → 1` en spring.
- Empilement : les notifs existantes descendent d'une case (`y = index depuis la plus récente * 170`) et rétrécissent légèrement en profondeur.

## Snippet Remotion
```tsx
import {AbsoluteFill, spring, useCurrentFrame, useVideoConfig} from 'remotion';

const N = 12;
const AT = Array.from({length: N}, (_, i) => Math.round((40 * (1 - Math.pow(0.82, i))) / 0.18));

export const Notifications: React.FC<{text?: string}> = ({text = 'Nouvelle commande · 49,00 €'}) => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const arrived = AT.filter((a) => frame >= a).length;
  return (
    <AbsoluteFill style={{background: 'linear-gradient(160deg,#4c1d95,#0f172a)', fontFamily: '-apple-system, Inter, sans-serif'}}>
      {AT.map((at, i) => {
        if (frame < at) return null;
        const s = spring({frame: frame - at, fps, config: {damping: 15, stiffness: 200}});
        const depth = arrived - 1 - i;
        return (
          <div key={i} style={{position: 'absolute', left: 120, right: 120, top: 260 + depth * 170, height: 150, borderRadius: 36,
            background: 'rgba(30,30,30,.78)', backdropFilter: 'blur(30px)', display: 'flex', alignItems: 'center', gap: 26, padding: '0 30px',
            transform: `translateY(${(1 - s) * -200}px) scale(${(0.9 + 0.1 * s) * (1 - Math.min(depth, 6) * 0.01)})`, opacity: depth > 8 ? 0 : 1, color: '#fff'}}>
            <div style={{width: 80, height: 80, borderRadius: 20, background: 'linear-gradient(135deg,#22C55E,#15803D)', fontSize: 48, display: 'flex', justifyContent: 'center', alignItems: 'center'}}>$</div>
            <div style={{flex: 1}}>
              <div style={{display: 'flex', justifyContent: 'space-between', fontSize: 30, fontWeight: 700}}><span>Boutique</span><span style={{opacity: 0.5, fontWeight: 400}}>maintenant</span></div>
              <div style={{fontSize: 32, opacity: 0.9}}>{text}</div>
            </div>
          </div>
        );
      })}
    </AbsoluteFill>
  );
};
```

## Variations
- Notifications d'apps différentes (likes, DM, mails) avec icônes variées.
- Pile qui finit par remplir tout l'écran puis « explose » en confettis.
- Son « ding » à chaque arrivée.

## Pièges
- Montants/chiffres inventés présentés comme réels = trompeur : préciser « exemple » si ce n'est pas vrai.
- Logos de marques réelles : attention aux droits.

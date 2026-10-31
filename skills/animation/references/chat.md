# Chat / iMessage Simulation · `/animation chat`

> Famille : Tableaux de bord & UI · #76

## C'est quoi
Une conversation qui se déroule à l'écran : bulles qui apparaissent une à une, indicateur « … en train d'écrire », sons de notification.

## Quand l'utiliser
- Storytelling (dialogue drôle, client/prestataire), démo de chatbot, prompt → réponse de Claude.

## Technique clé
- Messages `{from: 'me' | 'them', text, at}` ; `me` à droite en bleu `#0A84FF`, `them` à gauche en gris `#E9E9EB` (ou sombre `#26252A`).
- Avant chaque message de `them` : bulle « typing » (3 points qui pulsent en décalé : `opacity 0.3 + 0.7 * sin(...)`) pendant 20–30 frames.
- Apparition : `spring` sur `scale 0.6 → 1` avec `transformOrigin` en bas du côté de l'expéditeur + translateY.
- Défilement : quand la pile dépasse, `translateY` du conteneur pour garder le dernier message visible.
- Bulles `borderRadius: 40` avec coin réduit côté queue.

## Snippet Remotion
```tsx
import {AbsoluteFill, spring, useCurrentFrame, useVideoConfig} from 'remotion';

type Msg = {from: 'me' | 'them'; text: string; at: number};
const MSGS: Msg[] = [
  {from: 'me', text: 'Tu peux me faire une anim glitch ?', at: 5},
  {from: 'them', text: 'Bien sûr ! /animation glitch', at: 45},
  {from: 'me', text: 'En 9:16 pour TikTok 🙏', at: 75},
  {from: 'them', text: 'C\'est rendu. 1080×1920, 30 i/s ✨', at: 115},
];

export const Chat: React.FC<{text?: string}> = () => {
  const frame = useCurrentFrame();
  const {fps} = useVideoConfig();
  const typing = MSGS.find((m) => m.from === 'them' && frame >= m.at - 25 && frame < m.at);
  return (
    <AbsoluteFill style={{background: '#000', padding: '300px 140px', fontFamily: '-apple-system, "SF Pro Text", Inter, sans-serif'}}>
      <div style={{display: 'flex', flexDirection: 'column', gap: 18}}>
        {MSGS.filter((m) => frame >= m.at).map((m, i) => {
          const s = spring({frame: frame - m.at, fps, config: {damping: 14, stiffness: 220}});
          const me = m.from === 'me';
          return (
            <div key={i} style={{alignSelf: me ? 'flex-end' : 'flex-start', maxWidth: '78%', padding: '22px 30px', fontSize: 40, lineHeight: 1.3,
              borderRadius: 40, [me ? 'borderBottomRightRadius' : 'borderBottomLeftRadius']: 10, background: me ? '#0A84FF' : '#26252A', color: '#fff',
              transform: `translateY(${(1 - s) * 30}px) scale(${0.6 + 0.4 * s})`, transformOrigin: me ? 'bottom right' : 'bottom left', opacity: s}}>{m.text}</div>
          );
        })}
        {typing && (
          <div style={{alignSelf: 'flex-start', padding: '26px 30px', borderRadius: 40, background: '#26252A', display: 'flex', gap: 10}}>
            {[0, 1, 2].map((k) => <div key={k} style={{width: 18, height: 18, borderRadius: 9, background: '#8E8E93', opacity: 0.3 + 0.7 * Math.abs(Math.sin((frame - k * 4) / 6))}} />)}
          </div>
        )}
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- En-tête iMessage (nom, avatar) et fond clair iOS.
- Version WhatsApp (fond motif, bulles vertes, coches bleues).
- Réactions (❤️ qui pop sur une bulle).

## Pièges
- Messages trop rapprochés : laisser ~1 s de lecture par ligne de texte.
- Ne pas reproduire l'interface exacte d'une marque pour faire passer un faux échange pour vrai.

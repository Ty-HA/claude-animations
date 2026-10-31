# Typewriter · `/animation typewriter`

> Famille : Typographie cinétique · #8

## C'est quoi
Le texte apparaît lettre par lettre comme sur une machine à écrire, avec un curseur qui clignote.

## Quand l'utiliser
- Prompt d'IA, question, citation, réponse de chatbot.
- Rythme calme, narratif.

## Technique clé
- `visibleChars = Math.floor(frame / framesPerChar)` avec 1 à 2 frames par caractère (30 i/s).
- Ajouter des **pauses naturelles** après la ponctuation (`,` = +6 frames, `.` = +12 frames) en précalculant une table de timings.
- Curseur : `opacity = Math.floor(frame / 15) % 2` (clignote toutes les 0,5 s), et plein pendant la frappe.
- Ne jamais couper une chaîne avec `substring` sur des emojis : utiliser `Array.from(text)`.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

export const Typewriter: React.FC<{text?: string; speed?: number}> = ({text = 'Crée-moi une animation glitch, façon cyberpunk.', speed = 2}) => {
  const frame = useCurrentFrame();
  const chars = Array.from(text);
  // Table de temps : pauses après la ponctuation
  const times: number[] = [];
  let t = 0;
  chars.forEach((c) => { times.push(t); t += speed + (c === ',' ? 6 : c === '.' ? 12 : 0); });
  const n = times.filter((x) => x <= frame).length;
  const typing = n < chars.length;
  return (
    <AbsoluteFill style={{background: '#0d0d12', justifyContent: 'center', padding: 140}}>
      <div style={{fontFamily: '"JetBrains Mono", monospace', fontSize: 56, lineHeight: 1.4, color: '#E8E8F0'}}>
        {chars.slice(0, n).join('')}
        <span style={{display: 'inline-block', width: 28, height: 60, marginLeft: 4, verticalAlign: 'middle',
          background: '#00B5C7', opacity: typing || Math.floor(frame / 15) % 2 === 0 ? 1 : 0}} />
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Fautes de frappe : taper 2 lettres fausses, les effacer, puis continuer.
- Son de touche (un clic court par caractère) via `<Audio>` dans des `<Sequence>`.
- Style machine ancienne : police Courier, papier crème, légère rotation aléatoire par lettre.

## Pièges
- Texte long = trop lent : au-delà de 80 caractères, accélérer à 1 frame/caractère.
- Le retour à la ligne fait sauter le texte : fixer la largeur du bloc dès le départ.

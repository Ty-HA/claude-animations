# Code Typing / IDE · `/animation code-typing`

> Famille : Tableaux de bord & UI · #79

## C'est quoi
Du code qui s'écrit tout seul dans un éditeur (VS Code, terminal) avec coloration syntaxique, curseur et numéros de ligne.

## Quand l'utiliser
- Tutos dev, « voici le prompt / le code que Claude a écrit », démo de skill, ambiance tech.

## Technique clé
- Tokeniser le code en segments colorés `[texte, couleur]` (à la main, ou via `shiki` / `prismjs` en amont).
- Nombre de caractères visibles : `Math.floor(frame * charsPerFrame)` (1.5–3 car/frame). Parcourir les segments jusqu'à atteindre ce nombre.
- Curseur bloc ou barre clignotante à la fin du texte visible.
- Fenêtre : barre de titre avec 3 ronds (`#FF5F56 #FFBD2E #27C93F`), fond `#1E1E2E`, police « JetBrains Mono », numéros de ligne gris.
- Thème : mots-clés `#C678DD`, chaînes `#98C379`, fonctions `#61AFEF`, nombres `#D19A66`, commentaires `#5C6370`.

## Snippet Remotion
```tsx
import {AbsoluteFill, useCurrentFrame} from 'remotion';

const K = '#C678DD', S = '#98C379', F = '#61AFEF', T = '#E5C07B', P = '#ABB2BF';
const CODE: [string, string][] = [
  ['export const ', K], ['Hook', F], [' = () => {\n', P],
  ['  const ', K], ['frame', T], [' = ', P], ['useCurrentFrame', F], ['();\n', P],
  ['  return ', K], ['<Glitch ', F], ['text=', T], ['"CLAUDE"', S], [' />;\n', F],
  ['};', P],
];

export const CodeTyping: React.FC<{text?: string; speed?: number}> = ({speed = 1.6}) => {
  const frame = useCurrentFrame();
  let left = Math.floor(frame * speed);
  const visible = CODE.map(([t, c]) => {
    const part = t.slice(0, Math.max(0, left));
    left -= t.length;
    return [part, c] as const;
  });
  const lines = visible.map(([t]) => t).join('').split('\n').length;
  return (
    <AbsoluteFill style={{background: '#0d0d14', justifyContent: 'center', alignItems: 'center'}}>
      <div style={{width: 860, borderRadius: 24, overflow: 'hidden', background: '#1E1E2E', boxShadow: '0 40px 80px rgba(0,0,0,.5)'}}>
        <div style={{height: 64, background: '#181825', display: 'flex', alignItems: 'center', gap: 14, paddingLeft: 26}}>
          {['#FF5F56', '#FFBD2E', '#27C93F'].map((c) => <div key={c} style={{width: 22, height: 22, borderRadius: 11, background: c}} />)}
        </div>
        <div style={{display: 'flex', padding: 30, fontFamily: '"JetBrains Mono", monospace', fontSize: 34, lineHeight: 1.6}}>
          <div style={{color: '#45475A', textAlign: 'right', paddingRight: 26}}>{Array.from({length: lines}, (_, i) => <div key={i}>{i + 1}</div>)}</div>
          <pre style={{margin: 0, fontFamily: 'inherit', whiteSpace: 'pre-wrap'}}>
            {visible.map(([t, c], i) => <span key={i} style={{color: c}}>{t}</span>)}
            <span style={{background: '#67E8F9', opacity: Math.floor(frame / 15) % 2 ? 0 : 1}}>&nbsp;</span>
          </pre>
        </div>
      </div>
    </AbsoluteFill>
  );
};
```

## Variations
- Terminal : prompt `$`, commande tapée, puis sortie qui s'affiche d'un coup.
- Diff : lignes rouges supprimées, lignes vertes ajoutées.
- Autocomplétion : suggestion grise qui apparaît puis est acceptée d'un coup.

## Pièges
- Code trop long : 6–12 lignes max lisibles en 9:16.
- Tabulations : les remplacer par des espaces pour un alignement stable.

# /animation — 100 styles d'animation pour Claude

> Par **Ty — BTHA System** · [bthasystem.io](https://bthasystem.io)

Tu as commenté **ANIMATION** ? Le voilà. 🚀

Un skill pour Claude qui connaît **100 styles d'animation** : glitch, synthwave, néon, particules, typographie cinétique, data viz, transitions de caméra… Tu tapes le nom du style, Claude lit la fiche technique et te code l'animation (vidéo Remotion 9:16 prête pour TikTok/Reels, ou animation web).

```
/animation glitch "BTHA SYSTEM"
/animation synthwave
/animation odometer "48 250 abonnés"
```

Chaque style a sa fiche dans [`skills/animation/references/`](skills/animation/references/) : ce que c'est, quand l'utiliser, la technique avec les valeurs exactes, un composant Remotion qui marche, des variations et les pièges à éviter.

---

## Installation

### Option 1 — Claude Code, via le marketplace de plugins (recommandé)

Dans Claude Code :

```
/plugin marketplace add bthasystem/claude-animations-skill
/plugin install animation@bthasystem
```

> ⚠️ `bthasystem` est un **nom provisoire** : remplace-le par le vrai compte GitHub qui héberge ce repo.

Avec le plugin, la commande s'appelle `/animation:animation` (les skills de plugin sont préfixés par le nom du plugin). Tu peux aussi simplement écrire « fais-moi une animation glitch » : Claude charge le skill tout seul.

### Option 2 — Claude Code, copie manuelle (commande courte `/animation`)

```bash
git clone https://github.com/bthasystem/claude-animations-skill.git
cp -r claude-animations-skill/skills/animation ~/.claude/skills/
```

Redémarre Claude Code : la commande `/animation` est disponible dans tous tes projets.

### Option 3 — claude.ai (app web / desktop)

1. Télécharge le dossier `skills/animation` et compresse-le en **`animation.zip`** (le zip doit contenir le dossier `animation/` avec `SKILL.md` dedans).
2. Sur claude.ai : **Settings → Capabilities → Skills → Upload skill**, choisis le zip.
3. Dans une conversation, demande « /animation glitch » ou « une animation style synthwave ».

> Sur claude.ai, Claude peut écrire le code et l'aperçu web ; pour **rendre une vidéo MP4**, il faut Claude Code (ou ton ordi) avec Node.js et Remotion.

---

## Utilisation

```
/animation                          → affiche le catalogue
/animation neon "OPEN 24/7"         → style + texte
/animation data-story 16:9          → style + format
/animation glitch + synthwave       → combine deux styles
/animation vhs                      → slug inconnu : Claude te propose les 3 styles les plus proches
```

Par défaut : **Remotion, 1080×1920, 30 i/s, 5 secondes**. Pour le rendu :

```bash
npx remotion render <Composition> out/video.mp4
```

Pré-requis pour la vidéo : Node.js 18+ et un projet Remotion (`npx create-video@latest`). Claude peut le créer pour toi.

---

## Les 100 styles

| # | Style | Commande | Ce que ça fait |
|---|---|---|---|
| | **🔤 Typographie cinétique & effets textuels** | | |
| 1 | Kinetic Type (Headline) | `/animation kinetic-type` | Texte percutant synchronisé à la voix, zooms saccadés, mot par mot |
| 2 | Split-Flap Display | `/animation split-flap` | Panneau de gare : les lettres tournent avant de se fixer |
| 3 | Glitch & VHS Rétro | `/animation glitch` | Distorsions, lignes de scan, bruit analogique, aberration chromatique |
| 4 | HUD & Terminal Sci-Fi | `/animation hud` | Interface futuriste monospace, compteurs rapides, lignes de visée |
| 5 | ASCII Art Animation | `/animation ascii` | Image faite de caractères qui changent en temps réel |
| 6 | Scramble / Text Reveal | `/animation scramble` | Lettres aléatoires façon décryptage avant le vrai mot |
| 7 | Kinetic Stacking | `/animation stacking` | Mots qui s'empilent à un rythme frénétique |
| 8 | Typewriter | `/animation typewriter` | Machine à écrire, lettre par lettre, curseur clignotant |
| 9 | Karaoke Captions | `/animation karaoke` | Sous-titres TikTok, le mot prononcé s'allume |
| 10 | Handwriting / Signature | `/animation handwriting` | Texte qui se trace à la main (tracé SVG) |
| 11 | Text on Path | `/animation text-path` | Texte qui suit une courbe, un cercle, une spirale |
| 12 | Variable Font Morph | `/animation variable-font` | Lettres qui s'épaississent, s'étirent, respirent |
| 13 | Highlighter Sweep | `/animation highlighter` | Surligneur ou soulignement qui se dessine sous un mot |
| 14 | Rotating Words | `/animation rotating-words` | Un mot qui change en boucle dans une phrase fixe |
| 15 | Mask Reveal | `/animation mask-reveal` | Texte révélé à travers une forme ou une image |
| | **🎨 Mouvements organiques, fluides & abstraits** | | |
| 16 | Liquid Morphing | `/animation liquid-morph` | Formes qui se transforment les unes dans les autres |
| 17 | Particles System | `/animation particles` | Milliers de points soumis au vent, à la gravité |
| 18 | Neon Glow / Cyberpunk | `/animation neon` | Lignes lumineuses avec halo et traînées |
| 19 | Wave & Topographic | `/animation topographic` | Lignes de niveau ou vagues mathématiques en boucle |
| 20 | Aurora & Mesh Gradient | `/animation aurora` | Dégradés flous qui bougent comme des aurores |
| 21 | Blobs & Metaballs | `/animation metaballs` | Formes rondes qui fusionnent de façon visqueuse |
| 22 | Bioluminescence | `/animation bioluminescence` | Pulsation lumineuse douce façon abysses |
| 23 | Flow Field (Perlin Noise) | `/animation flow-field` | Milliers de traits qui suivent un champ de courants |
| 24 | Fluid / Ink in Water | `/animation ink-water` | Encre qui se diffuse dans l'eau |
| 25 | Fractal Zoom | `/animation fractal-zoom` | Zoom infini dans une fractale |
| 26 | Voronoi Cells | `/animation voronoi` | Cellules organiques qui se divisent et bougent |
| 27 | Reaction-Diffusion | `/animation reaction-diffusion` | Motifs vivants façon peau de zèbre ou corail |
| 28 | Smoke & Fire Shader | `/animation smoke-fire` | Fumée ou flammes générées en temps réel |
| 29 | Grain & Noise Gradient | `/animation grain-gradient` | Dégradé granuleux façon affiche moderne |
| | **🧊 Styles 3D, reliefs & matériaux** | | |
| 30 | Clay 3D (Argile) | `/animation clay-3d` | Objets pâte à modeler avec physique et rebonds |
| 31 | Glassmorphism | `/animation glassmorphism` | Panneaux de verre dépoli qui floutent le fond |
| 32 | Isometric & Wireframe 3D | `/animation wireframe` | Perspective isométrique ou structures en fil de fer |
| 33 | Neumorphism | `/animation neumorphism` | Formes moulées dans le fond par jeu d'ombres |
| 34 | Low Poly | `/animation low-poly` | 3D à facettes qui accrochent la lumière |
| 35 | Skeuomorphism | `/animation skeuomorphism` | Vrais boutons, interrupteurs, textures cuir ou métal |
| 36 | Abstract Chrome / Liquid Metal | `/animation liquid-chrome` | Mercure ou métal liquide réfléchissant |
| 37 | 3D Product Spin | `/animation product-spin` | Objet 3D qui tourne sur lui-même (packshot) |
| 38 | 3D Extruded Text | `/animation extruded-text` | Texte en volume avec lumière et ombres |
| 39 | Card Flip 3D | `/animation card-flip` | Cartes qui se retournent en perspective |
| 40 | Cube / Box Transition | `/animation cube-transition` | Transition par rotation d'un cube |
| 41 | 3D Globe | `/animation globe` | Globe terrestre qui tourne avec des points lumineux |
| 42 | Galaxy Particles 3D | `/animation galaxy` | Galaxie de particules en profondeur |
| 43 | Holographic Foil | `/animation holo-foil` | Reflet irisé arc-en-ciel façon carte holo |
| 44 | Voxel Art | `/animation voxel` | 3D en petits cubes façon Minecraft |
| | **📐 Esthétiques artistiques, graphiques & historiques** | | |
| 45 | Swiss Design | `/animation swiss` | Grilles asymétriques, grandes sans-serif, transitions géométriques |
| 46 | Risograph & Halftone | `/animation risograph` | Impression rétro granuleuse, calques décalés, 12 i/s |
| 47 | Paper Cut-out | `/animation paper-cutout` | Papier découpé avec ombre, stop-motion |
| 48 | Neobrutalism | `/animation neobrutalism` | Contours noirs épais, ombres dures, couleurs criardes |
| 49 | Bauhaus | `/animation bauhaus` | Cercle, carré, triangle, couleurs primaires |
| 50 | Memphis Style | `/animation memphis` | Zigzags, confettis, damiers, pastels années 80 |
| 51 | Synthwave / Vaporwave | `/animation synthwave` | Grille néon infinie vers un soleil couchant |
| 52 | Pixel Art / 8-Bit | `/animation pixel-art` | Rétro-gaming sans lissage |
| 53 | Comic Book / Cel Shading | `/animation comic` | BD, contours noirs, trame Pop Art |
| 54 | Blueprint | `/animation blueprint` | Plan d'architecte qui se trace sur fond bleu |
| 55 | Frutiger Aero | `/animation frutiger-aero` | Bulles, verre bleu-vert, années 2000 |
| 56 | Corporate Memphis | `/animation corporate-memphis` | Personnages flat aux membres étirés |
| 57 | Psychedelic Retro | `/animation psychedelic` | Années 70, ondulations, couleurs saturées |
| 58 | Line Boil | `/animation line-boil` | Contours qui tremblent comme un dessin animé |
| 59 | Y2K / Chrome Bubble | `/animation y2k` | Années 2000, chromes, étoiles, bulles |
| 60 | Manga Speed Lines | `/animation manga-speed-lines` | Lignes de vitesse et onomatopées manga |
| 61 | Watercolor | `/animation watercolor` | Taches d'aquarelle qui se diffusent |
| 62 | Chalkboard / Sketch | `/animation chalkboard` | Dessin à la craie ou au crayon qui se trace |
| 63 | Art Deco | `/animation art-deco` | Lignes dorées géométriques années 20 |
| 64 | Duotone | `/animation duotone` | Image en deux couleurs contrastées |
| | **📊 Tableaux de bord, données & interfaces UI** | | |
| 65 | Data Storytelling | `/animation data-story` | Barres qui montent, courbes qui se tracent |
| 66 | Bento Grid | `/animation bento` | Grille de cases asymétriques qui bougent |
| 67 | Count-up / Odometer | `/animation odometer` | Compteurs qui défilent jusqu'à la valeur |
| 68 | Micro-interactions UI | `/animation micro-interactions` | Boutons, icônes qui se transforment, ripple |
| 69 | Infinite Scroll / Marquee | `/animation marquee` | Bandeaux qui défilent à l'infini |
| 70 | Parallax Scrolling | `/animation parallax` | Couches à vitesses différentes |
| 71 | Skeleton Loading | `/animation skeleton` | Formes qui pulsent avant le contenu |
| 72 | Bar Chart Race | `/animation bar-race` | Classement de barres qui se doublent dans le temps |
| 73 | Progress Rings | `/animation progress-rings` | Anneaux type Apple Watch qui se remplissent |
| 74 | Map Route Tracing | `/animation map-route` | Parcours tracé sur une carte (running !) |
| 75 | Timeline | `/animation timeline` | Frise chronologique qui avance |
| 76 | Chat / iMessage Simulation | `/animation chat` | Bulles de conversation qui apparaissent |
| 77 | Notification Stack | `/animation notifications` | Notifications qui tombent et s'empilent |
| 78 | Phone Mockup Scroll | `/animation phone-scroll` | Écran d'app qui défile dans un téléphone |
| 79 | Code Typing / IDE | `/animation code-typing` | Code qui s'écrit dans un éditeur |
| 80 | Cursor Demo | `/animation cursor-demo` | Curseur qui clique et navigue dans une interface |
| 81 | Before / After Slider | `/animation before-after` | Comparaison avant-après avec curseur |
| | **🎬 Transitions & mouvements de caméra** | | |
| 82 | Whip Pan | `/animation whip-pan` | Panoramique ultra-rapide avec flou de mouvement |
| 83 | Zoom Punch | `/animation zoom-punch` | Zoom brutal sur le temps fort |
| 84 | Camera Shake | `/animation camera-shake` | Tremblement d'impact |
| 85 | Shape Wipe | `/animation shape-wipe` | Transition par forme qui balaie l'écran |
| 86 | Iris / Circle Reveal | `/animation iris` | Ouverture ou fermeture en cercle |
| 87 | Ink / Paint Transition | `/animation ink-transition` | Tache d'encre qui envahit l'écran |
| 88 | Light Leak & Lens Flare | `/animation light-leak` | Fuites de lumière façon pellicule |
| 89 | Ken Burns | `/animation ken-burns` | Zoom lent sur une photo |
| 90 | Split Screen | `/animation split-screen` | Écran divisé qui s'ouvre |
| 91 | Speed Ramp | `/animation speed-ramp` | Ralenti puis accélération |
| 92 | Page Curl | `/animation page-curl` | Page qui se tourne |
| 93 | Match Cut / Morph Cut | `/animation match-cut` | Raccord parfait entre deux formes |
| | **🧸 Personnages & effets fun** | | |
| 94 | Lip-Sync Avatar | `/animation lip-sync` | Avatar dont la bouche suit la voix |
| 95 | Puppet Rig | `/animation puppet-rig` | Personnage articulé (bras, tête, clignements) |
| 96 | Confetti / Emoji Rain | `/animation confetti` | Pluie de confettis ou d'emojis |
| 97 | Sticker Pop | `/animation sticker-pop` | Stickers qui surgissent avec rebond |
| 98 | Squash & Stretch | `/animation squash-stretch` | Objet élastique façon cartoon |
| 99 | Morphing Icons | `/animation morph-icons` | Une icône qui se transforme en une autre |
| 100 | Firework / Burst | `/animation fireworks` | Explosion de particules de fête |

Le plan de la série TikTok (5 parties × 20 styles) est dans [`parties.md`](parties.md).

---

## Structure du repo

```
.claude-plugin/
  marketplace.json      ← catalogue du marketplace (1 plugin : animation)
  plugin.json           ← manifeste du plugin
skills/animation/
  SKILL.md              ← le skill : lecture de l'argument, workflow, catalogue
  references/<slug>.md  ← 100 fiches techniques
parties.md              ← plan des 5 parties TikTok
```

## Licence

MIT — utilise, modifie, partage. Un petit crédit « BTHA System » fait toujours plaisir. 💜

Fait avec Claude par **Ty — BTHA System** · [bthasystem.io](https://bthasystem.io)

# /animation — 100 styles d'animation pour Claude

> Par **Ty — BTHA System**, développeuse full stack web2 & web3 · [bthasystem.io](https://bthasystem.io)
> 🇫🇷 Français · [🇬🇧 English](README.en.md)

Tu as commenté **ANIMATION** ? Le voilà, et c'est **gratuit**. 🚀

Un skill pour Claude qui connaît **100 styles d'animation** : glitch, synthwave, néon, particules, typographie cinétique, data viz, transitions de caméra… Tu choisis un style, Claude lit la fiche technique que j'ai préparée et te code l'animation : une vidéo 9:16 prête pour TikTok / Reels / Shorts, ou une animation pour ton site.

```
/animation glitch "BTHA SYSTEM"
/animation synthwave
/animation odometer "48 250 abonnés"
```

Chaque style a sa fiche dans [`skills/animation/references/`](skills/animation/references/) : ce que c'est, quand l'utiliser, la technique avec les valeurs exactes, un composant qui marche, des variations et les pièges à éviter.

---

## Avant de commencer : ce qu'il te faut

| Tu veux… | Abonnement Claude | Logiciel | En plus |
|---|---|---|---|
| **Voir et tester les animations** (aperçu dans la conversation) | Gratuit possible, **Pro conseillé** ¹ | Navigateur ([claude.ai](https://claude.ai)) ou [Claude Desktop](https://claude.com/download) | Rien |
| **Exporter de vraies vidéos MP4** pour TikTok / Reels | **Pro minimum** (Claude Code est inclus dans tous les abonnements payants) | [Claude Desktop](https://claude.com/download), onglet **Code** | [Node.js](https://nodejs.org) (version LTS), gratuit |

**Prix indicatifs (octobre 2026, tarifs US hors taxes, vérifie sur [claude.com/pricing](https://claude.com/pricing)) :**

| Abonnement | Prix | Pour qui |
|---|---|---|
| Free | 0 € | Découvrir, tester quelques aperçus |
| **Pro** | ~20 $/mois (~17 $/mois en annuel) | ✅ **Le bon choix pour ce skill** : chat + Claude Code + export vidéo |
| Max | 100 $/mois | Tu produis beaucoup de vidéos chaque semaine |
| Ultra | 200 $/mois | Usage intensif / pro |

¹ L'aide officielle d'Anthropic indique que les skills sont disponibles dès le plan gratuit, mais une partie de la documentation les réserve aux plans payants. Les limites d'utilisation du plan gratuit sont aussi très vite atteintes avec du code d'animation. **Pro est le minimum confortable.**

**Système :** Claude Desktop tourne sur **Mac** (Intel et Apple Silicon) et **Windows** (x64 et ARM). Pas besoin de Mac !

**Licence Remotion (le moteur vidéo) :** gratuite pour les particuliers, les associations et les structures de **3 personnes maximum**. Au-delà, ton entreprise a besoin d'une [licence Remotion](https://www.remotion.dev/docs/license/faq).

---

> 🧩 **Et les installations « npm » ?** Tu n'as **aucune commande à taper**.
> - **Sur claude.ai** : rien à installer du tout, Claude exécute le code dans son propre environnement.
> - **Pour exporter en MP4 (Claude Desktop, onglet Code)** : il te faut **Node.js**. C'est le moteur qui permet à ton ordinateur de faire tourner du code JavaScript, et donc de fabriquer la vidéo. Gratuit, il s'installe comme n'importe quelle app. **Pas besoin de savoir comment** : écris simplement à Claude « Installe Node.js pour moi », il s'en occupe ou te guide pas à pas.
> - Ensuite, Claude installe tout seul Remotion et les paquets npm dont il a besoin (`npm install`) ; il te demande juste l'autorisation, tu cliques sur « Autoriser ».

## ✨ Le plus simple : tout en langage naturel avec Claude Desktop

C'est comme ça que je travaille : **je conçois, je décris ce que je veux, Claude exécute.** Aucune commande à connaître.

1. Installe **[Claude Desktop](https://claude.com/download)** et connecte-toi avec ton compte Pro.
2. Ouvre l'onglet **Code** (c'est Claude Code, déjà inclus dans l'app : rien d'autre à installer pour lui).
3. Choisis un dossier vide pour tes vidéos (ex. `Documents/mes-animations`).
4. Écris simplement :

> « Installe le skill animation depuis github.com/Ty-HA/claude-animations dans mes skills personnels. »

> « Vérifie que Node.js est installé, sinon installe-le pour moi. »

> « Crée un projet Remotion et fais-moi une vidéo TikTok de 5 secondes style glitch avec le texte "BTHA SYSTEM", puis exporte-la en MP4. »

5. Claude te demande l'autorisation avant chaque action (installer, créer des fichiers, lancer le rendu) : tu valides, et ta vidéo arrive dans le dossier `out/`.

Ensuite, tu enchaînes en français normal : *« Plus rapide »*, *« Mets mes couleurs : #00B5C7 et #A855F7 »*, *« Fais la même en synthwave »*, *« Ajoute mon logo »*…

---

## Installation

### 🟢 Option 1 — Sans rien coder (claude.ai ou Claude Desktop, onglet Chat)

1. **[⬇️ Télécharge `animation.zip`](https://github.com/Ty-HA/claude-animations/releases/latest/download/animation.zip)** (ne le décompresse pas).
2. Dans **Paramètres → Capacités** (*Settings → Capabilities*), active **« Créer et modifier des fichiers »** / **exécution de code** (*Create and edit files / Code execution*). Les skills en ont besoin.
3. Va dans **Personnaliser → Skills** (*Customize → Skills*), clique sur **+** → **Créer un skill** → **Importer un skill** (*Upload a skill*) et choisis `animation.zip`.
4. Vérifie que le skill **animation** est activé, puis, dans une nouvelle conversation : `/animation glitch "Mon texte"`.

> L'interface de Claude change souvent : si un menu porte un nom légèrement différent, cherche « Skills » dans les paramètres.
> Bon à savoir : les skills activés sur ton compte claude.ai sont aussi synchronisés vers Claude Code (versions récentes).

### 💻 Option 2 — Claude Code, via le marketplace de plugins

```
/plugin marketplace add Ty-HA/claude-animations
/plugin install animation@bthasystem
```

Avec le plugin, la commande s'appelle `/animation:animation` (les skills de plugin sont préfixés par le nom du plugin). Tu peux aussi simplement écrire « fais-moi une animation glitch » : Claude charge le skill tout seul.

### 💻 Option 3 — Claude Code, copie manuelle (commande courte `/animation`)

```bash
git clone https://github.com/Ty-HA/claude-animations.git
cp -r claude-animations/skills/animation ~/.claude/skills/
```

Redémarre Claude Code : la commande `/animation` est disponible dans tous tes projets.

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

Pré-requis pour la vidéo : Node.js (version LTS) et un projet Remotion (`npx create-video@latest`). Claude peut le créer pour toi.

---

## 🪙 Combien de tokens ça consomme (et comment optimiser)

Je préfère te le dire franchement : chaque animation consomme des tokens, donc une part de ton quota Claude. Voici les vrais chiffres, mesurés sur les fichiers du skill (≈ 3,6 caractères par token).

| Étape | Tokens (environ) | Quand |
|---|---|---|
| Description du skill | ~120 | Toujours, dans chaque conversation (c'est tout ce que Claude voit tant que tu ne l'utilises pas) |
| `SKILL.md` | ~3 300 | Seulement quand tu tapes `/animation …` |
| Une fiche de style | 500 à 1 050 (médiane ~750) | Seulement la fiche du style demandé |
| Code de l'animation écrit par Claude | ~500 à 3 000 en sortie | À chaque version |
| Vérifier une image du rendu | ~1 500 par image | Si Claude regarde le résultat |

**Une animation simple, faite proprement : environ 10 000 à 20 000 tokens.** Avec 3 ou 4 allers-retours de corrections, compte plutôt 30 000 à 60 000 : à chaque nouveau message, Claude relit tout l'historique de la conversation (en grande partie depuis le cache, ce qui coûte moins cher, mais ça compte quand même).

**Pourquoi le skill est déjà optimisé** : les 100 fiches font ~74 000 tokens au total, mais Claude n'en charge qu'**une seule** à la fois. Résultat : ~4 000 tokens au lieu de ~78 000, soit **environ 95 % d'économie** par rapport à un gros prompt « tout-en-un ». C'est le principe des skills : le chargement progressif.

### Mes 8 tips pour économiser des tokens

1. **Tout dire dans le premier message** : style, texte, format, durée, couleurs. `/animation neon "OPEN 24/7" 9:16 5s couleurs rose et cyan` évite 2 ou 3 questions-réponses.
2. **Une conversation = une animation.** Les longues conversations coûtent de plus en plus cher à chaque message. Nouvelle animation → nouvelle conversation (ou `/clear` dans Claude Code).
3. **Vérifier avec une seule image**, pas une vidéo complète ni 10 captures : « rends juste la frame 30 et regarde-la ».
4. **Corriger précisément** : « le texte déborde à droite, réduis la taille à 90 px » coûte bien moins que « c'est pas bien, refais ».
5. **Choisir le bon modèle** : un modèle plus léger (Sonnet, voire Haiku) suffit pour une animation simple ; garde le plus puissant (Opus) pour les créations complexes ou les gros projets.
6. **Ne pas coller de gros fichiers dans le chat** : donne le chemin du fichier, Claude ne lira que ce qu'il faut.
7. **Réutiliser** : demande à Claude de ranger chaque animation réussie comme composant réutilisable, au lieu de la régénérer à chaque vidéo.
8. **Surveiller ta conso** : dans Claude Code, `/context` montre ce qui occupe la mémoire de la conversation et `/usage` ton quota d'abonnement.

> Sur un abonnement Pro ou Max, tu ne paies pas au token : tu as un quota qui se recharge. Ces tips te permettent simplement d'en faire beaucoup plus avec le même abonnement.

## FAQ

**Ça coûte combien ?**
Le skill est **gratuit** (licence MIT). Il te faut un abonnement Claude : **Pro (~20 $/mois)** suffit pour tout faire, export MP4 compris. Node.js et Remotion sont gratuits pour un usage perso ou une petite structure (≤ 3 personnes).

**Je n'ai pas de Mac.**
Aucun problème : Claude Desktop existe sur Windows, et Remotion fonctionne sur Windows (x64), macOS et Linux.

**Ça marche sur mobile ?**
Pour l'instant, je n'ai trouvé **aucune confirmation officielle** que les skills personnalisés s'installent ou s'utilisent depuis l'app iPhone / Android. Installe et utilise le skill depuis un ordinateur (navigateur ou Claude Desktop).

**« node: command not found » / erreur Node pendant le rendu ?**
Node.js n'est pas installé (ou pas trouvé). Installe la version **LTS** depuis [nodejs.org](https://nodejs.org), **ferme et rouvre** Claude Desktop, puis redemande le rendu. Tu peux aussi écrire à Claude : *« J'ai cette erreur, aide-moi à la corriger »* en collant le message.

**`/animation` ou `/animation:animation` ?**
- Installé via **claude.ai** (zip) ou par **copie manuelle** : `/animation glitch`.
- Installé via le **marketplace de plugins** : `/animation:animation glitch`.
- Dans tous les cas, tu peux juste écrire *« fais-moi une animation style glitch »*.

**Je peux utiliser les vidéos pour mes clients / ma marque ?**
Oui. Le skill est sous licence MIT. Vérifie seulement la [licence Remotion](https://www.remotion.dev/docs/license/faq) si ta structure compte plus de 3 personnes.

**Le style que je veux n'est pas dans la liste.**
Tape quand même `/animation <ton idée>` : Claude te propose les 3 styles les plus proches, ou combine-en deux (`/animation glitch + synthwave`).

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

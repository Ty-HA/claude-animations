---
name: animation
description: Crée une animation ou une vidéo motion design dans l'un des 100 styles du catalogue BTHA System (glitch, synthwave, kinetic type, neon, particules, data viz, transitions…). À utiliser quand on demande « /animation <style> », une animation dans un style précis, un effet vidéo, une intro, une transition ou une vidéo TikTok/Reels animée en code (Remotion, React, HTML/CSS, canvas, SVG).
---

# /animation — 100 styles d'animation prêts à l'emploi

Par Ty — BTHA System (bthasystem.io).

## Lire l'argument

Formes acceptées :
- `/animation glitch` → style `glitch`, texte par défaut.
- `/animation glitch "Mon texte"` → style + texte à animer (passe-le dans la prop `text`).
- `/animation glitch 16:9` ou `... web` → format ou cible précisés.
- `/animation` seul ou `/animation liste` → affiche le catalogue ci-dessous, groupé par famille, et demande un style.

Le premier mot est le **slug**. Chaque slug a sa fiche : `references/<slug>.md` (dans le dossier de ce skill). Lis **toujours** la fiche avant d'écrire du code : elle contient la technique, les valeurs exactes et un composant Remotion de départ.

## Slug inconnu

Si le slug n'existe pas : ne devine pas en silence. Propose les **3 styles les plus proches** (par nom, famille ou effet décrit), avec leur description d'une ligne, et demande lequel prendre. Exemples : « vhs » → `glitch`, `risograph`, `light-leak` ; « 3d text » → `extruded-text`, `liquid-chrome`, `kinetic-type` ; « matrix » → `ascii`, `scramble`, `code-typing`.

## Stack par défaut

- **Vidéo (par défaut)** : Remotion (React + TypeScript), **1080×1920, 30 i/s** (TikTok, Reels, Shorts). Durée par défaut : 5 s (150 frames) sauf demande contraire.
- **Web** : si l'utilisateur veut une animation pour un site, transpose la fiche en HTML/CSS/JS (CSS keyframes, `requestAnimationFrame`, canvas ou SVG). La logique « position = fonction du temps » reste la même.
- Autres formats : 1080×1080 (carré), 1920×1080 (YouTube). Respecter les zones sûres en 9:16 : ~140 px de marge latérale, rien d'important dans les 25 % du bas (interface TikTok).

Règles Remotion à respecter :
- Tout dépend de `useCurrentFrame()` : **aucun état accumulé** d'une frame à l'autre (les frames sont rendues dans le désordre et en parallèle).
- Aléatoire uniquement via `random('graine')` de `remotion`, jamais `Math.random()`.
- Médias via `staticFile()`, `<Img>`, `<OffthreadVideo>`, `<Audio>` pour que le rendu attende le chargement.
- Polices : `@remotion/google-fonts/<Police>` pour un rendu garanti.

## Workflow

1. **Identifier** le slug (ou proposer 3 styles proches).
2. **Lire** `references/<slug>.md`.
3. **Clarifier si besoin** (une seule question groupée, seulement si c'est inconnu et important) : format (9:16 par défaut), durée, texte, couleurs de marque, projet Remotion existant ou nouveau.
4. **Construire** :
   - Projet existant : ajouter `src/<Style>.tsx` et l'enregistrer dans `Root.tsx` avec `<Composition id=... durationInFrames fps={30} width={1080} height={1920} />`.
   - Nouveau projet : `npx create-video@latest` (template blank), puis même chose.
   - Partir du snippet de la fiche, l'adapter au texte/couleurs, installer les dépendances indiquées dans la fiche.
5. **Vérifier** : `npx remotion still <id> --frame=<n> out.png` sur 2–3 frames clés, regarder l'image, corriger.
6. **Rendre** : `npx remotion render <id> out/<id>.mp4` (ajouter `--crf=18` pour une meilleure qualité, `--gl=angle` si le rendu 3D est noir).
7. **Livrer** : chemin du fichier, durée, et 2–3 variations possibles tirées de la fiche.

Plusieurs styles demandés à la fois (`/animation glitch + synthwave`) : lire toutes les fiches, combiner (ex. fond `synthwave` + titre `glitch`) en un seul composant.

## Catalogue (100 styles)

### 🔤 Typographie cinétique & effets textuels

- `kinetic-type` — Kinetic Type (Headline) : Texte percutant synchronisé à la voix, zooms saccadés, mot par mot
- `split-flap` — Split-Flap Display : Panneau de gare : les lettres tournent avant de se fixer
- `glitch` — Glitch & VHS Rétro : Distorsions, lignes de scan, bruit analogique, aberration chromatique
- `hud` — HUD & Terminal Sci-Fi : Interface futuriste monospace, compteurs rapides, lignes de visée
- `ascii` — ASCII Art Animation : Image faite de caractères qui changent en temps réel
- `scramble` — Scramble / Text Reveal : Lettres aléatoires façon décryptage avant le vrai mot
- `stacking` — Kinetic Stacking : Mots qui s'empilent à un rythme frénétique
- `typewriter` — Typewriter : Machine à écrire, lettre par lettre, curseur clignotant
- `karaoke` — Karaoke Captions : Sous-titres TikTok, le mot prononcé s'allume
- `handwriting` — Handwriting / Signature : Texte qui se trace à la main (tracé SVG)
- `text-path` — Text on Path : Texte qui suit une courbe, un cercle, une spirale
- `variable-font` — Variable Font Morph : Lettres qui s'épaississent, s'étirent, respirent
- `highlighter` — Highlighter Sweep : Surligneur ou soulignement qui se dessine sous un mot
- `rotating-words` — Rotating Words : Un mot qui change en boucle dans une phrase fixe
- `mask-reveal` — Mask Reveal : Texte révélé à travers une forme ou une image

### 🎨 Mouvements organiques, fluides & abstraits

- `liquid-morph` — Liquid Morphing : Formes qui se transforment les unes dans les autres
- `particles` — Particles System : Milliers de points soumis au vent, à la gravité
- `neon` — Neon Glow / Cyberpunk : Lignes lumineuses avec halo et traînées
- `topographic` — Wave & Topographic : Lignes de niveau ou vagues mathématiques en boucle
- `aurora` — Aurora & Mesh Gradient : Dégradés flous qui bougent comme des aurores
- `metaballs` — Blobs & Metaballs : Formes rondes qui fusionnent de façon visqueuse
- `bioluminescence` — Bioluminescence : Pulsation lumineuse douce façon abysses
- `flow-field` — Flow Field (Perlin Noise) : Milliers de traits qui suivent un champ de courants
- `ink-water` — Fluid / Ink in Water : Encre qui se diffuse dans l'eau
- `fractal-zoom` — Fractal Zoom : Zoom infini dans une fractale
- `voronoi` — Voronoi Cells : Cellules organiques qui se divisent et bougent
- `reaction-diffusion` — Reaction-Diffusion : Motifs vivants façon peau de zèbre ou corail
- `smoke-fire` — Smoke & Fire Shader : Fumée ou flammes générées en temps réel
- `grain-gradient` — Grain & Noise Gradient : Dégradé granuleux façon affiche moderne

### 🧊 Styles 3D, reliefs & matériaux

- `clay-3d` — Clay 3D (Argile) : Objets pâte à modeler avec physique et rebonds
- `glassmorphism` — Glassmorphism : Panneaux de verre dépoli qui floutent le fond
- `wireframe` — Isometric & Wireframe 3D : Perspective isométrique ou structures en fil de fer
- `neumorphism` — Neumorphism : Formes moulées dans le fond par jeu d'ombres
- `low-poly` — Low Poly : 3D à facettes qui accrochent la lumière
- `skeuomorphism` — Skeuomorphism : Vrais boutons, interrupteurs, textures cuir ou métal
- `liquid-chrome` — Abstract Chrome / Liquid Metal : Mercure ou métal liquide réfléchissant
- `product-spin` — 3D Product Spin : Objet 3D qui tourne sur lui-même (packshot)
- `extruded-text` — 3D Extruded Text : Texte en volume avec lumière et ombres
- `card-flip` — Card Flip 3D : Cartes qui se retournent en perspective
- `cube-transition` — Cube / Box Transition : Transition par rotation d'un cube
- `globe` — 3D Globe : Globe terrestre qui tourne avec des points lumineux
- `galaxy` — Galaxy Particles 3D : Galaxie de particules en profondeur
- `holo-foil` — Holographic Foil : Reflet irisé arc-en-ciel façon carte holo
- `voxel` — Voxel Art : 3D en petits cubes façon Minecraft

### 📐 Esthétiques artistiques, graphiques & historiques

- `swiss` — Swiss Design : Grilles asymétriques, grandes sans-serif, transitions géométriques
- `risograph` — Risograph & Halftone : Impression rétro granuleuse, calques décalés, 12 i/s
- `paper-cutout` — Paper Cut-out : Papier découpé avec ombre, stop-motion
- `neobrutalism` — Neobrutalism : Contours noirs épais, ombres dures, couleurs criardes
- `bauhaus` — Bauhaus : Cercle, carré, triangle, couleurs primaires
- `memphis` — Memphis Style : Zigzags, confettis, damiers, pastels années 80
- `synthwave` — Synthwave / Vaporwave : Grille néon infinie vers un soleil couchant
- `pixel-art` — Pixel Art / 8-Bit : Rétro-gaming sans lissage
- `comic` — Comic Book / Cel Shading : BD, contours noirs, trame Pop Art
- `blueprint` — Blueprint : Plan d'architecte qui se trace sur fond bleu
- `frutiger-aero` — Frutiger Aero : Bulles, verre bleu-vert, années 2000
- `corporate-memphis` — Corporate Memphis : Personnages flat aux membres étirés
- `psychedelic` — Psychedelic Retro : Années 70, ondulations, couleurs saturées
- `line-boil` — Line Boil : Contours qui tremblent comme un dessin animé
- `y2k` — Y2K / Chrome Bubble : Années 2000, chromes, étoiles, bulles
- `manga-speed-lines` — Manga Speed Lines : Lignes de vitesse et onomatopées manga
- `watercolor` — Watercolor : Taches d'aquarelle qui se diffusent
- `chalkboard` — Chalkboard / Sketch : Dessin à la craie ou au crayon qui se trace
- `art-deco` — Art Deco : Lignes dorées géométriques années 20
- `duotone` — Duotone : Image en deux couleurs contrastées

### 📊 Tableaux de bord, données & interfaces UI

- `data-story` — Data Storytelling : Barres qui montent, courbes qui se tracent
- `bento` — Bento Grid : Grille de cases asymétriques qui bougent
- `odometer` — Count-up / Odometer : Compteurs qui défilent jusqu'à la valeur
- `micro-interactions` — Micro-interactions UI : Boutons, icônes qui se transforment, ripple
- `marquee` — Infinite Scroll / Marquee : Bandeaux qui défilent à l'infini
- `parallax` — Parallax Scrolling : Couches à vitesses différentes
- `skeleton` — Skeleton Loading : Formes qui pulsent avant le contenu
- `bar-race` — Bar Chart Race : Classement de barres qui se doublent dans le temps
- `progress-rings` — Progress Rings : Anneaux type Apple Watch qui se remplissent
- `map-route` — Map Route Tracing : Parcours tracé sur une carte (running !)
- `timeline` — Timeline : Frise chronologique qui avance
- `chat` — Chat / iMessage Simulation : Bulles de conversation qui apparaissent
- `notifications` — Notification Stack : Notifications qui tombent et s'empilent
- `phone-scroll` — Phone Mockup Scroll : Écran d'app qui défile dans un téléphone
- `code-typing` — Code Typing / IDE : Code qui s'écrit dans un éditeur
- `cursor-demo` — Cursor Demo : Curseur qui clique et navigue dans une interface
- `before-after` — Before / After Slider : Comparaison avant-après avec curseur

### 🎬 Transitions & mouvements de caméra

- `whip-pan` — Whip Pan : Panoramique ultra-rapide avec flou de mouvement
- `zoom-punch` — Zoom Punch : Zoom brutal sur le temps fort
- `camera-shake` — Camera Shake : Tremblement d'impact
- `shape-wipe` — Shape Wipe : Transition par forme qui balaie l'écran
- `iris` — Iris / Circle Reveal : Ouverture ou fermeture en cercle
- `ink-transition` — Ink / Paint Transition : Tache d'encre qui envahit l'écran
- `light-leak` — Light Leak & Lens Flare : Fuites de lumière façon pellicule
- `ken-burns` — Ken Burns : Zoom lent sur une photo
- `split-screen` — Split Screen : Écran divisé qui s'ouvre
- `speed-ramp` — Speed Ramp : Ralenti puis accélération
- `page-curl` — Page Curl : Page qui se tourne
- `match-cut` — Match Cut / Morph Cut : Raccord parfait entre deux formes

### 🧸 Personnages & effets fun

- `lip-sync` — Lip-Sync Avatar : Avatar dont la bouche suit la voix
- `puppet-rig` — Puppet Rig : Personnage articulé (bras, tête, clignements)
- `confetti` — Confetti / Emoji Rain : Pluie de confettis ou d'emojis
- `sticker-pop` — Sticker Pop : Stickers qui surgissent avec rebond
- `squash-stretch` — Squash & Stretch : Objet élastique façon cartoon
- `morph-icons` — Morphing Icons : Une icône qui se transforme en une autre
- `fireworks` — Firework / Burst : Explosion de particules de fête

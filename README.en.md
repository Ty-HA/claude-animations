# /animation — 100 animation styles for Claude

> By **Ty — BTHA System**, full stack web2 & web3 developer · [bthasystem.io](https://bthasystem.io)
> [🇫🇷 Français](README.md) · 🇬🇧 English

Commented **ANIMATION**? Here it is, and it's **free**. 🚀

A Claude skill that knows **100 animation styles**: glitch, synthwave, neon, particles, kinetic typography, data viz, camera transitions… Pick a style, Claude reads the technical sheet I prepared and codes the animation for you: a 9:16 video ready for TikTok / Reels / Shorts, or an animation for your website.

```
/animation glitch "BTHA SYSTEM"
/animation synthwave
/animation odometer "48,250 followers"
```

Each style has its own sheet in [`skills/animation/references/`](skills/animation/references/): what it is, when to use it, the technique with exact values, a working component, variations and pitfalls. (The sheets are written in French; Claude reads them fine and answers in your language.)

---

## Before you start: what you need

| You want to… | Claude plan | Software | Extra |
|---|---|---|---|
| **Preview and test animations** (inside the chat) | Free possible, **Pro recommended** ¹ | Browser ([claude.ai](https://claude.ai)) or [Claude Desktop](https://claude.com/download) | Nothing |
| **Export real MP4 videos** for TikTok / Reels | **Pro minimum** (Claude Code is included in every paid plan) | [Claude Desktop](https://claude.com/download), **Code** tab | [Node.js](https://nodejs.org) (LTS), free |

**Indicative prices (October 2026, US prices before tax, check [claude.com/pricing](https://claude.com/pricing)):**

| Plan | Price | For whom |
|---|---|---|
| Free | $0 | Discover, test a few previews |
| **Pro** | ~$20/month (~$17/month billed yearly) | ✅ **The right choice for this skill**: chat + Claude Code + video export |
| Max | $100/month | You produce lots of videos every week |
| Ultra | $200/month | Heavy / professional use |

¹ Anthropic's help center says skills are available from the Free plan, but part of the documentation reserves them for paid plans. Free usage limits are also reached quickly with animation code. **Pro is the comfortable minimum.**

**OS:** Claude Desktop runs on **Mac** (Intel and Apple Silicon) and **Windows** (x64 and ARM). No Mac needed!

**Remotion license (the video engine):** free for individuals, non-profits and teams of **up to 3 people**. Beyond that, your company needs a [Remotion license](https://www.remotion.dev/docs/license/faq).

---

> 🧩 **What about "npm" installs?** You **don't type a single command**.
> - **On claude.ai**: nothing to install at all, Claude runs the code in its own environment.
> - **To export MP4s (Claude Desktop, Code tab)**: you need **Node.js**. It's the engine that lets your computer run JavaScript code, and therefore build the video. It's free and installs like any app. **No need to know how**: just tell Claude "Install Node.js for me", it takes care of it or walks you through it step by step.
> - Then Claude installs Remotion and the npm packages it needs on its own (`npm install`); it just asks for permission and you click "Allow".

## ✨ The easiest way: everything in plain language with Claude Desktop

This is how I work: **I design, I describe what I want, Claude executes.** No command to learn.

1. Install **[Claude Desktop](https://claude.com/download)** and sign in with your Pro account.
2. Open the **Code** tab (that's Claude Code, already built into the app: nothing else to install for it).
3. Pick an empty folder for your videos (e.g. `Documents/my-animations`).
4. Just type:

> "Install the animation skill from github.com/Ty-HA/claude-animations into my personal skills."

> "Check that Node.js is installed, otherwise install it for me."

> "Create a Remotion project and make me a 5-second TikTok video in glitch style with the text "BTHA SYSTEM", then export it as MP4."

5. Claude asks for your permission before each action (install, create files, render): approve, and your video lands in the `out/` folder.

Then keep going in plain English: *"Faster"*, *"Use my colors: #00B5C7 and #A855F7"*, *"Same thing in synthwave"*, *"Add my logo"*…

---

## Installation

### 🟢 Option 1 — No code (claude.ai or Claude Desktop, Chat tab)

1. **[⬇️ Download `animation.zip`](https://github.com/Ty-HA/claude-animations/releases/latest/download/animation.zip)** (don't unzip it).
2. In **Settings → Capabilities**, turn on **"Create and edit files"** / **code execution**. Skills need it.
3. Go to **Customize → Skills**, click **+** → **Create skill** → **Upload a skill** and pick `animation.zip`.
4. Make sure the **animation** skill is toggled on, then in a new chat: `/animation glitch "My text"`.

> Claude's interface changes often: if a menu has a slightly different name, look for "Skills" in the settings.
> Good to know: skills enabled on your claude.ai account are also synced to Claude Code (recent versions).

### 💻 Option 2 — Claude Code, through the plugin marketplace

```
/plugin marketplace add Ty-HA/claude-animations
/plugin install animation@bthasystem
```

With the plugin, the command is `/animation:animation` (plugin skills are prefixed with the plugin name). You can also just write "make me a glitch animation": Claude loads the skill on its own.

### 💻 Option 3 — Claude Code, manual copy (short `/animation` command)

```bash
git clone https://github.com/Ty-HA/claude-animations.git
cp -r claude-animations/skills/animation ~/.claude/skills/
```

Restart Claude Code: `/animation` is available in all your projects.

---

## Usage

```
/animation                          → shows the catalog
/animation neon "OPEN 24/7"         → style + text
/animation data-story 16:9          → style + format
/animation glitch + synthwave       → combine two styles
/animation vhs                      → unknown slug: Claude suggests the 3 closest styles
```

Default: **Remotion, 1080×1920, 30 fps, 5 seconds**. To render:

```bash
npx remotion render <Composition> out/video.mp4
```

Video prerequisites: Node.js (LTS) and a Remotion project (`npx create-video@latest`). Claude can set it up for you.

---

## 🪙 How many tokens it uses (and how to optimize)

I'd rather be upfront: every animation uses tokens, so part of your Claude quota. Here are the real numbers, measured on the skill files (≈ 3.6 characters per token).

| Step | Tokens (approx.) | When |
|---|---|---|
| Skill description | ~120 | Always, in every conversation (that's all Claude sees until you use it) |
| `SKILL.md` | ~3,300 | Only when you type `/animation …` |
| One style sheet | 500 to 1,050 (median ~750) | Only the sheet for the requested style |
| Animation code written by Claude | ~500 to 3,000 output | For each version |
| Checking one rendered frame | ~1,500 per image | If Claude looks at the result |

**A simple animation, done cleanly: about 10,000 to 20,000 tokens.** With 3 or 4 rounds of fixes, expect 30,000 to 60,000: on every new message Claude re-reads the whole conversation (mostly from cache, which is cheaper, but it still counts).

**Why the skill is already optimized**: the 100 sheets total ~74,000 tokens, but Claude only loads **one** at a time. Result: ~4,000 tokens instead of ~78,000, **about 95% savings** compared with one giant "all-in-one" prompt. That's the whole point of skills: progressive loading.

### My 8 tips to save tokens

1. **Say everything in the first message**: style, text, format, length, colors. `/animation neon "OPEN 24/7" 9:16 5s pink and cyan` saves 2 or 3 back-and-forths.
2. **One conversation = one animation.** Long conversations get more expensive with every message. New animation → new conversation (or `/clear` in Claude Code).
3. **Check with a single frame**, not a full video or 10 screenshots: "just render frame 30 and look at it".
4. **Give precise fixes**: "the text overflows on the right, reduce it to 90px" costs far less than "it's not good, redo it".
5. **Pick the right model**: a lighter model (Sonnet, even Haiku) is enough for a simple animation; keep the most powerful one (Opus) for complex creations or big projects.
6. **Don't paste big files into the chat**: give the file path, Claude will read only what it needs.
7. **Reuse**: ask Claude to save each successful animation as a reusable component instead of regenerating it for every video.
8. **Watch your usage**: in Claude Code, `/context` shows what fills the conversation's memory and `/usage` shows your plan quota.

> On a Pro or Max plan you don't pay per token: you get a quota that refills. These tips just let you do a lot more with the same plan.

## FAQ

**How much does it cost?**
The skill is **free** (MIT license). You need a Claude plan: **Pro (~$20/month)** covers everything, MP4 export included. Node.js and Remotion are free for personal use or small teams (≤ 3 people).

**I don't have a Mac.**
No problem: Claude Desktop runs on Windows, and Remotion works on Windows (x64), macOS and Linux.

**Does it work on mobile?**
So far I found **no official confirmation** that custom skills can be installed or used from the iPhone / Android app. Install and use the skill from a computer (browser or Claude Desktop).

**"node: command not found" / Node error while rendering?**
Node.js isn't installed (or isn't found). Install the **LTS** version from [nodejs.org](https://nodejs.org), **quit and reopen** Claude Desktop, then ask for the render again. You can also paste the error and tell Claude: *"I got this error, help me fix it."*

**`/animation` or `/animation:animation`?**
- Installed via **claude.ai** (zip) or **manual copy**: `/animation glitch`.
- Installed via the **plugin marketplace**: `/animation:animation glitch`.
- Either way, you can just write *"make me a glitch-style animation"*.

**Can I use the videos for clients / my brand?**
Yes. The skill is MIT-licensed. Just check the [Remotion license](https://www.remotion.dev/docs/license/faq) if your organization has more than 3 people.

**The style I want isn't on the list.**
Type `/animation <your idea>` anyway: Claude suggests the 3 closest styles, or combine two (`/animation glitch + synthwave`).

---

## The 100 styles

| # | Style | Command | What it does |
|---|---|---|---|
| | **🔤 Kinetic typography & text effects** | | |
| 1 | Kinetic Type (Headline) | `/animation kinetic-type` | Punchy text synced to the voice, jerky zooms, word by word |
| 2 | Split-Flap Display | `/animation split-flap` | Train-station board: letters flip before settling |
| 3 | Glitch & VHS Rétro | `/animation glitch` | Distortion, scanlines, analog noise, chromatic aberration |
| 4 | HUD & Terminal Sci-Fi | `/animation hud` | Futuristic monospace interface, fast counters, targeting lines |
| 5 | ASCII Art Animation | `/animation ascii` | An image made of characters changing in real time |
| 6 | Scramble / Text Reveal | `/animation scramble` | Random letters decrypting into the real word |
| 7 | Kinetic Stacking | `/animation stacking` | Words stacking at a frantic pace |
| 8 | Typewriter | `/animation typewriter` | Letter by letter with a blinking cursor |
| 9 | Karaoke Captions | `/animation karaoke` | TikTok captions, the spoken word lights up |
| 10 | Handwriting / Signature | `/animation handwriting` | Text drawn by hand (SVG stroke) |
| 11 | Text on Path | `/animation text-path` | Text following a curve, circle or spiral |
| 12 | Variable Font Morph | `/animation variable-font` | Letters that thicken, stretch and breathe |
| 13 | Highlighter Sweep | `/animation highlighter` | Highlighter or underline drawn under a word |
| 14 | Rotating Words | `/animation rotating-words` | One word cycling inside a fixed sentence |
| 15 | Mask Reveal | `/animation mask-reveal` | Text revealed through a shape or an image |
| | **🎨 Organic, fluid & abstract motion** | | |
| 16 | Liquid Morphing | `/animation liquid-morph` | Shapes melting into one another |
| 17 | Particles System | `/animation particles` | Thousands of points driven by wind and gravity |
| 18 | Neon Glow / Cyberpunk | `/animation neon` | Glowing lines with halo and trails |
| 19 | Wave & Topographic | `/animation topographic` | Contour lines or looping math waves |
| 20 | Aurora & Mesh Gradient | `/animation aurora` | Blurred gradients drifting like northern lights |
| 21 | Blobs & Metaballs | `/animation metaballs` | Round shapes merging like goo |
| 22 | Bioluminescence | `/animation bioluminescence` | Soft deep-sea glow pulses |
| 23 | Flow Field (Perlin Noise) | `/animation flow-field` | Thousands of strokes following a current field |
| 24 | Fluid / Ink in Water | `/animation ink-water` | Ink spreading in water |
| 25 | Fractal Zoom | `/animation fractal-zoom` | Infinite zoom into a fractal |
| 26 | Voronoi Cells | `/animation voronoi` | Organic cells splitting and moving |
| 27 | Reaction-Diffusion | `/animation reaction-diffusion` | Living patterns like zebra skin or coral |
| 28 | Smoke & Fire Shader | `/animation smoke-fire` | Real-time smoke or flames |
| 29 | Grain & Noise Gradient | `/animation grain-gradient` | Grainy gradient, modern poster look |
| | **🧊 3D styles, depth & materials** | | |
| 30 | Clay 3D (Argile) | `/animation clay-3d` | Clay objects with bouncy physics |
| 31 | Glassmorphism | `/animation glassmorphism` | Frosted glass panels blurring the background |
| 32 | Isometric & Wireframe 3D | `/animation wireframe` | Isometric perspective or wireframe structures |
| 33 | Neumorphism | `/animation neumorphism` | Shapes molded into the background with soft shadows |
| 34 | Low Poly | `/animation low-poly` | Faceted 3D catching the light |
| 35 | Skeuomorphism | `/animation skeuomorphism` | Real buttons, switches, leather or metal textures |
| 36 | Abstract Chrome / Liquid Metal | `/animation liquid-chrome` | Reflective mercury / liquid metal |
| 37 | 3D Product Spin | `/animation product-spin` | 3D object spinning (packshot) |
| 38 | 3D Extruded Text | `/animation extruded-text` | 3D text with light and shadows |
| 39 | Card Flip 3D | `/animation card-flip` | Cards flipping in perspective |
| 40 | Cube / Box Transition | `/animation cube-transition` | Transition by rotating a cube |
| 41 | 3D Globe | `/animation globe` | Spinning Earth with glowing points |
| 42 | Galaxy Particles 3D | `/animation galaxy` | Deep 3D particle galaxy |
| 43 | Holographic Foil | `/animation holo-foil` | Iridescent holographic-card shine |
| 44 | Voxel Art | `/animation voxel` | 3D made of little cubes |
| | **📐 Art, graphic & historical aesthetics** | | |
| 45 | Swiss Design | `/animation swiss` | Asymmetric grids, big sans-serif, geometric transitions |
| 46 | Risograph & Halftone | `/animation risograph` | Grainy retro print, offset layers, 12 fps |
| 47 | Paper Cut-out | `/animation paper-cutout` | Paper cut-out with shadows, stop-motion |
| 48 | Neobrutalism | `/animation neobrutalism` | Thick black outlines, hard shadows, loud colors |
| 49 | Bauhaus | `/animation bauhaus` | Circle, square, triangle, primary colors |
| 50 | Memphis Style | `/animation memphis` | Zigzags, confetti, checkerboards, 80s pastels |
| 51 | Synthwave / Vaporwave | `/animation synthwave` | Infinite neon grid toward a sunset |
| 52 | Pixel Art / 8-Bit | `/animation pixel-art` | Retro gaming, no smoothing |
| 53 | Comic Book / Cel Shading | `/animation comic` | Comic book, black outlines, pop-art halftone |
| 54 | Blueprint | `/animation blueprint` | Architect's plan drawn on blue paper |
| 55 | Frutiger Aero | `/animation frutiger-aero` | Bubbles, blue-green glass, 2000s |
| 56 | Corporate Memphis | `/animation corporate-memphis` | Flat characters with stretched limbs |
| 57 | Psychedelic Retro | `/animation psychedelic` | 70s waves and saturated colors |
| 58 | Line Boil | `/animation line-boil` | Outlines that wobble like a cartoon |
| 59 | Y2K / Chrome Bubble | `/animation y2k` | 2000s chrome, stars and bubbles |
| 60 | Manga Speed Lines | `/animation manga-speed-lines` | Manga speed lines and onomatopoeia |
| 61 | Watercolor | `/animation watercolor` | Spreading watercolor blooms |
| 62 | Chalkboard / Sketch | `/animation chalkboard` | Chalk or pencil drawing being traced |
| 63 | Art Deco | `/animation art-deco` | 1920s golden geometric lines |
| 64 | Duotone | `/animation duotone` | Image in two contrasting colors |
| | **📊 Dashboards, data & UI** | | |
| 65 | Data Storytelling | `/animation data-story` | Rising bars, lines drawing themselves |
| 66 | Bento Grid | `/animation bento` | Asymmetric grid of moving tiles |
| 67 | Count-up / Odometer | `/animation odometer` | Counters rolling up to the value |
| 68 | Micro-interactions UI | `/animation micro-interactions` | Buttons and icons morphing, ripple |
| 69 | Infinite Scroll / Marquee | `/animation marquee` | Endlessly scrolling banners |
| 70 | Parallax Scrolling | `/animation parallax` | Layers moving at different speeds |
| 71 | Skeleton Loading | `/animation skeleton` | Pulsing placeholders before content |
| 72 | Bar Chart Race | `/animation bar-race` | Bars racing and overtaking over time |
| 73 | Progress Rings | `/animation progress-rings` | Apple Watch-style rings filling up |
| 74 | Map Route Tracing | `/animation map-route` | Route traced on a map (running!) |
| 75 | Timeline | `/animation timeline` | Moving timeline |
| 76 | Chat / iMessage Simulation | `/animation chat` | Chat bubbles popping in |
| 77 | Notification Stack | `/animation notifications` | Notifications dropping and stacking |
| 78 | Phone Mockup Scroll | `/animation phone-scroll` | App screen scrolling inside a phone |
| 79 | Code Typing / IDE | `/animation code-typing` | Code typing itself in an editor |
| 80 | Cursor Demo | `/animation cursor-demo` | Cursor clicking through an interface |
| 81 | Before / After Slider | `/animation before-after` | Before/after comparison slider |
| | **🎬 Transitions & camera moves** | | |
| 82 | Whip Pan | `/animation whip-pan` | Ultra-fast pan with motion blur |
| 83 | Zoom Punch | `/animation zoom-punch` | Hard zoom on the beat |
| 84 | Camera Shake | `/animation camera-shake` | Impact shake |
| 85 | Shape Wipe | `/animation shape-wipe` | A shape sweeping across the screen |
| 86 | Iris / Circle Reveal | `/animation iris` | Circular opening or closing |
| 87 | Ink / Paint Transition | `/animation ink-transition` | Ink blot taking over the screen |
| 88 | Light Leak & Lens Flare | `/animation light-leak` | Film-style light leaks |
| 89 | Ken Burns | `/animation ken-burns` | Slow zoom on a photo |
| 90 | Split Screen | `/animation split-screen` | Screen splitting open |
| 91 | Speed Ramp | `/animation speed-ramp` | Slow motion then speed-up |
| 92 | Page Curl | `/animation page-curl` | Page turning |
| 93 | Match Cut / Morph Cut | `/animation match-cut` | Seamless cut between two shapes |
| | **🧸 Characters & fun effects** | | |
| 94 | Lip-Sync Avatar | `/animation lip-sync` | Avatar whose mouth follows the voice |
| 95 | Puppet Rig | `/animation puppet-rig` | Rigged character (arms, head, blinks) |
| 96 | Confetti / Emoji Rain | `/animation confetti` | Confetti or emoji rain |
| 97 | Sticker Pop | `/animation sticker-pop` | Stickers popping in with a bounce |
| 98 | Squash & Stretch | `/animation squash-stretch` | Cartoon-style elastic object |
| 99 | Morphing Icons | `/animation morph-icons` | One icon morphing into another |
| 100 | Firework / Burst | `/animation fireworks` | Party particle burst |

The TikTok series plan (5 parts × 20 styles) is in [`parties.md`](parties.md) (French).

---

## Repo structure

```
.claude-plugin/
  marketplace.json      ← marketplace catalog (1 plugin: animation)
  plugin.json           ← plugin manifest
skills/animation/
  SKILL.md              ← the skill: argument parsing, workflow, catalog
  references/<slug>.md  ← 100 technical sheets
parties.md              ← plan of the 5 TikTok parts
```

## License

MIT — use it, change it, share it. A little "BTHA System" credit is always appreciated. 💜

Made with Claude by **Ty — BTHA System** · [bthasystem.io](https://bthasystem.io)

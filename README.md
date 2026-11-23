# # Anteiku

Link to the live deployment:  https://jessessozi-stack.github.io/anteiku-site/

Anteiku is a single-page website built from the free "Nexus System" HTML template by TemplateMo (template 629). It uses plain HTML, CSS and JavaScript only. There is no framework, no build step and no external library, so the project can be opened directly in a browser and hosted on any static host such as GitHub Pages or Netlify.

The page is built around a scroll-stacking effect. As the visitor scrolls, four full-screen sections slide up one on top of the other, and the section underneath shrinks and fades slightly to create a feeling of depth.

## Table of contents

1. Project overview
2. Folder structure
3. Page structure and content
4. How the code works
5. Responsive design and accessibility
6. Customizing the site
7. Running the site locally
8. Deploying the site
9. Credits and license

## 1. Project overview

| Item | Detail |
|---|---|
| Type | Single-page static website |
| Technologies | HTML5, CSS3, vanilla JavaScript |
| Fonts | Plus Jakarta Sans and JetBrains Mono, loaded from Google Fonts |
| Frameworks or libraries | None |
| Build tools required | None |
| Internet connection | Needed only to load the Google Fonts. The site still works without it, using fallback fonts |

## 2. Folder structure

```
anteiku-site/
|-- index.html
|-- templatemo-629-nexus-style.css
|-- templatemo-629-nexus-script.js
|-- README.md
`-- images/
    |-- nexus-macro.jpg
    |-- nexus-core.jpg
    |-- nexus-flux.jpg
    |-- nexus-grid.jpg
    |-- nexus-sync.jpg
    |-- nexus-tab-core.jpg
    |-- nexus-tab-fluidics.jpg
    `-- nexus-tab-sync.jpg
```

| File | Purpose |
|---|---|
| `index.html` | Holds the page structure and all visible text. This is the file to edit to change headings and wording. |
| `templatemo-629-nexus-style.css` | Holds all the styling: colors, layout, animations and responsive rules. |
| `templatemo-629-nexus-script.js` | Holds the behavior: scroll animation, progress bar, navigation arrows, the accordion and the tabs. |
| `images/` | Holds the photographs used in the second, third and fourth sections. |
| `README.md` | This documentation file. |

Note: `index.html` must stay at the top level of the project, next to the `images` folder, so that hosting services can find it and the image paths remain correct.

## 3. Page structure and content

The page contains four full-screen sections, called "cards", followed by a permanent bar at the bottom of the screen. Each card is a `<section>` element with the class `zcard` and a `data-card` number from 1 to 4.

### Card 1: Hero

The opening screen of the site.

| Element | Description |
|---|---|
| Eyebrow text | A small label above the title (class `hero-eyebrow`) |
| Main title | The large heading, "ANTEIKU" (class `hero-title`, an `<h1>` element) |
| Subtitle | A short paragraph describing the site (class `hero-sub`) |
| Decorative layers | A faint grid and three floating glass shapes behind the text, used only for visual effect |
| Scroll hint | The text "Scroll to descend" at the bottom of the screen |

### Card 2: Image with telemetry overlay

A full-screen photograph with an engineering-style overlay.

| Element | Description |
|---|---|
| Background image | `images/nexus-macro.jpg`, darkened by an overlay layer |
| Crosshair lines | A vertical and a horizontal line that cross over the image |
| Node dot | A pulsing dot placed at the crosshair intersection |
| Data box | A small panel (class `node-box`) listing three labeled values: efficiency, thermal delta and sync latency |
| Caption | A section label and an `<h2>` heading at the bottom of the screen |

### Card 3: Accordion

A row of four vertical image strips that expand when the visitor hovers over them or taps them.

| Strip | Idle title | Image | Revealed content |
|---|---|---|---|
| A.01 | Core Array | `nexus-core.jpg` | Label, heading and a short description |
| A.02 | Fluidics | `nexus-flux.jpg` | Label, heading and a short description |
| A.03 | Power Grid | `nexus-grid.jpg` | Label, heading and a short description |
| A.04 | Sync Layer | `nexus-sync.jpg` | Label, heading and a short description |

Each strip is a `<button>` element with the class `slice`. It has two states. The idle state shows a number and a title. The expanded state fades in the label, the `<h3>` heading and the description paragraph.

### Card 4: Tabs

A two-column section. The left column holds the heading and three tabs. The right column shows an image that changes when a tab is selected.

| Tab | Key (`data-goto`) | Image | Caption |
|---|---|---|---|
| 01. CORE | `core` | `nexus-tab-core.jpg` | FIG. 01 / CORE FRAME |
| 02. FLUIDICS | `fluidics` | `nexus-tab-fluidics.jpg` | FIG. 02 / FLUIDICS MANIFOLD |
| 03. SYNC | `sync` | `nexus-tab-sync.jpg` | FIG. 03 / SYNC TIMING PLANE |

Each tab has a number, a name and a one-sentence description. The image caption is stored in the JavaScript file, not in the HTML.

### Bottom bar (HUD)

A thin bar fixed to the bottom of the screen that stays visible on every card. The abbreviation HUD stands for heads-up display.

| Element | Description |
|---|---|
| Brand | The site name with a small colored dot (class `hud-brand`) |
| Credit | A link crediting the template author (class `hud-credit`) |
| Progress bar | A track and fill bar showing how far down the page the visitor has scrolled, with a percentage readout |
| Arrow buttons | Previous and next buttons that move the visitor exactly one card up or down |

## 4. How the code works

### The scroll-stacking engine

The HTML starts with an empty `<div class="scroll-spacer">` that is 400 percent of the screen height. It exists only to give the browser enough scrollable distance. All four cards are positioned with `position: fixed`, so they stay in place on the screen while the visitor scrolls.

On every scroll event, the function `renderEngine()` in the JavaScript file calculates how far the visitor has scrolled and, for each card, sets two values:

1. A vertical offset, so each card slides up into view as the previous one finishes.
2. A scale and opacity, so each card that has already been covered shrinks slightly (down to 90 percent) and fades.

The calculations are throttled with `requestAnimationFrame`, so the animation stays smooth.

### Progress bar and arrow buttons

`renderProgress()` converts the scroll position into a percentage, updates the width of the fill bar and the percentage text, and disables the up or down arrow when the visitor is at the very top or very bottom. The arrow buttons use `window.scrollTo()` with smooth scrolling to land exactly on a card boundary.

### Accordion

On devices with a mouse, hovering over a strip expands it and moving the mouse away collapses it. On touch devices, tapping a strip expands it and tapping it again collapses it. The active strip receives the class `active` and the attribute `aria-expanded="true"`.

### Tabs

Clicking a tab reads its `data-goto` value, marks that tab as active, shows the image whose `data-panel` value matches, and updates the caption text from the `captions` list in the JavaScript file.

### Styling

All colors are stored as CSS variables at the top of the stylesheet, in the `:root` block.

| Variable | Value | Use |
|---|---|---|
| `--titanium` | `#F4F5F6` | Light page background |
| `--carbon` | `#0D0E15` | Dark backgrounds and main text |
| `--volt` | `#D1FF00` | Bright accent color |
| `--steel` | `#D9E0E4` | Borders and thin lines |
| `--graphite` | `#1A1D1A` | Dark grey tone |
| `--white` | `#FFFFFF` | White |
| `--hud-h` | `60px` | Height of the bottom bar |

## 5. Responsive design and accessibility

### Responsive behavior

The stylesheet includes the following adjustments, all written with CSS media queries:

| Condition | What changes |
|---|---|
| Screen width of 900 pixels or less | The accordion strips stack vertically as horizontal bars. The tabs section becomes a single column. The decorative shapes shrink. The credit text and percentage readout in the bottom bar are hidden. |
| Screen height of 540 pixels or less | The hero subtitle and the tab descriptions are hidden so the layout fits short screens, such as a phone held sideways. |
| Reduced motion preference | See the accessibility section below. |

The `<meta name="viewport">` tag in the page head makes the layout scale correctly on phones and tablets.

### Accessibility features

- Visitors whose device is set to "reduce motion" receive a calm layout. The scroll-stacking effect is switched off, the cards display one after another as a normal page, and the animations are disabled.
- Decorative elements are marked with `aria-hidden="true"` so screen readers skip them.
- The accordion strips and tabs are real `<button>` elements, so they can be reached with the keyboard. They also report their state through `aria-expanded` and `aria-selected`.
- The arrow buttons have text labels ("Previous section" and "Next section") for screen readers.
- Every photograph has descriptive alternative text in its `alt` attribute.

## 6. Customizing the site

### Changing text

Open `index.html` and edit the text between the tags. Useful places:

| To change | Look for |
|---|---|
| Browser tab title | The `<title>` tag in the head |
| Search engine description | The `<meta name="description">` tag in the head |
| Main title and subtitle | `hero-title` and `hero-sub` in card 1 |
| Data box values | `node-stat` lines in card 2 |
| Accordion titles and descriptions | `slice-idle-title`, `<h3>` and `<p>` inside each `slice` in card 3 |
| Tab names and descriptions | `tab-name` and the `<p>` inside each `matrix-tab` in card 4 |
| Tab image captions | The `captions` list in `templatemo-629-nexus-script.js` |
| Footer brand name | `hud-brand` in the bottom bar |

### Changing colors

Edit the values in the `:root` block at the top of `templatemo-629-nexus-style.css`. Changing `--volt` alters the bright accent color across the whole site.

### Replacing images

Add your own images to the `images` folder and update the `src` attribute of the matching `<img>` tag in `index.html`. Keep file names lowercase with no spaces. Images of similar proportions to the originals will crop best. Update the `alt` text at the same time.

## 7. Running the site locally

1. Download or clone the project to your computer.
2. Open the folder in a code editor such as Visual Studio Code.
3. Open `index.html` in a web browser. Alternatively, install the Live Server extension in Visual Studio Code, right-click `index.html` and choose "Open with Live Server", which refreshes the page each time you save.

## 8. Deploying the site

Because the site is static, it can be published without any build settings.

### GitHub Pages

1. Upload the project files to a public GitHub repository, with `index.html` at the top level.
2. Open the repository's Settings tab and choose Pages.
3. Under Build and deployment, set Source to "Deploy from a branch".
4. Select the `main` branch and the `/ (root)` folder, then save.
5. After a few minutes, the site is available at `https://your-username.github.io/repository-name/`.

### Netlify

1. Create a new site and import the GitHub repository, or drag the project folder onto Netlify Drop.
2. Leave the build command and publish directory empty.
3. Deploy.

### Troubleshooting

| Problem | Likely cause |
|---|---|
| Blank page or 404 error | `index.html` is not at the top level of the published folder |
| Missing images | The `images` folder was not uploaded, or a file name differs in capitalization. Web servers treat `Logo.jpg` and `logo.jpg` as different files |
| No styling or no animation | The CSS or JavaScript file was not uploaded, or its name does not match the link in `index.html` |
| Old content still showing | The browser has cached the old version. Refresh with Ctrl+F5 |

## 9. Credits and license

This site is based on the Nexus System template by TemplateMo, available at https://templatemo.com/tm-629-nexus-system. Photographs and code belong to their respective authors. Check the license terms on the TemplateMo website before using the template commercially or removing the credit link from the bottom bar.

The Anteiku name, the text edits and this documentation were added for a web development course project.

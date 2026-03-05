# Saga Map Redesign Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Transform the flat Wordle-like grid into a One Piece themed saga map with Log Pose search panel.

**Architecture:** Rewrite of the same single `index.html`. Add saga/arc data structures, replace flat grid rendering with saga card generation, redesign search panel as Log Pose. All CSS pure, no external assets.

**Tech Stack:** HTML/CSS/JS static, zero dependencies.

**Design doc:** `docs/plans/2026-03-05-redesign-design.md`

**Working directory:** `/Users/polpo/repos/grand-line/`

---

### Task 1: Add saga and arc data layer

**Files:**
- Modify: `/Users/polpo/repos/grand-line/index.html` (script block)

**Step 1: Add SAGAS data structure after TYPE_INFO**

Add this data right after the `TYPE_INFO` const (line ~293). This defines all 11 sagas with their arcs. Each arc has a name, start/end episode, and whether it's a filler arc.

```js
const SAGAS = [
    {
        name: 'East Blue',
        gradient: 'linear-gradient(135deg, rgba(33,158,188,0.2), rgba(45,106,79,0.15))',
        arcs: [
            { name: 'Romance Dawn', start: 1, end: 3 },
            { name: 'Orange Town', start: 4, end: 8 },
            { name: 'Syrup Village', start: 9, end: 18 },
            { name: 'Baratie', start: 19, end: 30 },
            { name: 'Arlong Park', start: 31, end: 45 },
            { name: 'Buggy Side Story', start: 46, end: 47, filler: true },
            { name: 'Loguetown', start: 48, end: 53 },
            { name: 'Warship Island', start: 54, end: 61, filler: true }
        ]
    },
    {
        name: 'Alabasta',
        gradient: 'linear-gradient(135deg, rgba(233,196,106,0.2), rgba(180,130,60,0.15))',
        arcs: [
            { name: 'Reverse Mountain', start: 62, end: 63 },
            { name: 'Whiskey Peak', start: 64, end: 67 },
            { name: 'Koby & Helmeppo', start: 68, end: 69, filler: true },
            { name: 'Little Garden', start: 70, end: 77 },
            { name: 'Drum Island', start: 78, end: 91 },
            { name: 'Alabasta', start: 92, end: 130 },
            { name: 'Post-Alabasta', start: 131, end: 135, filler: true }
        ]
    },
    {
        name: 'Sky Island',
        gradient: 'linear-gradient(135deg, rgba(255,255,255,0.15), rgba(135,206,235,0.15), rgba(244,211,94,0.1))',
        arcs: [
            { name: 'Goat Island', start: 136, end: 138, filler: true },
            { name: 'Ruluka Island', start: 139, end: 143, filler: true },
            { name: 'Jaya', start: 144, end: 152 },
            { name: 'Skypiea', start: 153, end: 195 },
            { name: 'G-8', start: 196, end: 206, filler: true }
        ]
    },
    {
        name: 'Water 7',
        gradient: 'linear-gradient(135deg, rgba(0,128,128,0.2), rgba(10,22,40,0.3))',
        arcs: [
            { name: 'Long Ring Long Land', start: 207, end: 219 },
            { name: "Ocean's Dream", start: 220, end: 224, filler: true },
            { name: "Foxy's Return", start: 225, end: 228, filler: true },
            { name: 'Water 7', start: 229, end: 263 },
            { name: 'Enies Lobby', start: 264, end: 312 },
            { name: 'Post-Enies Lobby', start: 313, end: 325 }
        ]
    },
    {
        name: 'Thriller Bark',
        gradient: 'linear-gradient(135deg, rgba(88,24,120,0.2), rgba(20,10,30,0.3))',
        arcs: [
            { name: 'Ice Hunter', start: 326, end: 336, filler: true },
            { name: 'Thriller Bark', start: 337, end: 381 },
            { name: 'Spa Island', start: 382, end: 384, filler: true }
        ]
    },
    {
        name: 'Summit War',
        gradient: 'linear-gradient(135deg, rgba(193,18,31,0.2), rgba(255,140,0,0.12), rgba(20,10,10,0.3))',
        arcs: [
            { name: 'Sabaody Archipelago', start: 385, end: 405 },
            { name: 'Amazon Lily', start: 408, end: 421 },
            { name: 'Impel Down', start: 422, end: 452 },
            { name: 'Little East Blue', start: 426, end: 429, filler: true },
            { name: 'Marineford', start: 453, end: 489 },
            { name: 'Post-War', start: 490, end: 516 }
        ]
    },
    {
        name: 'Fish-Man Island',
        gradient: 'linear-gradient(135deg, rgba(10,30,80,0.3), rgba(0,200,200,0.12))',
        arcs: [
            { name: 'Return to Sabaody', start: 517, end: 522 },
            { name: 'Fish-Man Island', start: 523, end: 574 }
        ]
    },
    {
        name: 'Dressrosa',
        gradient: 'linear-gradient(135deg, rgba(200,50,100,0.18), rgba(220,170,50,0.12))',
        arcs: [
            { name: "Z's Ambition", start: 575, end: 578, filler: true },
            { name: 'Punk Hazard', start: 579, end: 625 },
            { name: 'Caesar Retrieval', start: 626, end: 628, filler: true },
            { name: 'Dressrosa', start: 629, end: 746 }
        ]
    },
    {
        name: 'Whole Cake Island',
        gradient: 'linear-gradient(135deg, rgba(255,182,193,0.18), rgba(255,228,196,0.12))',
        arcs: [
            { name: 'Silver Mine', start: 747, end: 750, filler: true },
            { name: 'Zou', start: 751, end: 779 },
            { name: 'Marine Rookie', start: 780, end: 782, filler: true },
            { name: 'Whole Cake Island', start: 783, end: 877 }
        ]
    },
    {
        name: 'Wano Country',
        gradient: 'linear-gradient(135deg, rgba(139,0,0,0.2), rgba(10,5,5,0.3), rgba(255,183,197,0.08))',
        arcs: [
            { name: 'Levely', start: 878, end: 889 },
            { name: 'Wano Act 1', start: 890, end: 894 },
            { name: 'Cidre Guild', start: 895, end: 896, filler: true },
            { name: 'Wano Act 2', start: 897, end: 958 },
            { name: 'Wano Act 3', start: 959, end: 1085 },
            { name: "Uta's Past", start: 1029, end: 1030, filler: true }
        ]
    },
    {
        name: 'Final Saga',
        gradient: 'linear-gradient(135deg, rgba(220,220,240,0.15), rgba(100,200,255,0.1))',
        arcs: [
            { name: 'Egghead', start: 1086, end: 1155 }
        ]
    }
];
```

**Step 2: Add helper to find saga/arc for an episode**

```js
function findSagaForEp(ep) {
    for (let i = 0; i < SAGAS.length; i++) {
        const saga = SAGAS[i];
        const firstArc = saga.arcs[0];
        const lastArc = saga.arcs[saga.arcs.length - 1];
        if (ep >= firstArc.start && ep <= lastArc.end) {
            return { sagaIndex: i, saga };
        }
    }
    return null;
}
```

**Step 3: Verify in browser console**

```js
SAGAS.length           // 11
SAGAS[0].name          // "East Blue"
SAGAS[0].arcs.length   // 8
findSagaForEp(1)       // { sagaIndex: 0, saga: {name: "East Blue", ...} }
findSagaForEp(337)     // { sagaIndex: 4, saga: {name: "Thriller Bark", ...} }
findSagaForEp(1155)    // { sagaIndex: 10, saga: {name: "Final Saga", ...} }
```

**Step 4: Commit**

```bash
git add index.html
git commit -m "Add saga and arc data layer"
```

---

### Task 2: Redesign search panel HTML + CSS as Log Pose

**Files:**
- Modify: `/Users/polpo/repos/grand-line/index.html` (HTML + CSS)

**Step 1: Replace search panel HTML**

Replace the entire `<section id="search-panel">...</section>` with:

```html
<section id="search-panel">
    <div class="log-pose">
        <div class="log-pose-ring">
            <input type="number" id="ep-input" min="1" max="1155" placeholder="#" inputmode="numeric">
            <div class="log-pose-needle" id="log-pose-needle"></div>
        </div>
        <div class="log-pose-label">LOG POSE</div>
    </div>
    <div id="result-card" class="result-card hidden">
        <div class="result-saga" id="result-saga"></div>
        <div class="result-current" id="result-current"></div>
        <div class="result-next" id="result-next"></div>
        <div class="result-skip" id="result-skip"></div>
    </div>
    <div class="legend">
        <span class="legend-item"><span class="dot" style="background:#2d6a4f"></span>Canon</span>
        <span class="legend-item"><span class="dot" style="background:#c1121f"></span>Filler</span>
        <span class="legend-item"><span class="dot" style="background:#e9c46a"></span>Mixed</span>
        <span class="legend-item"><span class="dot" style="background:#219ebc"></span>Anime</span>
    </div>
</section>
```

**Step 2: Replace search panel CSS**

Replace all CSS from `/* --- Search Panel --- */` through the legend section with:

```css
/* --- Search Panel --- */
#search-panel {
    flex-shrink: 0;
    padding: 16px 16px 10px;
    padding-top: calc(16px + env(safe-area-inset-top, 0px));
    background: #0a1628;
    border-bottom: 1px solid rgba(255,255,255,0.1);
    text-align: center;
}

/* --- Log Pose --- */
.log-pose {
    margin-bottom: 12px;
}

.log-pose-ring {
    position: relative;
    width: 100px;
    height: 100px;
    margin: 0 auto 6px;
    border: 3px solid #f4d35e;
    border-radius: 50%;
    box-shadow: 0 0 20px rgba(244,211,94,0.15), inset 0 0 15px rgba(244,211,94,0.05);
    display: flex;
    align-items: center;
    justify-content: center;
}

.log-pose-ring #ep-input {
    width: 60px;
    height: 60px;
    padding: 0;
    font-size: 22px;
    font-weight: 700;
    font-family: inherit;
    background: transparent;
    border: none;
    border-radius: 50%;
    color: #fff;
    text-align: center;
    outline: none;
    -moz-appearance: textfield;
}

.log-pose-ring #ep-input::-webkit-inner-spin-button,
.log-pose-ring #ep-input::-webkit-outer-spin-button {
    -webkit-appearance: none;
}

.log-pose-ring #ep-input::placeholder {
    color: rgba(255,255,255,0.25);
    font-weight: 400;
    font-size: 18px;
}

.log-pose-needle {
    position: absolute;
    bottom: -8px;
    left: 50%;
    transform: translateX(-50%);
    width: 0;
    height: 0;
    border-left: 6px solid transparent;
    border-right: 6px solid transparent;
    border-top: 10px solid #f4d35e;
    transition: transform 0.3s ease;
}

.log-pose-label {
    font-size: 9px;
    letter-spacing: 3px;
    text-transform: uppercase;
    opacity: 0.4;
}

/* --- Result Card --- */
.result-card {
    background: rgba(255,255,255,0.06);
    border-radius: 12px;
    padding: 10px 14px;
    margin: 0 auto 10px;
    max-width: 320px;
    text-align: left;
}

.result-card.hidden {
    display: none;
}

.result-saga {
    font-size: 10px;
    text-transform: uppercase;
    letter-spacing: 2px;
    opacity: 0.5;
    margin-bottom: 4px;
}

.result-current {
    font-size: 16px;
    font-weight: 700;
    margin-bottom: 4px;
}

.result-next, .result-skip {
    font-size: 12px;
    opacity: 0.6;
    margin-top: 3px;
    cursor: pointer;
}

.result-next:hover, .result-skip:hover {
    opacity: 1;
}

.type-tag {
    display: inline-block;
    padding: 1px 6px;
    border-radius: 3px;
    font-size: 10px;
    font-weight: 700;
    letter-spacing: 1px;
    vertical-align: middle;
    margin-left: 4px;
}

/* --- Legend --- */
.legend {
    display: flex;
    justify-content: center;
    gap: 10px;
    font-size: 9px;
    opacity: 0.5;
}

.dot {
    display: inline-block;
    width: 7px;
    height: 7px;
    border-radius: 50%;
    margin-right: 3px;
    vertical-align: middle;
}
```

**Step 3: Verify in browser**

Should see a gold circle with input inside, needle pointing down, "LOG POSE" label, then result card and legend below.

**Step 4: Commit**

```bash
git add index.html
git commit -m "Redesign search panel as Log Pose"
```

---

### Task 3: Add saga card CSS

**Files:**
- Modify: `/Users/polpo/repos/grand-line/index.html` (CSS)

**Step 1: Replace grid panel CSS**

Replace everything from `/* --- Grid Panel --- */` through `.cell.selected` with:

```css
/* --- Saga Map --- */
#saga-map {
    flex: 1;
    overflow-y: auto;
    overflow-x: hidden;
    padding: 16px 12px;
    padding-bottom: calc(16px + env(safe-area-inset-bottom, 0px));
    -webkit-overflow-scrolling: touch;
    scrollbar-width: none;
}

#saga-map::-webkit-scrollbar {
    display: none;
}

/* --- Saga Card --- */
.saga-card {
    border-radius: 16px;
    padding: 16px;
    margin-bottom: 0;
    position: relative;
    overflow: hidden;
}

.saga-header {
    margin-bottom: 12px;
}

.saga-name {
    font-size: 18px;
    font-weight: 800;
    letter-spacing: 1px;
    text-transform: uppercase;
}

.saga-range {
    font-size: 10px;
    opacity: 0.5;
    letter-spacing: 1px;
    margin-top: 2px;
}

/* --- Arc Section --- */
.arc-section {
    margin-bottom: 10px;
}

.arc-section:last-child {
    margin-bottom: 0;
}

.arc-label {
    font-size: 10px;
    font-weight: 600;
    letter-spacing: 1px;
    margin-bottom: 6px;
    opacity: 0.7;
}

.arc-label.filler-arc {
    opacity: 0.4;
    font-style: italic;
}

.arc-label.filler-arc::after {
    content: ' (filler)';
    font-weight: 400;
}

/* --- Episode Grid (inside arcs) --- */
.arc-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, 28px);
    gap: 3px;
}

.cell {
    width: 28px;
    height: 28px;
    border-radius: 4px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 9px;
    font-weight: 600;
    cursor: pointer;
    user-select: none;
    -webkit-user-select: none;
    transition: outline 0.15s;
    color: rgba(255,255,255,0.7);
}

.cell:active {
    transform: scale(0.92);
}

.cell.selected {
    outline: 2px solid #fff;
    outline-offset: 1px;
    color: #fff;
}

/* --- Route Line (between saga cards) --- */
.route-line {
    height: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.route-line::before {
    content: '';
    width: 2px;
    height: 100%;
    background: repeating-linear-gradient(
        to bottom,
        rgba(255,255,255,0.15) 0px,
        rgba(255,255,255,0.15) 4px,
        transparent 4px,
        transparent 8px
    );
}
```

**Step 2: Verify — CSS is loaded (no visual change yet since HTML still uses old structure)**

**Step 3: Commit**

```bash
git add index.html
git commit -m "Add saga card and arc section CSS"
```

---

### Task 4: Rewrite rendering to generate saga map

**Files:**
- Modify: `/Users/polpo/repos/grand-line/index.html` (HTML + JS)

**Step 1: Replace grid panel HTML**

Replace `<section id="grid-panel"><div id="grid" class="grid"></div></section>` with:

```html
<section id="saga-map"></section>
```

**Step 2: Replace the entire grid rendering section in JS**

Replace everything from `// --- Grid Rendering ---` through `renderGrid();` with:

```js
// --- Saga Map Rendering ---
const sagaMap = document.getElementById('saga-map');
const cells = [];
const sagaElements = [];

function renderSagaMap() {
    const fragment = document.createDocumentFragment();

    SAGAS.forEach((saga, sagaIndex) => {
        // Route line between cards (except before first)
        if (sagaIndex > 0) {
            const line = document.createElement('div');
            line.className = 'route-line';
            fragment.appendChild(line);
        }

        // Saga card
        const card = document.createElement('div');
        card.className = 'saga-card';
        card.style.background = saga.gradient;
        card.dataset.sagaIndex = sagaIndex;

        // Header
        const firstEp = saga.arcs[0].start;
        const lastEp = saga.arcs[saga.arcs.length - 1].end;
        const totalEps = lastEp - firstEp + 1;
        card.innerHTML = `
            <div class="saga-header">
                <div class="saga-name">${saga.name}</div>
                <div class="saga-range">EP ${firstEp}–${lastEp} · ${totalEps} episodes</div>
            </div>
        `;

        // Arc sections
        saga.arcs.forEach(arc => {
            const section = document.createElement('div');
            section.className = 'arc-section';

            const label = document.createElement('div');
            label.className = 'arc-label' + (arc.filler ? ' filler-arc' : '');
            label.textContent = arc.name;
            section.appendChild(label);

            const grid = document.createElement('div');
            grid.className = 'arc-grid';

            for (let i = arc.start; i <= arc.end; i++) {
                const cell = document.createElement('div');
                cell.className = 'cell';
                cell.textContent = i;
                cell.style.background = TYPE_INFO[episodes[i]].color;
                cell.dataset.ep = i;
                grid.appendChild(cell);
                cells[i] = cell;
            }

            section.appendChild(grid);
            card.appendChild(section);
        });

        fragment.appendChild(card);
        sagaElements[sagaIndex] = card;
    });

    sagaMap.appendChild(fragment);
}

renderSagaMap();
```

**Step 3: Update selectEpisode to show saga name and scroll to saga card**

In the `selectEpisode` function, after `cells[ep].classList.add('selected');`, add:

```js
// Show saga name
const found = findSagaForEp(ep);
if (found) {
    document.getElementById('result-saga').textContent = found.saga.name;
}
```

Replace the `scrollIntoView` at the end of `selectEpisode`:

```js
// Old: cells[ep].scrollIntoView({ behavior: 'smooth', block: 'center' });
// New:
cells[ep].scrollIntoView({ behavior: 'smooth', block: 'nearest' });
```

**Step 4: Update bidirectional linking**

Replace `grid.addEventListener('click', ...)` with:

```js
sagaMap.addEventListener('click', (e) => {
    const cell = e.target.closest('.cell');
    if (!cell) return;
    const ep = parseInt(cell.dataset.ep);
    epInput.value = ep;
    selectEpisode(ep);
});
```

**Step 5: Verify in browser**

- Should see 11 saga cards with themed gradients, arc labels, and episode grids
- Dashed route lines between cards
- Search input still works, now shows saga name
- Clicking cells updates search panel

**Step 6: Commit**

```bash
git add index.html
git commit -m "Replace flat grid with saga map rendering"
```

---

### Task 5: Log Pose needle animation + desktop responsive

**Files:**
- Modify: `/Users/polpo/repos/grand-line/index.html` (CSS + JS)

**Step 1: Add needle rotation on episode change**

In `selectEpisode`, after the saga lookup, add:

```js
// Animate Log Pose needle
const needle = document.getElementById('log-pose-needle');
const angle = (ep / TOTAL_EPISODES) * 360 - 180;
needle.style.transform = `translateX(-50%) rotate(${angle}deg)`;
```

**Step 2: Update desktop media query**

Replace the `@media (min-width: 768px)` block with:

```css
@media (min-width: 768px) {
    body {
        display: flex;
        justify-content: center;
    }

    #app {
        max-width: 480px;
        height: 100vh;
        border-left: 1px solid rgba(255,255,255,0.08);
        border-right: 1px solid rgba(255,255,255,0.08);
    }

    .saga-card {
        padding: 20px;
    }
}
```

**Step 3: Verify**

- Type different episodes — needle rotates proportionally
- Desktop: centered column with borders

**Step 4: Commit**

```bash
git add index.html
git commit -m "Add Log Pose needle animation and desktop layout"
```

---

### Task 6: Polish and deploy

**Files:**
- Modify: `/Users/polpo/repos/grand-line/index.html`

**Step 1: Remove any leftover old CSS/HTML references**

Check for any remaining `.grid` class CSS, `#grid-panel` references, or `#grid` references. Remove them.

**Step 2: Verify complete page**

Test in browser:
- Log Pose circle with input, needle animation
- Result card shows saga name, episode type, next, skip
- 11 saga cards with gradients and arc labels
- Episode cells colored correctly
- Bidirectional linking works (input ↔ grid)
- Route lines between cards
- Desktop responsive

**Step 3: Commit**

```bash
git add index.html
git commit -m "Polish: remove old grid references, final cleanup"
```

**Step 4: Push and deploy**

```bash
git push
export PATH="$HOME/.nvm/versions/node/v24.11.1/bin:$PATH"
npx wrangler pages deploy . --project-name=grand-line --branch=main --commit-dirty=true
```

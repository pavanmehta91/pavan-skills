# Diagram patterns

Copy a pattern, then change coordinates and labels. All patterns are inline SVG
or plain HTML that use the palette in `template.html`. Hex values in SVG:

| Token | Stroke / text | Fill |
|---|---|---|
| blue | `#2f6fde` | `#e6eefc` |
| green | `#1f8a4c` | `#e3f4ea` |
| red | `#c63d3d` | `#fbe7e7` |
| amber | `#b7791f` (text `#7a5113`) | `#fdf3dc` |
| violet | `#6b4fc8` (text `#4a3596`) | `#eee9fb` |
| gray | `#5b6475` | `#eef0f4` |
| ink | `#1c2230` | `#ffffff` |

Arrowhead marker. Define once per `<svg>`, with an id that's unique on the page
(one per color you use):

```html
<defs>
  <marker id="ab" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto">
    <path d="M0,0 L10,5 L0,10 z" fill="#2f6fde"/>
  </marker>
</defs>
```

Box sizing: width ≈ 7.5px × longest label (at 13–14px) + 24px. Title baseline
at `y + 26`, subtitle at `y + 45`, for a 60px-tall box.

---

## A. Before / after lanes

Use for **the problem**: the same kind of request on two paths, one that hits a
safeguard and one that skips it. You can also stack "before" and "after"
lanes for the same path.

```html
<svg viewBox="0 0 980 330" role="img" aria-label="Path A goes through the check, path B skips it">
  <defs>
    <marker id="ab" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#2f6fde"/></marker>
    <marker id="ar" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#c63d3d"/></marker>
  </defs>
  <!-- lane labels -->
  <text x="20" y="40" font-size="13" font-weight="600" fill="#2f6fde">Path A (protected)</text>
  <text x="20" y="215" font-size="13" font-weight="600" fill="#c63d3d">Path B (before the fix)</text>

  <!-- lane A: 4 boxes at x = 20, 260, 540, 770 -->
  <rect x="20" y="55" width="170" height="60" rx="10" fill="#e6eefc" stroke="#2f6fde"/>
  <text x="105" y="81" text-anchor="middle" font-size="14" font-weight="600">Caller</text>
  <text x="105" y="100" text-anchor="middle" font-size="12" fill="#5b6475">subtitle</text>
  <rect x="260" y="55" width="210" height="60" rx="10" fill="#fff" stroke="#2f6fde" stroke-width="2"/>
  <text x="365" y="90" text-anchor="middle" font-size="14" font-weight="600">The check</text>
  <rect x="540" y="55" width="160" height="60" rx="10" fill="#fff" stroke="#5b6475"/>
  <text x="620" y="90" text-anchor="middle" font-size="14" font-weight="600">Backend</text>
  <rect x="770" y="55" width="190" height="60" rx="10" fill="#e3f4ea" stroke="#1f8a4c"/>
  <text x="865" y="90" text-anchor="middle" font-size="14" font-weight="600">Correct result</text>
  <line x1="190" y1="85" x2="256" y2="85" stroke="#2f6fde" stroke-width="2" marker-end="url(#ab)"/>
  <line x1="470" y1="85" x2="536" y2="85" stroke="#2f6fde" stroke-width="2" marker-end="url(#ab)"/>
  <line x1="700" y1="85" x2="766" y2="85" stroke="#2f6fde" stroke-width="2" marker-end="url(#ab)"/>

  <!-- lane B: same x positions, y = 230; the skipped step is dashed -->
  <rect x="20" y="230" width="170" height="60" rx="10" fill="#fbe7e7" stroke="#c63d3d"/>
  <text x="105" y="265" text-anchor="middle" font-size="14" font-weight="600">Other caller</text>
  <rect x="260" y="230" width="210" height="60" rx="10" fill="#fff" stroke="#5b6475" stroke-dasharray="5 4"/>
  <text x="365" y="265" text-anchor="middle" font-size="14" font-weight="600">No check</text>
  <rect x="540" y="230" width="160" height="60" rx="10" fill="#fff" stroke="#5b6475"/>
  <text x="620" y="265" text-anchor="middle" font-size="14" font-weight="600">Backend</text>
  <rect x="770" y="230" width="190" height="60" rx="10" fill="#fbe7e7" stroke="#c63d3d"/>
  <text x="865" y="265" text-anchor="middle" font-size="14" font-weight="600">Wrong result</text>
  <line x1="190" y1="260" x2="256" y2="260" stroke="#c63d3d" stroke-width="2" marker-end="url(#ar)"/>
  <line x1="470" y1="260" x2="536" y2="260" stroke="#c63d3d" stroke-width="2" marker-end="url(#ar)"/>
  <line x1="700" y1="260" x2="766" y2="260" stroke="#c63d3d" stroke-width="2" marker-end="url(#ar)"/>

  <!-- callout linking the lanes -->
  <path d="M365 225 C 365 170, 365 160, 365 120" stroke="#c63d3d" stroke-width="1.5" stroke-dasharray="4 4" fill="none"/>
  <rect x="378" y="150" width="210" height="30" rx="6" fill="#fff" stroke="#c63d3d"/>
  <text x="483" y="170" text-anchor="middle" font-size="12.5" fill="#c63d3d" font-weight="600">never goes through the check</text>
</svg>
```

Follow it with a `.grid3` of `.card.red` boxes, one per concrete way the
problem happens.

---

## B. Hub and spoke

Use for **the fix overview**: one new shared component (violet) used by
several callers or enforcement points (green). Put existing users on the left
and new ones on the right.

```html
<svg viewBox="0 0 980 400" role="img" aria-label="Shared service used by one existing caller and four new call sites">
  <defs>
    <marker id="ag" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#6b4fc8"/></marker>
  </defs>
  <!-- hub -->
  <rect x="370" y="160" width="240" height="80" rx="12" fill="#eee9fb" stroke="#6b4fc8" stroke-width="2"/>
  <text x="490" y="192" text-anchor="middle" font-size="16" font-weight="700" fill="#4a3596">SharedService</text>
  <text x="490" y="214" text-anchor="middle" font-size="12" fill="#5b6475">what it owns</text>

  <!-- left: existing caller -->
  <rect x="20" y="170" width="230" height="60" rx="10" fill="#e6eefc" stroke="#2f6fde"/>
  <text x="135" y="196" text-anchor="middle" font-size="14" font-weight="600">ExistingCaller</text>
  <text x="135" y="215" text-anchor="middle" font-size="12" fill="#5b6475">behavior unchanged</text>
  <line x1="250" y1="200" x2="366" y2="200" stroke="#6b4fc8" stroke-width="2" marker-end="url(#ag)"/>

  <!-- right: spokes at y = 20, 110, 200, 290 (70 tall, 20 gap) -->
  <g font-size="13">
    <rect x="720" y="20" width="240" height="70" rx="10" fill="#fff" stroke="#1f8a4c"/>
    <text x="735" y="44" font-weight="600">① call site one</text>
    <text x="735" y="63" font-size="12" fill="#5b6475">what it covers</text>
    <!-- repeat for y = 110, 200, 290 -->
  </g>
  <!-- curved spokes from hub right edge (x=610) to each box left edge (x=716) -->
  <path d="M610 185 C 660 120, 670 60, 716 55" stroke="#6b4fc8" stroke-width="2" fill="none" marker-end="url(#ag)"/>
  <path d="M610 195 C 660 165, 680 145, 716 145" stroke="#6b4fc8" stroke-width="2" fill="none" marker-end="url(#ag)"/>
  <path d="M610 210 C 660 225, 680 235, 716 235" stroke="#6b4fc8" stroke-width="2" fill="none" marker-end="url(#ag)"/>
  <path d="M610 220 C 660 280, 670 320, 716 325" stroke="#6b4fc8" stroke-width="2" fill="none" marker-end="url(#ag)"/>

  <!-- optional banner under the hub, e.g. a feature flag -->
  <rect x="370" y="310" width="240" height="40" rx="8" fill="#fdf3dc" stroke="#b7791f"/>
  <text x="490" y="335" text-anchor="middle" font-size="12.5" fill="#7a5113" font-weight="600">Behind flag some_flag</text>
</svg>
```

---

## C. Decision flowchart

Use for **runtime mechanics**: which outcome happens for which input. Rules:

- Start: gray pill (`rx` = half the height). Questions: white diamonds with a
  blue stroke. Outcomes: colored boxes (red = fails/blocked, green = changed
  safely, blue = unchanged, gray = not applicable).
- The main path runs straight down; side exits go right; a two-way split goes
  left and right.
- Every path ends in an outcome box. Label every edge (`yes`/`no` or the value).
- Wrap it in `<div class="scroll">` and give the svg `style="min-width:760px"`.

```html
<div class="scroll">
<svg viewBox="0 0 980 400" role="img" aria-label="Decision flow" style="min-width:760px">
  <defs>
    <marker id="ak" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#5b6475"/></marker>
  </defs>
  <g font-size="13">
    <!-- start -->
    <rect x="390" y="10" width="200" height="44" rx="22" fill="#eef0f4" stroke="#5b6475"/>
    <text x="490" y="37" text-anchor="middle" font-weight="600">Request arrives</text>
    <line x1="490" y1="54" x2="490" y2="78" stroke="#5b6475" stroke-width="1.6" marker-end="url(#ak)"/>

    <!-- question: diamond centred at (490,120), half-width 150, half-height 40 -->
    <polygon points="490,80 640,120 490,160 340,120" fill="#fff" stroke="#2f6fde"/>
    <text x="490" y="116" text-anchor="middle" font-weight="600">First question</text>
    <text x="490" y="133" text-anchor="middle" font-weight="600">on two lines?</text>

    <!-- side exit to an outcome -->
    <line x1="640" y1="120" x2="736" y2="120" stroke="#5b6475" stroke-width="1.6" marker-end="url(#ak)"/>
    <text x="688" y="112" text-anchor="middle" fill="#5b6475">no</text>
    <rect x="740" y="98" width="220" height="44" rx="8" fill="#fbe7e7" stroke="#c63d3d"/>
    <text x="850" y="118" text-anchor="middle" font-weight="600" fill="#c63d3d">Blocked</text>
    <text x="850" y="134" text-anchor="middle" font-size="12" fill="#5b6475">what the user sees</text>

    <!-- main path continues down -->
    <line x1="490" y1="160" x2="490" y2="188" stroke="#5b6475" stroke-width="1.6" marker-end="url(#ak)"/>
    <text x="502" y="178" fill="#5b6475">yes</text>

    <!-- two-way split: go left/right with an elbow, then down -->
    <polygon points="490,190 620,226 490,262 360,226" fill="#fff" stroke="#2f6fde"/>
    <text x="490" y="231" text-anchor="middle" font-weight="600">Which mode?</text>
    <line x1="360" y1="226" x2="280" y2="226" stroke="#5b6475" stroke-width="1.6"/>
    <line x1="280" y1="226" x2="280" y2="300" stroke="#5b6475" stroke-width="1.6" marker-end="url(#ak)"/>
    <text x="320" y="218" text-anchor="middle" fill="#5b6475">mode A</text>
    <rect x="180" y="302" width="200" height="40" rx="8" fill="#e3f4ea" stroke="#1f8a4c"/>
    <text x="280" y="327" text-anchor="middle" font-weight="600" fill="#1f8a4c">Outcome A</text>
    <line x1="620" y1="226" x2="700" y2="226" stroke="#5b6475" stroke-width="1.6"/>
    <line x1="700" y1="226" x2="700" y2="300" stroke="#5b6475" stroke-width="1.6" marker-end="url(#ak)"/>
    <text x="660" y="218" text-anchor="middle" fill="#5b6475">mode B</text>
    <rect x="600" y="302" width="200" height="40" rx="8" fill="#e6eefc" stroke="#2f6fde"/>
    <text x="700" y="327" text-anchor="middle" font-weight="600" fill="#2f6fde">Outcome B</text>
  </g>
</svg>
</div>
<p class="foot">* Footnote for the one rule the diagram can't show.</p>
```

---

## D. Set comparison (Venn)

Use for **why it is safe**: showing that a change narrows or widens a set.
Outer dashed rectangle = the set before (red if it was too wide), circles =
the constraints, green fill = the set after. Put two variants side by side in
a `.grid2` of cards.

```html
<svg viewBox="0 0 420 220" role="img" aria-label="Union of two sets inside the old, wider set">
  <rect x="10" y="10" width="400" height="200" rx="10" fill="#fbe7e7" stroke="#c63d3d" stroke-dasharray="5 4"/>
  <text x="22" y="32" font-size="12" fill="#c63d3d" font-weight="600">Before: everything</text>
  <!-- union: both circles filled -->
  <circle cx="170" cy="115" r="65" fill="#e3f4ea" stroke="#1f8a4c"/>
  <circle cx="260" cy="115" r="65" fill="#e3f4ea" stroke="#1f8a4c"/>
  <text x="140" y="120" font-size="13" text-anchor="middle" font-weight="600">A</text>
  <text x="290" y="120" font-size="13" text-anchor="middle" font-weight="600">B</text>
  <text x="215" y="202" font-size="12" text-anchor="middle" fill="#1f8a4c" font-weight="600">After: A OR B (green)</text>
</svg>
```

For an intersection, draw both circles white and fill only the lens. For
centres (170,115) and (260,115) with r=65, the lens points are at x=215,
y = 115 ± √(65² − 45²) ≈ 68.1 and 161.9:

```html
<path d="M215,68.1 A65,65 0 0,1 215,161.9 A65,65 0 0,1 215,68.1 z" fill="#e3f4ea" stroke="#1f8a4c"/>
```

---

## E. Timeline

Use for **rollout or ordered steps**. 3–5 numbered dots on a gray rail; each
step has a bold title and one or two gray lines under it.

```html
<svg viewBox="0 0 980 150" role="img" aria-label="Rollout in four steps">
  <line x1="60" y1="40" x2="920" y2="40" stroke="#d9dee7" stroke-width="4"/>
  <g font-size="13">
    <circle cx="120" cy="40" r="14" fill="#2f6fde"/>
    <text x="120" y="45" text-anchor="middle" fill="#fff" font-weight="700">1</text>
    <text x="120" y="80" text-anchor="middle" font-weight="600">Step title</text>
    <text x="120" y="98" text-anchor="middle" font-size="12" fill="#5b6475">one-line detail</text>
    <!-- repeat at cx = 400, 660, 880. Color the dots by meaning:
         blue = start, amber = observe/caution, violet = coordinate, green = done -->
  </g>
</svg>
```

Follow it with `.card.amber` boxes for the metrics or logs to watch.

---

## F. Sequence (who calls whom)

Use when **order across several actors** matters (request → worker → service →
response). Actors are boxes at the top with dashed lifelines; messages are
horizontal arrows numbered top to bottom.

```html
<svg viewBox="0 0 980 300" role="img" aria-label="Sequence between three actors">
  <defs>
    <marker id="as" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#1c2230"/></marker>
  </defs>
  <g font-size="13">
    <!-- actors at x centres 150, 490, 830 -->
    <rect x="70" y="10" width="160" height="40" rx="8" fill="#e6eefc" stroke="#2f6fde"/>
    <text x="150" y="35" text-anchor="middle" font-weight="600">Caller</text>
    <rect x="410" y="10" width="160" height="40" rx="8" fill="#eee9fb" stroke="#6b4fc8"/>
    <text x="490" y="35" text-anchor="middle" font-weight="600">Worker</text>
    <rect x="750" y="10" width="160" height="40" rx="8" fill="#fff" stroke="#5b6475"/>
    <text x="830" y="35" text-anchor="middle" font-weight="600">Service</text>
    <line x1="150" y1="50" x2="150" y2="290" stroke="#d9dee7" stroke-dasharray="4 4"/>
    <line x1="490" y1="50" x2="490" y2="290" stroke="#d9dee7" stroke-dasharray="4 4"/>
    <line x1="830" y1="50" x2="830" y2="290" stroke="#d9dee7" stroke-dasharray="4 4"/>

    <!-- messages every 50px; solid = call, dashed = reply -->
    <line x1="150" y1="90" x2="486" y2="90" stroke="#1c2230" stroke-width="1.6" marker-end="url(#as)"/>
    <text x="320" y="82" text-anchor="middle">1. enqueue(job)</text>
    <line x1="490" y1="140" x2="826" y2="140" stroke="#1c2230" stroke-width="1.6" marker-end="url(#as)"/>
    <text x="660" y="132" text-anchor="middle">2. search(filters)</text>
    <line x1="830" y1="190" x2="494" y2="190" stroke="#1c2230" stroke-width="1.6" stroke-dasharray="5 4" marker-end="url(#as)"/>
    <text x="660" y="182" text-anchor="middle">3. results</text>
  </g>
</svg>
```

Highlight the step the change affects by drawing it in red (before) or green
(after), and say why in the `.lede`.

---

## G. Plain HTML pieces

**Scenario cards** (problem cases, gaps, metrics):

```html
<div class="grid3">
  <div class="card red"><h3>Short case name</h3><p>One or two sentences, concrete.</p></div>
</div>
```

**Yes/no scope table**:

```html
<table>
  <thead><tr><th>Item</th><th>Covered?</th><th>Why</th></tr></thead>
  <tbody>
    <tr><td>Case</td><td class="yes">Yes</td><td>Reason</td></tr>
    <tr><td>Case</td><td class="no">No</td><td>Reason</td></tr>
  </tbody>
</table>
```

**Mode comparison table with pills**: the first column holds the outcome
pill (`.pill.g/.r/.b/.a/.v/.n`), then one column per mode.

**Callouts**: `.note` (amber) for caveats and things to do; `.finding` (blue)
for your own findings that aren't in the source.

**Code shapes** (call tree, file tree, diff): use `<pre><code>` with box-drawing
characters. Keep only the lines the point needs.

---

## H. Same layout, three states (today → breaks → proposed)

Use in **Explore** and **Propose** modes. Draw today's flow once, then repeat
the *identical* layout (same boxes, same coordinates) with only the colors
changed, so the eye sees exactly what differs. Put each state in its own
labelled block, stacked vertically.

1. **Today:** every box white with a gray stroke, arrows gray. No judgement.
2. **Where it breaks:** same layout; the faulty step gets a red fill, red
   stroke, and a short red callout ("skips the check", "reads stale value").
   Everything else stays gray.
3. **Proposed:** same layout; new steps are inserted as green boxes, changed
   steps get a green stroke, removed steps become gray dashed with a strike
   (`text-decoration` doesn't work in SVG; draw a `<line>` across the label).
   Anything the user didn't specify is an unknown box (J).

```html
<h3 class="sub">Today</h3>
<svg viewBox="0 0 980 110" role="img" aria-label="Today's flow">
  <defs><marker id="h1" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="7" markerHeight="7" orient="auto"><path d="M0,0 L10,5 L0,10 z" fill="#5b6475"/></marker></defs>
  <g font-size="14" font-weight="600" text-anchor="middle">
    <rect x="20" y="25" width="200" height="60" rx="10" fill="#fff" stroke="#5b6475"/><text x="120" y="60">Step 1</text>
    <rect x="270" y="25" width="200" height="60" rx="10" fill="#fff" stroke="#5b6475"/><text x="370" y="60">Step 2</text>
    <rect x="520" y="25" width="200" height="60" rx="10" fill="#fff" stroke="#5b6475"/><text x="620" y="60">Step 3</text>
    <rect x="770" y="25" width="190" height="60" rx="10" fill="#fff" stroke="#5b6475"/><text x="865" y="60">Result</text>
  </g>
  <line x1="220" y1="55" x2="266" y2="55" stroke="#5b6475" stroke-width="2" marker-end="url(#h1)"/>
  <line x1="470" y1="55" x2="516" y2="55" stroke="#5b6475" stroke-width="2" marker-end="url(#h1)"/>
  <line x1="720" y1="55" x2="766" y2="55" stroke="#5b6475" stroke-width="2" marker-end="url(#h1)"/>
</svg>

<h3 class="sub">Where it breaks</h3>
<!-- copy the svg above; change Step 2 to fill="#fbe7e7" stroke="#c63d3d",
     Result to red, and add under Step 2:
     <text x="370" y="104" font-size="12" fill="#c63d3d" text-anchor="middle" font-weight="600">skips the check</text>
     (raise the viewBox height to 120) -->

<h3 class="sub">Proposed</h3>
<!-- copy again; insert a green box between Step 1 and Step 2 by shifting
     later boxes right (or shrinking widths), stroke Step 2 green if changed,
     Result green. Keep the order and positions of untouched steps. -->
```

Keep each strip one row. If the flow has more than 5 steps, use a sequence
(F) for all three states instead, with the same "only colors change" rule.

---

## I. Option strips

Use for **comparing 2–3 candidate fixes**. One compact strip per option, all
drawn on the same base flow, each highlighting *where* that option acts. Then
a trade-off table.

```html
<div class="grid3">
  <div class="card green">
    <h3>Option A: fix at the source</h3>
    <svg viewBox="0 0 300 90" role="img" aria-label="Option A acts at step 1">
      <g font-size="12" font-weight="600" text-anchor="middle">
        <rect x="5" y="20" width="80" height="44" rx="8" fill="#e3f4ea" stroke="#1f8a4c" stroke-width="2"/><text x="45" y="47">Step 1</text>
        <rect x="110" y="20" width="80" height="44" rx="8" fill="#fff" stroke="#5b6475"/><text x="150" y="47">Step 2</text>
        <rect x="215" y="20" width="80" height="44" rx="8" fill="#fff" stroke="#5b6475"/><text x="255" y="47">Step 3</text>
      </g>
      <line x1="85" y1="42" x2="108" y2="42" stroke="#5b6475" stroke-width="1.6"/>
      <line x1="190" y1="42" x2="213" y2="42" stroke="#5b6475" stroke-width="1.6"/>
      <text x="45" y="82" font-size="11" text-anchor="middle" fill="#1f8a4c">acts here</text>
    </svg>
    <p>One sentence: what this option changes.</p>
  </div>
  <!-- Option B, C: same mini-strip, highlight a different step -->
</div>

<table style="margin-top:14px">
  <thead><tr><th></th><th>Option A</th><th>Option B</th><th>Option C</th></tr></thead>
  <tbody>
    <tr><td>Fixes the symptom?</td><td class="yes">Yes</td><td class="yes">Yes</td><td class="no">Partly</td></tr>
    <tr><td>Blast radius</td><td><span class="pill a">all callers</span></td><td><span class="pill g">one path</span></td><td><span class="pill g">one path</span></td></tr>
    <tr><td>Effort</td><td>…</td><td>…</td><td>…</td></tr>
    <tr><td>Main risk</td><td>…</td><td>…</td><td>…</td></tr>
    <tr><td>Decided by</td><td colspan="3">Open question #2</td></tr>
  </tbody>
</table>
```

---

## J. Unknown box

Use **anywhere** a step, value, or behavior hasn't been verified or the user
hasn't decided it. Gray dashed border, a "?" badge, and a question as the
label. Number unknowns so the list in "Open questions" can refer to them.

```html
<g>
  <rect x="270" y="25" width="200" height="60" rx="10" fill="#fff" stroke="#5b6475" stroke-dasharray="6 4"/>
  <circle cx="462" cy="25" r="11" fill="#5b6475"/>
  <text x="462" y="29" text-anchor="middle" font-size="12" fill="#fff" font-weight="700">?1</text>
  <text x="370" y="52" text-anchor="middle" font-size="13" font-weight="600">Who retries?</text>
  <text x="370" y="70" text-anchor="middle" font-size="11.5" fill="#5b6475">not specified yet</text>
</g>
```

Then, in the Open questions section:

```html
<ol>
  <li><b>?1 Who retries?</b> The proposal doesn't say whether the worker or the caller retries. This decides whether duplicates are possible.</li>
</ol>
```

## Choosing a pattern

| To show… | Use |
|---|---|
| What is broken / which path skips a safeguard | A. Before/after lanes |
| One new component used in several places | B. Hub and spoke |
| Which outcome happens for which input | C. Decision flowchart |
| A change narrows or widens a set | D. Set comparison |
| Steps in time, rollout | E. Timeline |
| Order of calls between actors | F. Sequence |
| 3+ comparable facts | G. Table |
| Today vs. where it breaks vs. a proposal, on one layout | H. Three states |
| 2–3 candidate fixes side by side | I. Option strips + trade-off table |
| Something not yet known or decided | J. Unknown box |

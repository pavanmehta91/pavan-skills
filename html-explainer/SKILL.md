---
name: html-explainer
description: >-
  Build the user's mental model of a problem, a flow, or a change as one
  self-contained, light-themed HTML page that leads with diagrams: current
  flows, where things break, decision flowcharts, option comparisons, and the
  user's own proposed solution drawn next to today's behavior. Three modes:
  explain an existing change (ticket/PR), explore a problem that has no
  solution yet and brainstorm options, or draw and stress-test a solution the
  user proposes. Use when the user asks to "explain this visually", "make an
  HTML explainer", "show me with diagrams / flowcharts", "draw this", "help me
  understand the problem", "map the flow", "here is my proposed solution,
  draw it", or wants a ticket, PR, or long comment turned into something
  easier to read than text. Do not use for a quick inline diagram in chat
  (use show-me), for PR correctness review (use pr-html-review), or for data
  dashboards with charts.
disable-model-invocation: true
---

# HTML Explainer

The goal is the user's **mental model**: after looking at the page they should
be able to redraw the flow from memory and say where it breaks and why. A
reader should get there by looking at the pictures and reading only the
one-line captions. Prose exists to label and connect the visuals, not to carry
the explanation.

## 0. Pick the mode

| Mode | The user has… | The page ends with… |
|---|---|---|
| **Explain** | a ticket/PR/comment that already contains a solution | what's pending and the gaps |
| **Explore** | a problem, maybe a symptom or a ticket with no fix yet | candidate options and the open questions that decide between them |
| **Propose** | a problem *and* their own idea for a fix ("this is what I'm thinking") | their proposal drawn against today's flow, what it changes, what it risks, what's still unknown |

Infer the mode from the request. If it's unclear whether the user wants
options from you or wants their own idea drawn, ask that one question. The
modes chain: an Explore page often becomes a Propose page once the user picks a
direction. When that happens, **update the same file** (same path) by adding
sections, rather than starting a new page, so the problem and the proposal
stay side by side.

## 1. Gather the facts first

- Read the real source: the ticket, its comments, the PR description, the diff,
  the code it touches. Use real names (classes, flags, metrics, endpoints).
- Separate what the source states from what you inferred. Put your own
  findings (risks, gaps, things that changed since) in a clearly labelled
  callout, never mixed into the author's story.
- Do not invent behavior. If a box in a diagram would need a guess, verify it
  in code or leave it out.

## 2. Pick the story, then the sections

Drop any section the source doesn't support; never pad.

### Explain mode (an existing change)

| # | Section | Answers | Default visual |
|---|---|---|---|
| 1 | Problem | What is broken, and for whom? | Before lanes (pattern A) + 2-3 red scenario cards |
| 2 | Fix overview | What changes, and where? | Hub-and-spoke (B) or after lanes (A) |
| 3 | Mechanics | What happens at runtime, step by step? | Decision flowchart (C) or sequence (F) |
| 4 | Why it is safe / correct | The one non-obvious argument | Set comparison (D) or worked example |
| 5 | Scope | What is in and out, and why | Table with yes/no column |
| 6 | Modes / flags | How behavior differs by setting | Two-column comparison table with pills |
| 7 | Rollout / timeline | What happens in what order | Timeline (E) + metric cards |
| 8 | Gaps and next actions | What is not done, what the reader does next | Amber cards + note callout |

### Explore mode (a problem, no solution yet)

Spend most of the page on **today's flow**. A good problem picture usually
makes the options obvious.

| # | Section | Answers | Default visual |
|---|---|---|---|
| 1 | Symptom | What does someone see go wrong, in one concrete example? | 2-3 red scenario cards with real values |
| 2 | Today's flow | How does it work right now, end to end? | Lanes (A) or sequence (F), drawn from the code, nothing red yet |
| 3 | Where it breaks | Which step lets the symptom happen? | The same diagram again with the faulty step marked red (pattern H) |
| 4 | Actors and constraints | Who and what is involved; what can't change? | Hub (B) or a small table |
| 5 | Root cause vs. contributing factors | The one thing that, if fixed, removes the symptom | Short list + a `.finding` box for anything inferred |
| 6 | Options | 2-3 distinct ways to fix it | Option strips (pattern I) + trade-off table |
| 7 | Open questions | What must be answered before choosing? | Unknown boxes (pattern J) + a numbered list |

Options must differ in *where* they act (different step, different
component), not just in wording. Mark each option's cost, risk, and blast
radius in the trade-off table. Give a recommendation only if the facts support
one; otherwise say which open question decides it.

### Propose mode (the user's own solution)

The user's idea is the subject. Draw it faithfully first, critique second.

| # | Section | Answers | Default visual |
|---|---|---|---|
| 1 | The problem, briefly | What is being fixed? | One "where it breaks" diagram (H), reused from Explore if it exists |
| 2 | Your proposal | What the user described, as a picture | Today's flow and the proposed flow drawn on the same layout (pattern H), new or changed steps in green |
| 3 | What changes for whom | Which callers, users, data paths behave differently | Table: path · today · with proposal |
| 4 | Walk-throughs | Does it handle the real cases? | The symptom cards from Explore, each traced through the proposed flow (decision flowchart C) |
| 5 | Risks and edge cases | What could go wrong, what it doesn't cover | Amber cards; red cards only for things that break |
| 6 | Alternatives (optional) | Is there a smaller or safer variant? | Option strips (I), only if one is clearly worth comparing |
| 7 | Unknowns and next steps | What to verify before building | Unknown boxes (J) + a numbered checklist |

Rules for Propose mode:

- Draw what the user said, in their words. If their description has a gap
  (a step they didn't mention), draw it as an unknown box (J) and ask, don't
  fill it with your own design.
- Keep "your proposal" and "my suggestions" visibly separate: the user's
  design in green, your additions in the `.finding` style, labelled as yours.
- Verify the proposal against the code: if it assumes a hook, field, or call
  that doesn't exist, mark it red and say so.
- When the user revises the idea, update the same page: replace the proposal
  section and keep a one-line "v1 → v2: what changed" note under its heading.

### Concept (not a problem or a change)

What it is for → the boundary (what goes in, what comes out) → how it works →
variations / edge cases → when not to use it.

Each section gets: a numbered `h2`, one `.lede` sentence that states the
point, then the visual, then at most a few short cards or a footnote.

## 3. Build the page

1. Save where the user's global instructions (CLAUDE.md / AGENTS.md) say
   generated HTML goes; if they say nothing, ask. When updating a page from
   earlier in the conversation, write to the same path.
2. Copy `template.html` from this skill's directory into that path. Keep its
   `<style>` block and color tokens; add page-specific CSS only if needed.
3. Write each section using the patterns in `diagrams.md`. Copy a pattern,
   then change coordinates and labels; don't draw from scratch.
4. Footer: say what the page was generated from (ticket key, PR number, branch,
   date of the source).

### Visual rules

- **Inline SVG by default.** No Mermaid, no chart libraries, no web fonts, no
  external requests. The file must open offline.
- **Light only.** `color-scheme: light`; no `prefers-color-scheme` dark block,
  no theme toggle.
- **One color meaning per page**, used everywhere (boxes, pills, card borders):

  | Color | Meaning |
  |---|---|
  | red | broken / before / failure / blocked |
  | green | fixed / allowed / success / after |
  | blue | the normal path, the user-facing surface |
  | violet | the shared core component the change introduces |
  | amber | flags, caution, rollout, known gaps |
  | gray | neutral, unchanged, out of scope |
  | gray dashed + "?" | unknown, not verified, needs an answer |

- **Short labels.** A box holds a 1-3 word title and an optional 3-5 word
  subtitle. At font-size 13-14 assume ~7.5px per character; size the box to
  the longest line plus 24px. If it doesn't fit, shorten the label, don't
  shrink the font below 11.
- **SVG hygiene.** `viewBox` with `width:100%`; `role="img"` and an
  `aria-label` on every `<svg>`; marker ids unique per page (`ab`, `ar`,
  `ag`…); colors as hex values that mirror the CSS tokens (SVG presentation
  attributes don't reliably resolve `var()`).
- **Wide diagrams** (flowcharts > 700px of content) go inside `.scroll` with a
  `min-width` on the `<svg>` so phones scroll the diagram, not the page.
- **One idea per diagram.** If a diagram needs more than ~12 boxes, split it.
- Tables for anything with 3+ rows of comparable facts; never a paragraph
  listing them.

### Writing rules

- Plain words. Define a term the first time it appears, or don't use it.
- Real names in `<code>`: `RuleConfig#es_searcher`, `workflow_territory_boundary_total`.
- A caption says what to notice ("never goes through the check"), not what
  the shape is ("arrow from A to B").
- No marketing adjectives, no emoji headers.

## 4. Reply

- Give the absolute file link.
- One line per section saying what it shows (not a repeat of the content).
- Point out anything in the page that is your finding rather than the
  source's.
- Explore and Propose modes: end with the open questions, and ask the one
  that most changes the picture. The page is a working surface; expect to
  update it as answers come in.
- Offer, in one line, to publish it as a shareable page. Publish only if the
  user says yes.

## Smell test

You have drifted back into a text document if:

- a section has more than ~4 lines of prose before its first visual;
- the problem section has no "before" picture;
- in Explore mode, options appear before today's flow has been drawn;
- in Propose mode, the user's idea was redesigned instead of drawn as they
  described it, or a gap in it was filled silently instead of marked unknown;
- a flowchart has no outcome boxes (every path must end in a colored result);
- the same color means two different things on the page;
- the reader has to read the footer or a callout to understand the main point.

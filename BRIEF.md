# Self Development App Set: Build Brief

Handoff document for continuing this project in Claude Code. Drop this file in the repo root and point Claude Code at it.

---

## 1. What this project is

A set of small, self contained self development web apps. Each one is a single HTML file with no build step, no dependencies and no backend. Each one does exactly one job well and has its own visual identity.

The set is intended to be sold or bundled, so every app must stand alone and must not look like a recolored copy of the others.

**Current status: 6 shipped, more in the queue.**

---

## 2. Hard rules

These are not preferences. Break any of them and the output is rejected.

1. **No em dashes.** Not in copy, not in UI text, not in code comments, not in this or any other document. Use commas, colons, periods or a rewrite.
2. **Every app gets its own visual identity.** New palette, new type pairing, new layout logic. There is no house style to fall back on.
3. **Never reuse the warm dark neo brutalist look** (orange `#ff7a18`, Sora, JetBrains Mono, Plus Jakarta Sans). That is one option among many, and it is currently off the table because it has been used elsewhere.
4. **When supplied copy exists, reproduce it verbatim.** No rewording, retitling, restructuring, synonym swaps or casing changes. Suggestions go in a separate note, never applied silently.
5. **One file per app.** All CSS and JS inline. No frameworks, no npm, no CDN except Google Fonts.

---

## 3. Technical spec

| Item | Rule |
|---|---|
| Output | Single `.html` file, lowercase hyphenated filename |
| CSS | Inline `<style>`, CSS custom properties for all tokens |
| JS | Vanilla, inline `<script>`, no libraries |
| Fonts | Google Fonts only, always with a system fallback stack |
| Width | Mobile first, content column max 540px to 560px |
| Storage | See section 4 |
| Accessibility | Visible `:focus-visible`, `aria-label` on icon buttons, `prefers-reduced-motion` respected |
| Browser support | Evergreen only. `aspect-ratio`, `:is()`, custom properties are all fair game |

### CSS discipline

Watch selector specificity collisions, particularly between a type selector like `.section` and an element selector like `.cta`. Section padding and margin rules cancelling each other out is the most common failure in these builds.

---

## 4. Storage layer

Two targets, one swap.

**A. Claude artifact preview.** Uses `window.storage`. Never `localStorage`, which fails inside Claude artifacts.

**B. Self hosted or sold version.** Uses `localStorage`.

Every app already wraps storage in a `store` object, so the swap is one block. To convert an app for self hosting, replace the `store` object with this:

```js
const store = {
  async load(){
    try{
      const raw = localStorage.getItem(KEY);
      if(raw) return JSON.parse(raw);
    }catch(e){}
    return null;
  },
  async save(d){
    try{ localStorage.setItem(KEY, JSON.stringify(d)); }
    catch(e){ console.error("Could not save", e); }
  }
};
```

Nothing else in the app changes. Keep the `async` signatures so the call sites stay identical.

Each app owns one storage key, namespaced and versioned: `blueprint:v1`, `switchback:v1`, `longhand:v1`, `thegap:v1`, `counterweight:v1`, `carryover:v1`. All state batched into that one key, never one key per record.

---

## 5. Design rules

Approach each app as a separate client with a separate brand.

**Process per app:** decide the concept first, then derive the palette and type from the concept's own world. Never pick colors first.

**Token system.** Before writing code, define:
- 4 to 6 named hex values
- a display face, a body face, and a utility face if the app needs data or labels
- a layout concept in one sentence
- one signature element the app is remembered by

**Signature element.** Every app needs one memorable device that encodes something true about the content. Not decoration. Examples already used are in section 6.

**Defaults to avoid.** These read as generic AI output and are banned unless a brief explicitly asks for them:
- cream background near `#F4F1EA` with a high contrast serif and a terracotta accent near `#D97757`
- near black background with a single acid green or vermilion accent
- broadsheet layout with hairline rules, zero border radius and dense newspaper columns
- numbered markers `01 / 02 / 03` when the content is not actually a sequence

**Restraint.** Spend boldness in one place. The signature element carries it, everything else stays quiet. Cut one thing before shipping.

### Copy voice

Words are design material. Plain verbs, sentence case, no filler. Name things by what the person controls, not by how the system works. A button that says "Save entry" produces a toast that says "Saved". Empty states are an invitation to act, not an apology. Errors say what happened and how to fix it.

---

## 6. Already shipped

Do not reuse these palettes, type pairings, or signature devices.

### 1. `blueprint.html` — daily habits
- **Job:** repetition. Tick habits, build streaks, reflect at night.
- **Palette:** deep blueprint blue `#0E2A4D`, panel `#123258`, chalk `#E8F1F8`, amber `#F2A93B`
- **Type:** Bricolage Grotesque 800 display, IBM Plex Sans body, IBM Plex Mono utility
- **Signature:** the wall. Four weeks of bricks aligned to real weekdays, each filling from the bottom as habits are ticked, turning solid only on a complete day. Dashed outline means not yet built.
- **Storage key:** `blueprint:v1`

### 2. `switchback.html` — one big goal
- **Job:** decomposition. One goal, milestones, steps, and only the single next move shown.
- **Palette:** sage `#E3E7DE`, card `#FBFCFA`, ink `#171C18`, olive `#8A9683`, raspberry `#C8305F`
- **Type:** Archivo 800 display, Inter body
- **Signature:** the black "next move" card that hides everything except the one step you can start now, plus a vertical path with a live node showing where you stand.
- **Storage key:** `switchback:v1`

### 3. `longhand.html` — guided journal
- **Job:** reflection. Mood matched prompts and a browsable archive.
- **Palette:** dusk `#191428`, panel `#221B36`, mauve `#A899C4`, rose `#E9739A`
- **Type:** Fraunces italic display, Karla body
- **Signature:** the grain. Every past entry is a horizontal bar sized by word count and colored by mood, so the archive shows the texture of your months at a glance. Tap to read back.
- **Storage key:** `longhand:v1`

### 4. `the-gap.html` — weekly values check
- **Job:** alignment. Forced ranking of values against how the week was actually lived.
- **Palette:** white, wash `#F2F2EF`, ink `#0B0B0C`, ultramarine `#2233DD`, red `#D33B3B`
- **Type:** Epilogue 800 display, Public Sans body
- **Signature:** the gap chart. A blue dot for what you said matters and a red ring for what you actually did, with the distance between them drawn in red and sorted widest first.
- **Storage key:** `thegap:v1`

### 5. `counterweight.html`, fear setting
- **Job:** decision. Name the risks in the thing you keep avoiding, plan each one down, then weigh what is left against the cost of waiting.
- **Palette:** steel `#C6CFD4`, slab `#F5F8F9`, iron `#141A1D`, fog `#7C8A91`, anchor `#0F5B52`, ember `#8E2B22`
- **Type:** Chivo 900 display, Newsreader body, Chivo Mono utility
- **Signature:** the weigh in. Two columns of plates rising off a solid baseline. Every named risk starts as a heavy white plate and darkens and shrinks as a prevention and a repair get written, while each cost of waiting stacks up in red, the three year ones hatched and counting double. A dashed risk line marks the risk total, so the only question left is whether the cost column clears it.
- **Storage key:** `counterweight:v1`

### 6. `carryover.html`, weekly review
- **Job:** honesty. Sunday retro of what moved, what stalled, what you learned and what you are dropping, with stalled items carried into next week.
- **Palette:** oat `#EFE7D2`, card `#FCF8EE`, ink `#20211C`, mute `#8B8676`, violet `#4B33C4`, clay `#A4462B`
- **Type:** Syne 800 display, Work Sans body, Azeret Mono utility
- **Signature:** the drag tail. Every stalled item carries a row of notches, one per week it has survived, the newest in clay, so an item that has been dragging a month is longer and louder than one that showed up on Tuesday. Three weeks or more and it is marked overdue.
- **Storage key:** `carryover:v1`

---

## 7. Build queue

Pick the next unbuilt item. Concepts are fixed, the visual identity is yours to invent within the rules in section 5.

**5. Fear setting. BUILT, shipped as `counterweight.html`.** A decision tool for something the person keeps avoiding. Three columns: what could go wrong, how to prevent each one, how to repair it if it happens. Then the cost of inaction over six months and three years. Signature idea: the cost of inaction should visually outweigh the risks by the end.

**6. Weekly review. BUILT, shipped as `carryover.html`.** Sunday retro. What moved, what stalled, what you learned, what you will drop. Carries forward unfinished items so the person sees how long something has been dragging.

**7. Deep work sessions.** Timer plus intention logging. Before each block you state what you are doing and why. Afterward you rate the focus. Builds a record of when your good hours actually are.

**8. Evidence log.** A counter to negative self talk. You log specific evidence of progress, tagged by area. Signature idea: it plays evidence back to you on days you log nothing.

**9. Reframe.** Catch the thought, name the distortion, write the truer version. A short library of the common distortions built in.

**10. Energy audit.** Track what drained and what filled you across a week, then show the pattern. People, tasks, places and times, split into gain and drain.

**11. Backcast.** A five year picture worked backward to what happens this month. Each layer has to connect to the one above it or it does not count.

**12. Boundary scripts.** Pick the situation, get a phrasing you can actually send. Saying no, renegotiating scope, ending a call, asking for money owed.

---

## 8. Quality checklist

Run this before declaring any app done.

- [ ] Zero em dashes anywhere in the file
- [ ] Palette and type pairing not used by any shipped app
- [ ] Signature element present, and it encodes something true about the content
- [ ] Works with an empty state, and the empty state invites action
- [ ] Works after a reload, all state persists
- [ ] Deleting the last item does not break rendering
- [ ] Readable at 360px wide
- [ ] Keyboard tab order sensible, focus ring visible
- [ ] `prefers-reduced-motion` kills all transitions
- [ ] No `localStorage` if targeting the artifact preview
- [ ] Copy is plain, active and consistent across the whole app

---

## 9. Running this in Claude Code

Suggested loop from the repo root:

```
Read BRIEF.md. Build item 5 from the queue as a single HTML file.
Follow every rule in sections 2 through 5. When done, run the section 8
checklist against your own output, fix what fails, then append the new app
to section 6 and mark item 5 built in section 7.
```

Repeat with the next queue number. Claude Code should update this file after each build so the palette and type history stays accurate and nothing gets reused by accident.

For a batch run, chain it:

```
Build queue items 5, 6 and 7 in order. After each one, update BRIEF.md
before starting the next. Do not reuse any palette or type pairing listed
in section 6.
```

---

## 10. Packaging for sale

When a batch is ready to ship:

1. Swap every `store` object to the `localStorage` version in section 4
2. Strip the wordmark into a single CSS variable and a single text node so buyers can rebrand in two edits
3. Add a short comment block at the top of each file: what it is, where to change the name, where to change the colors
4. Write a one page usage note per app: what it does, who it is for, how to install
5. Bundle: apps, usage notes, a license file, and a combined index page linking all of them

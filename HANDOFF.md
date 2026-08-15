# Handoff

Written at the end of the session that built queue items 5 through 12. Read `BRIEF.md` first, it is the source of truth for the rules and the app history. This file only covers what is not in there: what is left to do, how the apps were checked, and the conventions the last eight files share.

---

## Where this stands

Twelve apps are specified in `BRIEF.md` section 6. Eight of them are in this repo, built in this batch:

| File | Queue item |
|---|---|
| `counterweight.html` | 5, fear setting |
| `carryover.html` | 6, weekly review |
| `good-hours.html` | 7, deep work sessions |
| `receipts.html` | 8, evidence log |
| `reframe.html` | 9, thought reframing |
| `tideline.html` | 10, energy audit |
| `backcast.html` | 11, five year backcast |
| `plainly.html` | 12, boundary scripts |

The first four apps in section 6 (`blueprint`, `switchback`, `longhand`, `the-gap`) are recorded in the brief but their files are not in this repo. Their palettes, type pairings and signature devices are listed in section 6 and must not be reused.

Branch: `claude/self-dev-app-set-x3ineh`. One commit per app, each naming the queue item, the palette and the signature element.

---

## What is not done

### 1. Section 10 packaging

None of it. Every app still targets the Claude artifact preview through `window.storage`. To ship a self hosted or sold version:

- Swap the `store` object in each file for the `localStorage` version in `BRIEF.md` section 4. It is one block per file, marked with a comment directly above it, and nothing else in the app changes. Keep the `async` signatures so the call sites stay identical.
- Write the one page usage note per app.
- Add the license file.
- Build the combined index page linking all of them.

The wordmark is already a single text node in each file, and the palette is already 5 or 6 custom properties at the top of `:root`, so the rebrand edits in section 10 steps 2 and 3 are mostly done.

### 2. Em dashes in section 6 of the brief

The four original entries in section 6 use an em dash in their headings, between the file name and the label, as in the `blueprint.html` heading. That collides with hard rule 1, which bans em dashes in this and any other document. The rule about reproducing supplied copy verbatim, rule 4, says not to silently change it, so it was left alone and flagged instead. The eight entries added in this batch use a comma in the same position, so section 6 currently reads inconsistently. Normalising the four originals to commas is a one line decision for the owner of the brief.

---

## How each app was checked

Every app was driven in headless Chromium before it was committed. The checks below are the section 8 checklist made repeatable, and all eight pass them.

Run from any directory with node available:

- Playwright is installed globally, import from `/opt/node22/lib/node_modules/playwright/index.mjs` if it is not resolvable locally.
- Chromium is pre-installed at `/opt/pw-browsers`. Do not run `playwright install`.

The one piece worth keeping is the storage shim. `window.storage` only exists inside the artifact host, so tests inject a stand in before page load that backs it onto `localStorage`, which makes reload persistence testable:

```js
await page.addInitScript(() => {
  window.storage = {
    getItem: async k => window.localStorage.getItem('shim:' + k),
    setItem: async (k, v) => { window.localStorage.setItem('shim:' + k, String(v)); }
  };
});
```

What was checked per app:

- Zero em dashes in the file, and no real `localStorage` use, only the mention inside the swap comment.
- No page errors and no console errors, ignoring the Google Fonts requests, which are blocked in that sandbox. Screenshots therefore show fallback faces, not the real type.
- No horizontal overflow at 360px wide.
- An outline is actually painted on the first tab stop, and the first few tab stops are in a sensible order.
- No icon button is left without an `aria-label`, including repeated buttons like several add buttons on one page.
- Under `reducedMotion: 'reduce'`, zero elements report a non zero `transitionDuration`.
- A scripted run through the real flow of each app, then a reload, confirming state comes back.
- Deleting every item, confirming the empty state returns and nothing throws.

Defects this caught, rather than shipped: clipped text areas in `counterweight`, a `.hidden` class used in `backcast` that was never defined in its CSS so the payoff rendered twice, a grid column in `backcast` that could not shrink under a long `select`, live states in `backcast` that only updated on a full re-render, and in `plainly` an empty name fallback that produced "Thanks for thinking of me, there".

---

## Conventions the eight files share

Each app has its own identity, which is the point, but the plumbing is deliberately the same so a new one is quick to write and the batch is quick to package.

- **One storage key per app**, versioned, all state batched into it. Never one key per record.
- **Saving is debounced** by about 300ms and reports into a `#status` element with `role="status"` and `aria-live="polite"`. The message clears itself after a couple of seconds. Pass `false` instead of a message for saves the person did not ask for, so the status line does not chatter while they type.
- **Erase all is two press.** The first press arms the button and relabels it, a timer disarms it after four seconds, the second press wipes. No `confirm()`, which is unreliable inside the artifact host.
- **Text areas grow with their content.** They are `resize:none; overflow:hidden` with a `fit()` helper that sets height from `scrollHeight`. Call it after any re-render, not just on input, because heights need the node in the document. Forgetting this clips text.
- **Tints come from `color-mix`**, not from new hex values, which is what keeps each palette to 5 or 6 named tokens.
- **Every empty state is an invitation**, and deleting the last item restores it and moves focus somewhere sensible rather than leaving it on a removed node.
- **Live updates beat full re-renders** where the person is typing. Rebuilding a list mid keystroke steals the caret. `backcast.html` shows the pattern: refresh the dependent bits directly and leave the inputs alone.
- Each file opens with a comment block naming what it is, where the wordmark is, where the colours are and where the storage key is, which is section 10 step 3.

---

## Still true for anything new

`BRIEF.md` sections 2 to 5 apply in full. The short version: no em dashes anywhere, a new palette and type pairing and layout logic per app, no reuse of anything in section 6, supplied copy reproduced verbatim, one file per app with everything inline, and one signature element that encodes something true about the content rather than decorating it.

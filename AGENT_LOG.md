# Agent Log

Append-only record of automated and agent-assisted changes to this repository.
Newest entry first. No participant data, committee or faculty names, credentials,
or tokens.

---

## 2026-09-10 - Point the Friction Audit entry at live registration

PD365 published the event listing, so `secondary` now points there with the label
"Register through PD365" instead of at the companion essay. The essay keeps its own
entry in this catalog, so nothing was lost by the swap.

**Note for whoever updates this next.** PD365 titled the listing "What Your Phone Policy
Cannot Do: The Friction Audit" - inverted from the title used across the materials and in
this catalog. Left as-is here because the materials are the thing being catalogued, but
the two do differ.

---

## 2026-09-10 - Register the PD365 Friction Audit workshop

Added `pd365-friction-audit` to the `sites` array as `kind: "Talks"`, matching how the
other session decks in this catalog are classified. Placed after the K-12 Device Policy
Evidence Hub so the three device-policy projects sit together: the essay, the evidence
base, and now the workshop built on both.

`secondary` points at The Pouch and the Bypass, mirroring the evidence hub entry, because
that essay is the argument these materials operationalize.

**Verified.** Site count 16 to 17. Both `live` and `repo` URLs return 200. The entry
carries every field the schema uses, so the card renders complete rather than with a
missing summary or an absent secondary link.

**Context worth knowing.** The workshop repository was created 2026-09-09 and its
`AGENT_HANDOFF.md` documents a three-copy deck arrangement - committed source, generated
participant build, published artifact - that nothing enforces the sync of. Read that
before editing anything in that repo.

---

## 2026-08-31 - Fix a pre-existing contrast failure on faint text

Found while probing every repo after CI caught a contrast regression elsewhere. This one
**predates the token adoption**: it measured the same with the shared tokens disabled.

`--faint` (`#74807e`) passed against the page ground at 4.54 but measured **4.21** against
the panel surface, failing AA. It is used only as text - five `color:` declarations, no
borders or fills - so lifting the value carries no other visual consequence.

Lifted to `#828f8d`: **5.54** on the page ground and **5.14** on panels, still clearly
dimmer than `--muted` so the visual hierarchy is preserved.

**Verified.** 292 text-bearing elements probed: **zero** failures, down from five.
Tightest remaining pair is 4.92 against 4.5.

---

## 2026-08-31 - Adopt the shared ecosystem design tokens

**Context.** A design review across 31 repositories found 27 distinct page backgrounds,
no shared token naming, and only two class names common to more than half the sites. A
shared token layer now lives at https://minerclass.github.io/tokens.css. This repo is one
of the first two adopters, chosen as the dark-ground case.

**Changed.** `index.html` only. Linked the token file before the inline `<style>`, then
pointed this page's ground tokens at the shared ones: `--bg`, `--panel`, `--panel-2`,
`--ink`, `--muted`, `--line`. Set `--mjm-accent` to the shared teal.

**Accents stay local.** The gold, teal, rose, green, and blue used to distinguish sections
were deliberately left as this page's own values. The goal is family resemblance, not
uniformity: shared ground and ink, own character.

**Every reference carries a fallback** equal to the value this page used before adoption,
for example `--bg: var(--mjm-bg, #101415)`. A bare `var(--mjm-bg)` would be invalid at
computed-value time if the token file failed to load, which would break the page rather
than leave it unchanged.

**Verified in a real browser.** With tokens loaded, `--bg` resolves to `#111318` and the
body paints `rgb(17, 19, 24)`. With the token sheet disabled at runtime to simulate a
failed load, `--bg`, `--ink`, and `--muted` revert to exactly `#101415`, `#f2efe7`, and
`#aeb8b6`, their pre-adoption values, and the page renders as before. Zero console errors.

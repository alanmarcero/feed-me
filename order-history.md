# Order History

## Current ratchet rung

**Rung 3 — ceiling $18.60, floor $16.85.**

Rung 1 cleared 2026-09-03: $17.98 subtotal fully covered, $0.76 unused.
Rung 2 cleared 2026-09-16: $18.55 subtotal fully covered, $0.00 out of pocket, $0.15 unused. That order was built in order-mode off the live cart, which reads the true tax and subsidy and overrides the ratchet — it landed above rung 2's $18.45 estimate and still cleared. Advance to rung 4 after the next fully-covered order.

## Rotation state

Read this before building a slate.

- **Last format:** `protein-plate` (2026-09-16) — do not recommend `protein-plate` as the top pick next order.
- **Last `carb-base`:** never — available, and now overdue for its slot.
- **Formats in the last four orders:** `salad` x1, `protein-plate` x1. Still needs a third and fourth distinct format.
- **Last restaurant:** GRECO — prefer a different one if the menus allow.

## Log

| Ordered | Delivered | Restaurant | Item | Format | Subtotal | Tax | Total | Unused | Covered | Verdict |
|---|---|---|---|---|---|---|---|---|---|---|
| 2026-09-03 | 2026-09-08 | Scali Deli & Cafe | Chicken Mediterranean Salad + Grilled Chicken ($0.00) | `salad` | $17.98 | $1.26 | $19.24 | $0.76 | yes | ok-with-mods |
| 2026-09-16 | 2026-09-17 | GRECO | Bifteki Plate w/ Lemon Pilaf + Tzatziki Sauce ($1.00) | `protein-plate` | $18.55 | $1.30 | $19.85 | $0.15 | yes | pending |

## Liked

_(nothing confirmed unmodified yet)_

## Ok with mods

- **Scali — Chicken Mediterranean Salad.** Verdict 2026-09-08: "ok, too much lettuce." The Mediterranean component set is right; the greens volume is not. Recommend again only with **light lettuce or no lettuce**, and not back-to-back with another salad.

## Never Again

_(empty)_

## Component preferences

The part that generalizes across restaurants. Apply these to any menu, not just the one they came from.

**Wants:**

- tahini
- hummus
- feta
- olives
- tomato
- onion
- cucumber

Read as a pattern, not a checklist: Mediterranean and Middle Eastern builds land well. Kebab, shawarma, and grilled-protein-with-mezze shapes carry these components without a lettuce base.

**Wants less of:**

- **Lettuce.** The standing complaint. Filler that displaces real food. Order salads light-lettuce or no-lettuce where mods are accepted; do not treat lettuce volume as part of the meal.

**Grain choice, when a grain base is happening:**

Stated 2026-09-09. Whenever the build includes a grain and the restaurant offers a choice, pick **quinoa** first, **brown rice** second. Both beat white rice, and both beat leaving it to the default. This applies to grain bowls specifically but generalizes to any item with a rice-or-grain selector.

If neither is on the menu, white rice is acceptable rather than a reason to drop the item — but say in the writeup that the grain was not their pick. Where the restaurant takes modifications and the grain is incidental to the dish, asking for it swapped or served on the side is still worth a line.

**Notes-box style, stated 2026-09-16:**

Brief and courteous. One or two sentences with a please and a thank you — the kitchen is reading it mid-rush. Never repeat a selection the order already carries (side, protein, sauce, grain), and never ask about a component the dish does not contain. The 2026-09-16 GRECO note opened by asking for the lemon pilaf that was already selected; that line was dead weight.

**Wants rationed:**

- **Starch bases** — wrap, sandwich, sub, rice bowl, grain bowl. Not disliked, explicitly stated as "not every week." This is what the `carb-base` cooldown enforces. The grain-choice rule above governs *which* grain once the slot is being spent; it does not unlock the slot more often.

## Observed preferences

Derived from actual orders, not stated preferences. Update as the log grows.

- Picked Scali over Cafe Landwer and Life Alive when all three were available.
- Chose the salad-with-chicken shape over rice-based plates and sandwiches — but the follow-up feedback says that was a budget-and-macros outcome, not a format preference.
- Chose the highest-priced qualifying option rather than the cheapest.
- Life Alive was dismissed outright — its menu is vegetable-and-tofu bowls, nothing clears 40g protein. Deprioritize it; don't build options from it unless asked.

**GRECO menu intel (read 2026-09-16).** Strong match for the Mediterranean component set; its Greek salad is tomato/cucumber/red onion/Kalamata/feta with no lettuce base at all, so the standing lettuce complaint does not apply there.

- Plates ($13.95-$17.55) have no cheap protein add-ons — the modal offers only desserts and drinks. The Bifteki Plate lands at $17.55 and cannot be tuned upward except by a separate cart line.
- The Souvlaki Plate is the tunable one: $16.55 chicken, pork +$0.30, lamb +$2.30. Lamb puts it at $18.85, which breaks the $18.68 arithmetic wall at a $20.00 stipend. Chicken and pork both fit.
- Create-Your-Own Plate $12.95 with protein/side/sauce selectors tops out near $17.75 (chicken souvlaki +$3.80, extra pita +$1.00) — every other extra side overshoots.
- $1.00 sauces are the precision lever for the last few cents. The Bifteki Plate is the one plate that does not include tzatziki, so adding it is filling a real gap rather than padding.
- Bifteki Plate side choice is Greco Fries or Lemon Pilaf only — no bean or slaw option, so the grain preference has nowhere better to go there.

**Flavor Boom! intel (read 2026-09-16).** Every entree is a $13.85 rice bowl — all `carb-base`, all well under the floor, and no add-ons exist to lift them. Only the Supernova Shrimp Curry Bowl ($16.85) reaches the window, and shrimp curry is thin on protein. Rice is the only grain offered.

**B.GOOD intel (read 2026-09-16).** Nothing reaches the floor on its own: salads $13.80-$14.95, sandwiches $9.50-$13.23. Build-Your-Own Burger starts at $9.00 with protein/style/bun selectors. Useful only if a build can be stacked to the window without violating the one-item rule.

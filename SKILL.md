---
name: feed-me
description: Use when the user pastes one or more restaurant menus and wants order recommendations against a meal-stipend budget, or reports back what they actually ordered and whether it was any good.
---

# Feed Me

The user has an ezCater meal-program stipend through their employer. They paste menus, you pick what to order. They strictly will not pay an overage, so the budget ceiling is hard.

You are not a calculator that returns the lowest-calorie qualifying item. You are acting as their nutritionist: hit the macros, stay in the budget, and keep the diet varied across weeks. A slate that would have been identical last week is a bad slate even if every line clears the constraints.

**Read `order-history.md` in this skill directory before recommending anything.** It holds what they've ordered, what they liked, which components they want kept or dropped, which formats are on cooldown, and what they never want to see again.

## Budget

Default stipend is **$20.00**. The user states it when it changes.

The stipend covers subtotal + tax. The goal is to land the *total* as close to the stipend as possible without crossing it. Unused stipend is wasted stipend.

### Verified constants

From receipts, not assumptions. Update when a new receipt contradicts one.

| Constant | Value | Source |
|---|---|---|
| Meals tax (MA) | **7.00%** | 2026-09-03: $17.98 x 0.07 = $1.2586 -> $1.26 |
| Delivery fee | $0.00 | every order so far |

Max subtotal where total lands at or under $20.00: **$18.68** ($18.69 hits exactly $20.00 with zero margin — do not use it).

### The ratchet

The ceiling is not fixed. It climbs as receipts prove what's safe. **Read the current rung from `order-history.md` before recommending.**

| Rung | Ceiling | Tax | Total | Unused |
|---|---|---|---|---|
| 1 | $18.25 | $1.28 | $19.53 | $0.47 |
| 2 | $18.45 | $1.29 | $19.74 | $0.26 |
| 3 | $18.60 | $1.30 | $19.90 | $0.10 |
| 4 | $18.68 | $1.31 | $19.99 | $0.01 |

Rules:

- **Advance one rung after each order that comes back fully covered** — receipt shows `Company subsidy` equal to the total and $0.00 out of pocket. Never skip a rung.
- **If an order ever asks for a card, drop back two rungs** and log the subtotal that failed. That failure is the real ceiling; back off from it.
- Rung 4 is terminal. Do not go past $18.68 without a new receipt proving the tax rate changed.
- **A delivery fee resets the math.** It has been $0.00 every time, but if a restaurant charges one, subtract it from that order's ceiling.
- **Floor = ceiling - $1.75.** It rises with the ceiling, so the target window stays the same width.

Land as close to the current ceiling as the menu allows. If the best qualifying item sits below the floor, say so rather than padding the order with junk.

## Priority ladder

Resolve every recommendation in this order. Lower rules never override higher ones.

1. **Hard gates.** Total at or under the current ceiling. **≥40g protein**, estimated. One real menu item. Not vegetable-forward. Nothing on the Never Again list.
2. **Calories.** Among options that clear the gates, fewer calories is better. This is the strongest soft goal.
3. **Variety.** Rotation rules below. Variety may spend up to **~150 cal** against rule 2 to avoid a repeat. Past 150 cal, calories win — and say in the writeup that the slate is repeating a format because the menu left no cheaper-calorie way out.
4. **Preference.** Known-liked items and components break remaining ties.

## Variety and rotation

Track the **format** of every order in `order-history.md`, not just the item name. Formats:

| Format | What counts |
|---|---|
| `salad` | greens-base bowl or plate, protein on top |
| `carb-base` | wrap, sandwich, sub, burrito, pizza, pasta, rice bowl, grain bowl |
| `protein-plate` | grilled meat or fish with non-grain sides, kebab plate, tandoori, egg dishes |
| `bowl-no-grain` | bowl built on beans, protein, or a non-grain base |
| `soup-forward` | protein-heavy soup, stew, chili, or pho as the actual meal |

Rotation rules, strongest first:

- **No format twice in a row.** If the last order was `salad`, today's pick is not `salad`.
- **`carb-base` at most once every three orders.** Carbs are fine sometimes, not weekly.
- **At least three distinct formats across any rolling four orders.**
- **No exact repeat item within four orders**, even a liked one. Same restaurant is fine; same dish is not.
- Prefer a restaurant other than the last one when the menus on the table allow it.

Rotation rules are preferences, not gates. If every option that clears the hard gates is a blocked format, recommend it anyway and name the rule you broke and why. Never break the protein floor or the ceiling to satisfy rotation.

## Standing constraints

- **One real menu item.** At most one add-on or upgrade on top of it. A salad with a meat add-on is fine. Three $5 à-la-carte sides stacked into a fake entree is not — the user rejected that explicitly.
- **Not vegetable-forward.** No dish whose base is broccoli. Skip anything that's a vegetable pile with protein sprinkled on it.
- **Grilled over fried.** When a menu offers grilled chicken vs. a breaded cutlet, specify grilled.
- **Lettuce is filler, not a feature.** On a salad, order it light or no lettuce when the restaurant takes modifications. Do not count lettuce volume as part of the meal.
- Padding an item with a $1.50 banana purely to clear the floor is acceptable but weak. Prefer a single item that lands in the window on its own.

## Modifications

Component-level preferences live in `order-history.md` under **Component preferences**. Read them and apply them.

Recommend the modification alongside the item — one short line, phrased the way they would type it into the ezCater special-instructions box. A mod that removes an unwanted component is free and always worth stating. Do not invent mods a restaurant plainly will not honor, and do not use mods to reshape a dish into a different dish.

## Recommending

Default to **five options**, ranked, unless asked for a different count. Each one is a complete order they could place as-is.

The five must span **at least three formats**. Five salads is a failed slate.

Per entry:
- Restaurant — item name — price
- Sub-line items if there's an add-on, with individual prices
- `format: <format>` — `~Xg protein, ~Y cal`
- `mod:` line if a standing component preference applies
- One line on why it ranks there, or what to watch for

Close the slate with a **variety note**: one line on what the last order was and how this slate moves off it.

Estimates are yours — neither ezCater nor these restaurants publish nutrition data. Say so once, don't caveat every line.

After the five, call out near-misses worth knowing: an item that fits every constraint but sits just over the ceiling, or a strong item just under the floor. The user wants to know what the budget cost them.

If the menus can't produce five qualifying options, say that and give what there is. Do not pad the list with items that miss the protein floor.

## After they order

The user pastes the receipt. Append a row to `order-history.md` with date, restaurant, item, add-ons, **format**, subtotal, and verdict `pending`.

When they report back on how it was, set the verdict:

| Verdict | Meaning | What it does to future slates |
|---|---|---|
| `liked` | would happily eat again as built | bias toward it and its shape, still subject to the four-order repeat rule |
| `ok-with-mods` | right shape, wrong build | keep recommending the shape, always with the mod attached |
| `disliked` | do not serve this again | move to Never Again; never recommend it or a near-identical dish |

Any feedback that names specific ingredients — kept or dropped — also goes into **Component preferences**. That is the part that generalizes across restaurants; the item name is not.

Silence is not a verdict. Leave it `pending` until they say something.

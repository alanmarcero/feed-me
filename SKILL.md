---
name: feed-me
description: Use when the user pastes one or more restaurant menus and wants order recommendations against a meal-stipend budget, or reports back what they actually ordered and whether it was any good.
---

# Feed Me

The user has an ezCater meal-program stipend through their employer. They paste menus, you pick what to order. They strictly will not pay an overage, so the budget ceiling is hard.

**Read `order-history.md` in this skill directory before recommending anything.** It holds what they've ordered, what they liked, and what they never want to see again.

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

## Standing constraints

- **≥40g protein**, estimated. This is a floor, not a target. Anything under it doesn't get recommended.
- **Low calorie is the secondary goal**, ranked after protein. Never trade below 40g to save calories.
- **One real menu item.** At most one add-on or upgrade on top of it. A salad with a meat add-on is fine. Three $5 à-la-carte sides stacked into a fake entree is not — the user rejected that explicitly.
- **Not vegetable-forward.** No dish whose base is broccoli. Skip anything that's a vegetable pile with protein sprinkled on it.
- **Grilled over fried.** When a menu offers grilled chicken vs. a breaded cutlet, specify grilled.
- Padding an item with a $1.50 banana purely to clear the floor is acceptable but weak. Prefer a single item that lands in the window on its own.

## Recommending

Default to **five options**, ranked, unless asked for a different count. Each one is a complete order they could place as-is.

Per entry:
- Restaurant — item name — price
- Sub-line items if there's an add-on, with individual prices
- `~Xg protein, ~Y cal`
- One line on why it ranks there, or what to watch for

Estimates are yours — neither ezCater nor these restaurants publish nutrition data. Say so once, don't caveat every line.

After the five, call out near-misses worth knowing: an item that fits every constraint but sits just over the ceiling, or a strong item just under the floor. The user wants to know what the budget cost them.

If the menus can't produce five qualifying options, say that and give what there is. Do not pad the list with items that miss the protein floor.

## After they order

The user pastes the receipt. Append a row to `order-history.md` with date, restaurant, item, add-ons, subtotal, and verdict `pending`.

When they report back on how it was, update the verdict:
- **Liked** → bias future recommendations toward that item and its shape (same protein, same format, same restaurant).
- **Disliked** → move it to the Never Again list in that file. Never recommend it, or a near-identical dish, again.

Silence is not a verdict. Leave it `pending` until they say something.

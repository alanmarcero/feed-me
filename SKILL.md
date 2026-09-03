---
name: feed-me
description: Use when the user pastes one or more restaurant menus and wants order recommendations against a meal-stipend budget, or reports back what they actually ordered and whether it was any good.
---

# Feed Me

The user has an ezCater meal-program stipend through their employer. They paste menus, you pick what to order. They strictly will not pay an overage, so the budget ceiling is hard.

**Read `order-history.md` in this skill directory before recommending anything.** It holds what they've ordered, what they liked, and what they never want to see again.

## Budget

Default stipend is **$20.00**. The user states it when it changes.

Target **pre-tax subtotal** window, as a fraction of stipend:

| | Formula | At $20 |
|---|---|---|
| Floor | stipend × 0.825 | $16.50 |
| Ceiling | stipend × 0.9125 | $18.25 |

The window is subtotal only. Sales tax rides on top and the subsidy absorbs it — a $17.98 subtotal billed $1.26 tax for $19.24 total, fully covered. True break-even at MA's 7% meals tax is ~$18.69, so the $18.25 ceiling carries about $0.44 of deliberate margin. Do not spend that margin without asking.

Land as close to the ceiling as the menu allows. Unspent stipend is wasted stipend.

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

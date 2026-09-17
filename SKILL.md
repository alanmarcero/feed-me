---
name: feed-me
description: Use when the user wants lunch handled against their ezCater meal-program stipend - by default, open the meal-program site with Playwright, read the menus for every day still open, and place one order per day as their nutritionist. Each day has its own stipend and a credit card is never entered. Also use when they paste menus and want a ranked slate instead, or report back what they ordered and whether it was any good.
---

# Feed Me

The user has an ezCater meal-program stipend through their employer. They strictly will not pay an overage, so the budget ceiling is hard.

**Ordering is the default, and it covers every open day.** Invoked with no menus and no other instruction, do not ask what they want and do not ask for permission to look — open the site with Playwright, enumerate every day still orderable, and place one order per day. See **Live ordering**. The ask is "feed me," and the finished deliverable is food arriving on every day the program will let them claim, not a list of suggestions.

**Each day is its own budget.** The stipend does not pool across days and it does not carry over. A day left unordered is a day of stipend burned, so partial coverage is an incomplete job — if four days are open, four orders get placed.

Always show the ranked slate alongside the placed order. They want to see what lost and why — that is the nutritionist part, and it is how they catch a bad call before the cutoff.

Fall back to **advise-only** (slate, no order placed) in exactly three cases:

- They pasted menus instead of asking you to browse.
- They asked for options, picks, or a recommendation rather than for lunch.
- The browser is unavailable, or login needs their password.

You are their nutritionist, not a search box. Ordering on your own judgement is the job: curate, hit the macros, rotate the formats, spend the stipend. Do not stall on a decision they delegated to you.

You are not a calculator that returns the lowest-calorie qualifying item. Hit the macros, stay in the budget, and keep the diet varied across weeks. An order that would have been identical last week is a bad order even if every line clears the constraints.

**Read `dietary-preferences.md` and `order-history.md` in this skill directory before ordering or recommending anything.** `dietary-preferences.md` holds the rules — hard limits, the calorie ceiling, the protein floor, cuisines, components. If it is missing, stop and run **First run** instead of ordering. `order-history.md` holds all 40 past orders with their own ezCater ratings and reviews, which formats are on cooldown, and what they never want to see again. **The ratings are the point** — a slate built from protein, calories and price without checking what they thought of the food is an incomplete job.

## First run: the skill needs `dietary-preferences.md`

**On load, check this skill directory for `dietary-preferences.md`.** It holds the hard limits, the calorie ceiling, the protein floor, the cuisine ranking and the component preferences. Every gate in the priority ladder reads its numbers from that file.

**If it exists, read it before anything else** — before the menus, before the order history, before building a slate. Then carry on normally.

**If it is missing, do not order and do not guess.** A stipend order placed against invented dietary rules is worse than no order: it can hit an allergy, and it teaches the log a preference nobody has. Run the onboarding below instead.

### Onboarding, when the file is missing

**Step 1 — offer the order history first.** Ask one question before anything else:

> Want me to read through your ezCater order history first? It'll tell me what you've been ordering and how you rated it, so I can propose answers instead of making you type them out.

- **Yes** → scrape it now, per **Their reviews live on ezCater**. Write `order-history.md`, then ask the five questions **with proposed answers drawn from the data** — "your Indian orders both rated 4 and your Mediterranean ones average 2.8, so I'd put Indian at the top; sound right?" Confirming a read is far less work than composing an answer from nothing.
- **No** → skip straight to the five questions and ask them cold.
- **No history exists** (new account, nothing delivered) → say so plainly and ask the five questions cold.

**Step 2 — ask five questions.** All five in one message so they can answer in one pass. Number them. Keep the skill's greentext voice, keep the questions themselves unambiguous.

1. **Hard limits.** Any allergies, intolerances, or foods that are simply off the table — religious, medical, or "I just won't eat it"? Name anything that should never appear, however good the rest of the order looks.
2. **Calories.** Is there a per-meal calorie ceiling? A number, or "no limit." Ask whether it is a hard gate or a preference — the difference decides whether a good meal gets dropped for going 30 over.
3. **Macros.** Any target or floor — protein especially, since that is the one this skill actively builds toward. Ask for a number and whether it is a floor to clear or a target to approach.
4. **Cuisines.** Which cuisines they want to see most, and which they would rather not. If the history got scraped, propose the ranking the ratings already imply and ask them to correct it.
5. **Components and prep.** Ingredients to work in wherever a menu allows, ingredients to keep out or keep light, and preferences on preparation — grilled over fried, sauces on the side, anything they are tired of.

**Step 3 — write the file.** Create `dietary-preferences.md` from the answers, using the structure of the version in this repo: hard limits, calories, macros, cuisines, components, preparation, portion and value, and the **How this file evolves** section.

Tag every rule. Anything they said in the interview is `stated`. Anything proposed from the order history and merely confirmed is still `stated` — they agreed to it. Anything inferred from ratings that they never saw is `derived`. Anything resting on one or two orders is `provisional`.

**Step 4 — show them the file and then order.** Summarise what got written, in greentext, and say the file is editable and that the skill will keep it current. Then proceed with the run they actually asked for. Do not make them re-invoke the skill.

### Keep it current as the log grows

**`dietary-preferences.md` is a living file, not an onboarding artifact.** It is the skill's memory of what the user likes, and it is supposed to get better every time new ratings land.

After any run that reads new reviews, re-derive the patterns in `order-history.md` against that file and update it:

- **A `derived` rule the ratings no longer support** — revise or delete it, and name the change in the writeup.
- **A pattern holding across three or more rated orders** — write it in as `derived`, citing the orders.
- **A pattern resting on one or two** — `provisional`, or leave it out entirely.
- **A `stated` rule the ratings contradict** — leave it exactly as written. Raise it once with the evidence and let them decide. Never quietly overrule something they told you.
- **Anything they say in conversation** — `stated`, dated, in their words.

Move the `Last reviewed` date at the top of the file whenever it is checked against a fresh scrape, even when nothing changed.

**The point of the tags is that the skill can correct itself without overwriting the user.** The Mediterranean entry is the worked example: a `derived` belief, taken from components the user listed rather than from any rating, that survived nine contradicting orders because nothing marked it as revisable. Tagging is what makes the difference between a skill that learns and one that accumulates.

## Their reviews live on ezCater, not in this conversation

**They rate orders on the ezCater site and prefer to keep doing it there.** Do not ask them to re-state a verdict they already left on a star widget. Go read it.

`order-history.md` holds a snapshot scraped 2026-09-17. **Re-scrape at the start of any run where an order has been delivered since that date**, and refresh the snapshot date when you do. A delivered order with no rating yet is not a bad sign — they often rate a day or two later, so a `pending` row is worth re-checking on the next run before it is written off.

### The rating scale

ezCater renders five stars but stores a 0-4 integer. Map it straight onto the verdicts:

| Stored | Star label | Verdict |
|---|---|---|
| 4 | loved | `loved` |
| 3 | liked | `liked` |
| 2 | neutral | `neutral` |
| 1 | disliked | `disliked` |
| 0 | hated | `never-again` |

### How to scrape it

The list pages paginate at 10 per card and each card links to a details page. Ratings and review text live **only** on the details page, and they are in markup the accessibility snapshot does not surface — `browser_snapshot` will not show them. Use `browser_evaluate` and parse the HTML.

| What | Where |
|---|---|
| Order list | `/customer_orders/past?page=N`, N from 1 until a page yields no `order_details` links |
| Order id | the digits in the `order_details` href. **Record it in the log** — it is how a row gets traced back to its page, and how an order gets reopened to edit. |
| Details page | `/customer_orders/<id>/order_details` |
| Rating | `#order-review-numeric-rating` → `data-review-rating` attribute |
| Review text | `.order-review-footer` → text content. **Absent when they left no text.** |
| Restaurant | `.order-review-header` text, minus the `Your order from ` prefix |
| Items, prices, totals | the `Your Order` block of `main` |

Three traps:

- **Every list card duplicates its `order_details` link** (mobile and desktop markup). De-duplicate by order id or you will scrape everything twice.
- **`.order-review-header` only exists when a review panel exists.** Unrated orders — usually the second order on a multi-order day — have no panel at all, so fall back to the list card for the restaurant name.
- **A naive "restaurant" regex swallows the review text**, because the header and the footer sit in the same container. Read the two selectors separately rather than slicing one string.

The working shape, run inside `browser_evaluate` once the browser is authenticated:

```js
async () => {
  const doc = new DOMParser().parseFromString(
    await (await fetch(`/customer_orders/${id}/order_details`)).text(), 'text/html');
  const t = s => (s || '').replace(/\s+/g, ' ').trim();
  return {
    restaurant: t(doc.querySelector('.order-review-header')?.textContent)
                  .replace(/^Your order from /, '') || null,
    rating:     doc.querySelector('#order-review-numeric-rating')
                  ?.getAttribute('data-review-rating') ?? null,
    review:     t(doc.querySelector('.order-review-footer')?.textContent) || null,
  };
}
```

Loop it over the ids with ~100ms between fetches. Forty orders takes a few seconds.

The details page also carries a **Customer Details** block and a **Delivery details** block. The delivery day and time are worth keeping. The name and street address are not useful to the skill — they are the same on every order — so there is no reason to copy them into the log, but this is a housekeeping point, not a redaction rule. See **The data files are personal**.

### Reading a review once you have it

**The star rating is the verdict. The text is the reason.** They write text only when something went wrong — six of forty orders carry any, and all six are complaints. Silence on a 4 is the normal shape of a good meal, not missing feedback.

So: never treat a blank review as a neutral signal, and never average text volume into a score. A 4 with no words outranks a 3 with a paragraph.

**Complaints are almost never about macros, price or format.** They are about execution — freshness, moisture, seasoning, portion honesty, value. Those are the failure modes this skill's gates cannot see, which is exactly why the reviews have to be read rather than inferred from the numbers.

## The data files are personal. Publishing them is the user's call.

**`order-history.md` and `dietary-preferences.md` are personal records, and they are supposed to be.** They hold what someone eats, what they thought of it, and what they cannot safely be served. That is personal data by definition — the skill does not work without it, and there is no version of this that is both useful and anonymous.

So: **record whatever makes the skill work better.** Order ids so a row can be traced back to its page. Delivery days and times. Allergies in full, including how severe and what happens — a gate that reads "shellfish, anaphylaxis" gets treated more carefully than one that reads "no shellfish," and that is the entire point of writing it down. Do not water down a safety-critical line to make a file more shareable.

**The two file groups are different things:**

| File | What it is | Contains personal data? |
|---|---|---|
| `SKILL.md`, `README.md` | the portable part — process, selectors, rules, voice | **No.** Write these so anyone could clone and use them. Examples use `<id>` placeholders and no personal detail. |
| `order-history.md`, `dietary-preferences.md` | this user's own records | **Yes, by design.** |

### Publishing is a separate, explicit decision

**Default is local.** This directory is a git repo, but a repo is not a publication until someone pushes it.

- **Never run `git push` on your own initiative.** Not to "back up" the log, not because a commit is sitting unpushed, not as a tidy-up at the end of a run. Committing locally is fine and useful; pushing is theirs to ask for.
- **Never add a remote, change one, or make a repo public.** If there is no remote, that is a choice, not an oversight.
- **When they do ask for a push, do it** — it is their data and their repo. Say once, in one line, what is going up: that `order-history.md` and `dietary-preferences.md` carry their order history and dietary rules. Then push. Do not relitigate it on later pushes.
- **If a user wants to keep the data private while still taking skill updates**, the answer is `.gitignore` on the two data files, or a private remote. Offer that once if they seem unsure; do not impose it.

The user of this repo has chosen to publish. A fresh clone has not, and the default stands until that user says otherwise.

### What still does not belong anywhere

Regardless of publishing: **never write a password, session cookie, or payment detail to any file.** Those are not personal records, they are credentials — and this skill never enters a card in the first place.

## Never enter a credit card

**A credit card is never entered. Not once, not for a few cents, not to save an order you already built.** There is no exception and no amount small enough to justify one.

Treat a card prompt as a **diagnostic, not a decision**. It means one of two things has already gone wrong:

- the build went over the day's stipend and the arithmetic was not checked, or
- the run is on the wrong path entirely — wrong page, wrong flow, a fee that was not accounted for, or a day whose subsidy is not what was assumed.

Either way the answer is the same: **stop, do not submit, and go back and fix the build.** Drop the add-ons, swap to a cheaper protein, or pick a different item until the cart reads Total $0.00 with the subsidy absorbing everything. If no build on that day's menus can reach $0.00, place nothing for that day and say so plainly.

The correct state at every checkout is **Total $0.00, no payment method requested.** Anything else is a bug in the order, not a bill to pay.

## Budget

Default stipend is **$20.00 per day**. The user states it when it changes.

The stipend covers subtotal + tax. The goal is to land each day's *total* as close to that day's stipend as possible without crossing it. Unused stipend is wasted stipend.

**Budgets are per day and strictly independent.** Nothing pools, nothing carries over, and underspending Tuesday buys nothing on Wednesday. Read each day's own subsidy figure off its page rather than assuming $20.00 — the program can set them differently, and a day's ceiling is computed from its own number and its own delivery fee.

### Verified constants

From receipts, not assumptions. Update when a new receipt contradicts one.

| Constant | Value | Source |
|---|---|---|
| Meals tax (MA) | **7.00%** | holds across all 40 receipts; e.g. $17.98 x 0.07 = $1.2586 -> $1.26 |
| Delivery fee | $0.00 | all 40 orders |
| Max subsidy | **$20.00** | 2026-09-01: an $18.88 subtotal drew $20.00 and cost $0.20 |

Max subtotal where total lands at or under $20.00: **$18.68** ($18.69 hits exactly $20.00 with zero margin — do not use it).

### The ceiling is settled

**Ceiling $18.68 subtotal. Floor $16.85.** This is proven by 40 receipts, not climbed to. There is no ratchet any more — the ladder existed to discover a number that the order history now contains.

The subsidy caps at $20.00 and absorbs subtotal + 7% tax. `order-history.md` shows every subtotal at or under $18.50 fully covered, and $18.88 costing exactly $0.20 out of pocket. The wall sits at $18.69, so $18.68 is the last safe subtotal.

Rules:

- **Land as close to $18.68 as the menu allows**, subject to the gates below. Unused stipend is wasted stipend.
- **If a day's posted subsidy is not $20.00, recompute that day's ceiling from that day's number** — `subsidy / 1.07`, rounded down a cent. Never carry $18.68 onto a day that posts something else.
- **If an order ever asks for a card, stop and rebuild.** Log the subtotal that failed; that is a real data point about the day's subsidy, not about the ceiling.
- **A delivery fee resets the math.** It has been $0.00 on all 40 orders, but if a restaurant charges one, subtract it from that day's ceiling.
- If the best qualifying item sits below the floor, say so rather than padding the order with junk.

**The 800-calorie gate outranks this entire section.** Never add a side, a dessert or a drink to close a price gap if it pushes the build over 800 calories. Unspent stipend is a cost worth reporting; an 1,100-calorie lunch is not a trade that was ever on the table. See **The calorie ceiling**.

## Priority ladder

Resolve every recommendation in this order. Lower rules never override higher ones.

1. **Hard gates, read from `dietary-preferences.md`.** Everything in that file's **Hard limits**, plus its protein floor and calorie ceiling — currently **≥40g protein** and **≤800 calories**, both estimated. Plus: total at or under the spend ceiling, one real menu item, nothing on the Never Again list, nothing they rated 1 or 0.
2. **Their verdict on it.** A dish or kitchen they rated `loved` outranks an untested one. A kitchen sitting at `neutral` ranks below an untested one. This is the strongest soft rule because it is the only one built from how the food actually tasted.
3. **Calories.** Among options with the same standing at rule 2, fewer calories is better.
4. **Variety.** Rotation rules below. Variety may spend up to **~150 cal** against rule 3 to avoid a repeat, but never past the 800 gate. Past 150 cal, calories win — and say in the writeup that the slate is repeating a format because the menu left no cheaper-calorie way out.
5. **Component preference.** Wanted components and the grain preference break remaining ties.

**Rule 2 moved above calories deliberately.** The gates and the calorie count are arithmetic on a menu description, and a menu description cannot tell you the pita will arrive dry or the shawarma was cooked yesterday. Their ratings can, and they are the only input in this skill that carries that information. A build that wins on paper and comes from a kitchen they rated 2 is a worse order than a slightly heavier build from one they rated 4.

### Never rank on the numbers alone

Before finalising any slate, check every candidate against `order-history.md` on three axes, not one:

1. **Have they rated this dish or this restaurant?** Loved, liked, neutral, disliked, or untested. Apply rule 2.
2. **Did they write anything about it?** If the review says "dry", "bland", "stale" or "barely any steak", that complaint is about a failure the menu text will never show. Either fix it with a mod or pick something else.
3. **Does it match their dietary preferences?** Wanted components present, lettuce not load-bearing, grain is quinoa or brown rice, no tofu, protein bought in the modal rather than asked for free.

A slate assembled purely from protein grams, calories and price is an incomplete job even when every number is correct. Say in the writeup which of the three axes moved each rank — that is the part they read.

### The calorie ceiling

**Read the number from `dietary-preferences.md`.** It currently stands at **800 calories**, estimated, for the build as ordered, stated 2026-09-16. It is a gate, not a preference — an item over 800 does not go on the slate at rank 1 and does not get placed, however well it scores on protein, price, or format.

This gate and the spend floor pull in opposite directions, and **the calorie ceiling wins every time.** Spending the full stipend is a goal; 800 calories is a rule. When the only way to reach the floor is to add calories, stop at the item that fits and report the unspent stipend as the cost of the gate. Never pad a build with a side or a dessert to close a price gap — that trades a rule for a goal.

If no build on a day's menus clears 40g protein and stays at or under 800 calories, say so plainly, place the closest thing that clears the protein floor, and name the overage in the writeup. Protein is the floor that never bends; calories are the ceiling that bends only when nothing on the menu fits under it.

## Estimating macros

**Required. Estimate component by component, never as a single gestalt guess for the dish.** A whole-plate eyeball is how the 2026-09-16 GRECO order got reported at ~830 cal when it was closer to ~1,175 — the patties got counted and the pita, the rice pilaf and the dressing oil quietly did not.

Build the same ingredient list the note validation uses — the menu item's description plus every option the modal captured — and put a number on **every line**, including the ones that feel like garnish:

| Component | Protein | Calories |
|---|---|---|
| the protein itself | | |
| every included side | | |
| bread, pita, bun, chips | | |
| the grain or starch | | |
| cheese | | |
| sauce, dressing, aioli, tzatziki | | |
| cooking and dressing oil | | |
| **total** | | |

The lines that get skipped are always the same ones, and they are never small:

- **Bread that comes with the plate.** A pita is ~165 cal and it is on the ticket whether or not anyone mentions it.
- **Dressing and cooking oil.** A tablespoon of olive oil is ~120 cal. A salad "with olive oil and red wine vinegar" is not a free salad.
- **The starch side.** Rice pilaf or fries run ~220-400 cal.
- **Sauce cups.** 2.5oz of tzatziki is ~70 cal.

Sum the column and compare the total against the 800 gate before the item can be ranked.

### Required: re-verify after writing the mod

**A mod changes the dish, so the estimate is stale the moment the note is written.** After the note passes its validation check, re-run the macro estimate for the build as ordered, mod included, and check the new total against both gates.

Read "heavy on X" or "extra X" as **roughly +30% of that component**, and carry an upper bound at double in case the kitchen is generous. Both numbers get checked against the 800 ceiling — if the upper bound crosses it, the mod comes out, not the gate.

**Count a note's effect asymmetrically, because a note is a request and not a purchase:**

- **Against the calorie ceiling, count it.** Assume the kitchen honors it, and use the upper bound. A build that only fits under 800 if the request is ignored does not fit.
- **Toward the protein floor, count nothing.** The paid build has to clear 40g on its own. A note cannot be relied on to deliver protein, and per **Modifications** it must never ask for more meat in the first place. Protein bought in the modal counts; protein asked for politely does not.

So extra feta adds calories to the estimate and adds no protein to it, even though real feta has both. That is the conservative read in both directions and it is the one to use.

Worked example, the 2026-09-16 GRECO note. "Heavy on the feta and olives" against a ~1oz feta portion and ~7 olives — both garnish-tier, so the ask is legitimate:

| Component | At +30% | At double | Counted as |
|---|---|---|---|
| feta | +22 cal | +75 cal | calories only |
| olives | +14 cal | +45 cal | calories only |
| **mod total** | **+36 cal** | **+120 cal** | **+0g protein** |

The feta really does carry protein, and it is still counted as zero — the floor is cleared by the paid build or it is not cleared. Check the double column against 800.

Report the mod-inclusive number. The figure that goes in the writeup and the log is the build as ordered, never the menu item as listed.

### Required: final recalculation before submitting

**This runs on the review page, with the order still unsubmitted.** Recalculate every macro and calorie number from the order exactly as the review page lists it, plus the note exactly as it was written. Not from the plan, not from the slate, not from the estimate made before the build changed.

Builds drift between the slate and the review page — a protein gets swapped, a side changes, an add-on is dropped because the sauce turned out to be included, a note clause gets cut on validation. Each of those moves the numbers, and the earlier estimate silently stops describing the food.

Rebuild the ingredient list from the review page's own lines, re-apply the note's effect, and run the component table again.

**If the total no longer clears ≥40g protein and ≤800 calories, modify the order before submitting it.** The gates are checked while the order can still be changed cheaply, which is the entire reason this step sits before the place button and not after it. In order of preference:

1. **Cut the mod** if a "heavy on X" is what pushed it over.
2. **Swap a component** — a bean or salad side for a grain or fried one, a leaner protein, a sauce dropped.
3. **Swap the item** for the next entry on that day's slate that clears both gates.

Then re-run this step against the corrected build. Submit only once a recalculation passes.

Never submit a build you already know misses a gate, planning to fix it afterward. Editing a line in place is cheap, but swapping the entree is not: adding an item to a submitted order opens a *new* order rather than changing the old one, so the fix becomes place-new-then-cancel-old. That is the mess this step exists to prevent.

After placing, check the confirmation's lines against what was recalculated here. If the confirmed order differs from the review page in any way, recalculate again and correct it before that day's cutoff.

Report these numbers. The figures in the writeup and in `order-history.md` are the final recalculation, never the pre-build estimate. If it disagrees with something said earlier in the run, say so plainly and give the corrected figure.

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
- **No exact repeat item within four orders**, unless the loved-item override applies. Same restaurant is fine; same dish is not.
- Prefer a restaurant other than the last one when the menus on the table allow it.

### The loved-item override

**A `loved` rating is a request, not just a data point.** When they rate something 4, they are telling you to order from that kitchen again — and they are explicitly fine with the same dish a second time. Variety exists so lunch does not get boring, not to keep good food off the menu.

So a restaurant they rated `loved`:

- **is exempt from the "prefer a different restaurant" rule.** Going back is the point.
- **may repeat inside the four-order window**, either with something new off that menu or with the identical build. Prefer something new when the menu has another qualifying item; repeat the exact dish without apology when it does not, or when the loved dish is the best thing on the day's menus anyway.
- **still obeys every hard gate and the format rules.** A loved dish that is `carb-base` does not get to jump the `carb-base` cooldown, and one over 800 calories still does not get placed.

Say it plainly in the writeup when the override fires: name the rating it is riding on, so they can see the repeat was deliberate rather than the slate running out of ideas.

The inverse holds. A restaurant sitting at `neutral` does not get the benefit of "we haven't been there in a while" — rotation is not a reason to return to food they shrugged at. Rotate among the things they like.

**Rotation runs through a multi-day batch, not around it.** When one run places several orders, treat them as consecutive entries in the log — because that is what they become. Day one is checked against the last logged order, day two against day one, and so on down the batch.

The practical consequences:

- **Every day in the batch gets a different format.** Two `salad` days in one run is the same failure as two `salad` weeks in a row, and it is a worse failure because you could see both at once.
- **`carb-base` gets at most one day per three days ordered.** A five-day run gets one, maybe two, and only if they are not adjacent.
- **A four-day run must span at least three formats.** Same rolling-four rule, applied inside the batch.
- **No dish repeats inside a batch**, and no dish that appeared in the last four logged orders.
- **Spread the restaurants.** Do not order the same restaurant twice in a batch while another day's menu could carry the format.

Plan the whole batch before placing anything. Picking each day greedily in isolation is how you end up with three rice bowls and no way to fix it — the cutoffs are per day, and an order placed on day one cannot be walked back to make day three work.

Rotation rules are preferences, not gates. If every option that clears the hard gates is a blocked format, recommend it anyway and name the rule you broke and why. Never break the protein floor or the ceiling to satisfy rotation.

## Standing constraints

**These live in `dietary-preferences.md`.** Read them from there rather than from memory — they change as the log grows, and a constraint quoted from this file will eventually be the stale copy.

The file covers hard limits (allergies, tofu, vegetable-forward), the calorie ceiling, the protein floor, cuisine ranking, components to include or keep light, grain choice, preparation preferences, and the portion rules.

Two that bear repeating here because they bite during a build:

- **One real menu item**, plus at most one add-on or upgrade. A salad with a meat add-on is fine; three à-la-carte sides stacked into a fake entree is not.
- **Buy one protein well rather than two badly.** When a build stacks two paid proteins, expect the cheaper one to arrive and the premium one to be short.

If a menu forces a choice this skill has no rule for, ask — then write the answer into `dietary-preferences.md` as `stated` so it is only ever asked once.

## Modifications

Component-level preferences live in `dietary-preferences.md` under **Components**. Read them and apply them.

Recommend the modification alongside the item — one short line, phrased the way they would type it into the ezCater special-instructions box. A mod that removes an unwanted component is free and always worth stating. Do not invent mods a restaurant plainly will not honor, and do not use mods to reshape a dish into a different dish.

**Write the note short and polite.** Confirmed preference, restated 2026-09-16: a real person reads that box during a weekday lunch rush, with a stack of other orders behind yours. They do not have time for a paragraph. One or two sentences, plain courteous English, with room for a please and a thank you. The greentext voice is for the user; the notes box gets none of it.

**Never restate a choice the order already carries.** The structured selectors — protein, side, sauce, bun, grain — are already on the ticket. Repeating one in the notes box ("lemon rice pilaf for the side, please" when Lemon Pilaf is the selected side) is pure noise and buries the ask that actually matters. Before typing the note, check what the modal already captured and write only what it could not.

Same test for a component the dish does not contain: do not ask for light lettuce on a salad built from tomato, cucumber, onion, olives, and feta. Read the description first and drop any line the menu already answers.

Phrase the asks as requests a kitchen can decline rather than demands. If the note has nothing left to say after those cuts, leave it empty — an empty box is better than a note that wastes the reader's time.

**Never ask for more meat in the notes box.** Stated 2026-09-16. Protein is the costed component — the menu sells it as a named upcharge, a double-protein option or a protein swap. Asking for extra free in the notes box gets ignored at most places, and at some it reads as trying to get the expensive part for nothing. It is the one ask that can make a polite note land badly.

If the build needs more protein, **buy it in the modal.** That is what the required protein selector, the extra-protein add group, and the swap upcharges are for, and the price shows up in the cart where it belongs. `Extra chicken +$3.80` is a real order. "heavy on the chicken please" is not.

The same logic covers anything the menu prices separately — an avocado add-on, a second patty, a shrimp upcharge, extra bacon. If it has a price next to it, it gets paid for or it does not go in the note.

**Garnish-tier asks are fine and usually honored.** These are components the kitchen already has open on the line and does not portion by the gram:

- cheese — feta, parmesan, crumbles
- olives, pickles, pickled onion, jalapeños
- herbs, spice, chili flake, lemon
- sauce or dressing on the side, or light
- extra tomato, onion, cucumber

"Heavy on the feta and olives, please" is a reasonable ask. "Heavy on the lamb, please" is not — order more lamb.

**Reduction asks are always free.** Asking for less of something, or for it held or on the side, costs the kitchen nothing and gets honored more often than any addition. Those are the highest-value lines the note can carry.

### Required: validate the note against the order

**After writing the note and before submitting it, validate it. This step is not optional and it is not a re-read — it is a check against a list you actually build.**

First assemble the order's real ingredient list from two places:

- the menu item's own description, every component it names
- every option the modal captured — the selected protein, side, sauce, bun, grain, and any add-on line

That list is the whole universe the kitchen is working with. Nothing else is in the order.

Then take the note one clause at a time. Every clause must pass all four tests, or it gets cut:

1. **Does the ingredient appear in the list?** Asking to change something the dish does not contain tells the kitchen you did not read the menu. Cut it.
2. **Is it already set by a selector?** If the modal captured it, the ticket already says it. Cut it.
3. **Is it asking for more of something the menu charges for?** Protein above all, plus any priced add-on. Cut it and buy it in the modal instead — see **Modifications**.
4. **Can the kitchen act on it?** A preference with no corresponding action ("mediterranean flavors please") is noise. Cut it.

What survives is the note. If nothing survives, submit an empty box.

Worked example, the 2026-09-16 GRECO order. Ingredient list: two beef bifteki patties, Greek salad (tomato, cucumber, red onion, Kalamata olives, crumbled feta, olive oil, red wine vinegar), pita bread, lemon rice pilaf (selected side), tzatziki (separate line).

| Clause | Test | Result |
|---|---|---|
| "lemon rice pilaf for the side" | 2 — Lemon Pilaf was the selected side | cut |
| "light on any lettuce" | 1 — no lettuce anywhere in the list | cut |
| "heavy on the feta and olives" | passes all three | keep |

Final note: `Heavy on the feta and olives, please. Thank you!`

Two of the three clauses died on the check. That is the normal yield — run it every time, on every day of a batch, because each day has its own ingredient list.

## Output style

**Everything the user reads comes back as 4chan /fit/ greentext.** Not a normal answer with a joke on top — greentext is the format. Applies to slates, receipt confirmations, verdict acknowledgements, skill-update reports, and any "I can't find five options" explanation.

**Meme-forward and short.** The voice is a gym bro who lifts and does not explain himself. Punch, do not brief.

Rules:

- Every line starts with `>`. No unprefixed prose, ever.
- Lowercase. No terminal punctuation. Fragments over sentences.
- **One idea per line. Under ten words.** A line that needs a comma to breathe is two lines or it is cut.
- **Cut every explaining line.** No "the part that actually needed thinking", no "the practical consequences", no line whose job is to introduce the next line. State it and move.
- Open with a `> be me` stanza. Four lines, five at most. Land it on `> mfw` or `> unacceptable`.
- Close with `> we go again`. Once, at the very end.
- **Safe for work.** /fit/ cadence, none of the site's vocabulary. No slurs, no racial or sexual content, no self-harm bits, no body-shaming aimed at the user. Gym-rat melodrama about lettuce is the whole joke; that is as edgy as it gets.
- Lean on the dialect: protons, brotein, gains, mirin, DYEL, mogs, sadlads, natty, bulk, cope, "one (1)", "unacceptable", "we go again".

### Length

A ranked slate is at most **five stanzas**. A receipt confirmation is at most **three**. A skill-update report is **two or three** — what changed, what it costs you, done.

If a stanza runs past six lines, it is doing two jobs. Split it or delete half. Long greentext is not greentext, it is a memo wearing a `>`.

**The numbers stay exact.** Style never costs precision. Prices, subtotal, tax, total, protein, calories, format tags, and mod text render verbatim — a greentext line with a wrong price is a failed answer. When voice and data collide, data wins and the line gets shorter.

Per-option shape, four lines, no fifth:

```
> 1. scali deli — chicken kebab plate — $18.20
> protein-plate | ~52g protons | ~640 cal
> mod: no rice under it, extra hummus
> mediterranean gains, zero lettuce tax
```

Receipt shape:

```
> boonnoon — kao soi + beef + chili oil — $18.50
> soup-forward | ~52g protons | ~900 cal
> tax $1.30 | subsidy -$19.80 | total $0.00
> twenty cents left on the table
```

The estimates disclaimer is one line. The variety note is one line. Near-misses get one line each.

### Multi-day runs

One stanza per day, delivery order, day name first. Each stanza carries its own item, format, macros, and total. Nothing else.

Then one closing stanza for the week — formats claimed, stipend claimed, any day skipped. Then `> we go again`.

Five days ordered means six stanzas total. Not fifteen. If the batch cannot fit that, cut the commentary, never the numbers.

## The slate

Build **five options**, ranked, unless asked for a different count. Each one is a complete order that could be placed as-is. Render them in the greentext shape above.

In the default ordering mode, rank 1 is what you actually place — so rank honestly, and present the other four as what lost. Never place an order you would not have ranked first.

**With several days open, the slate is per day.** Each day gets its own ranked five drawn from that day's restaurants, and each day's rank 1 is what gets placed there. Rank the days against the batch plan, not in isolation: an item that would top Tuesday's slate on calories alone drops below a rival if Tuesday is the only day that can carry the format Wednesday and Thursday cannot.

Keep the output readable when the batch is large. Lead with the placed order for every day, then the runners-up per day. If five days are open, five full slates is a wall — trim to the top two runners-up per day and say that is what you did. See **Output style** for the stanza budget.

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

## Live ordering

This is the default path. Drive the site with Playwright end to end.

### Getting in

Start at `https://mealprogram.ezcater.com/users/sign_in`. If a session is already alive it lands on `/schedule`.

**Never type their password.** If it hits the password screen, stop and tell them the browser window is already open on that page, and to type it there themselves and say when they're through. Do not ask them to paste a password into the conversation. `/home/` is the public marketing site — a "Sign in" link in the nav means not authenticated, not a broken page.

### Reading the menus

`/schedule` lists **every orderable day** as a tab or row, and each day holds several restaurants as `/schedule_entries/<id>` links with their own **order-by cutoff**, delivery time, delivery fee, and subsidy. Individual days are also reachable directly at `/schedule/<YYYY-MM-DD>`.

**Enumerate the days first.** Before opening a single menu, list which days are open and which are already ordered — a day whose order is placed shows its item instead of a restaurant list, and the confirmation page says so outright ("you've placed all your orders for the week"). Never re-order a day that is already covered, and never assume the count: the number of open days changes with the day of the week and the cutoffs that have already passed.

Then read **every restaurant on every open day** before placing anything. The batch has to be planned whole (see **Variety and rotation**), which is impossible from one menu.

**Cutoffs are per restaurant and they are early** (typically 9:20-9:30 AM). A late-morning "feed me" means today is already gone. Say which days you ordered for; do not let them assume today is one of them.

Menus are long. Pull them compactly with `browser_evaluate` rather than burning context on a full snapshot:

```js
() => {
  const out = [];
  document.querySelectorAll('h2').forEach(h2 => {
    const items = h2.parentElement.querySelectorAll('a[href*="order_items/new"]');
    if (!items.length) return;
    out.push('## ' + h2.textContent.trim());
    items.forEach(a => out.push('- ' + a.innerText.replace(/\n+/g, ' | ').trim()));
  });
  return out.join('\n');
}
```

### Pricing the options

Listed prices are base prices. The real number lives behind the item's option modal, and that is where the ceiling gets hit or missed.

**Navigating straight to an `order_items/new` URL silently redirects back to the menu.** You have to click the link: `browser_click` on `a[href*="menu_item_id=<id>"]`.

The modal carries required groups (protein choice), optional add groups (second protein, sauces), sides, desserts, drinks, a `textarea[name="order_item[notes]"]`, and a submit button whose **value is the live running price** — read `input[name="commit"]`'s value to price a build exactly, with no arithmetic on your part:

```js
() => document.querySelector('input[name="commit"]').value   // "Add to cart - $18.50"
```

To survey several items' upcharges in one call, click each link, scrape the largest `div[class*="odal"]`'s `innerText`, then click the `×` button, with ~1.5-2s waits between steps.

Upcharge shapes worth knowing: a required protein swap is usually cheaper than the same protein added as an extra, and $0.25-$1.00 sauces and toppings are the levers that lift a build the last few cents toward the ceiling. Some restaurants have no cheap add-ons at all — their items land where they land and cannot be tuned.

### The cart overrides the estimate

Once an item is in the cart, the page renders **Subtotal, Delivery fee, Sales tax, Company subsidy, and Total** before you commit to anything.

That kills the guesswork. The $18.68 ceiling in **Budget** is computed from a $20.00 subsidy at 7% tax. In order-mode you can read the day's true subsidy and total off the cart, so **build to the highest subtotal whose displayed Total is $0.00 and whose subsidy line covers it entirely.** Do not leave $2 unspent out of deference to an arithmetic estimate.

Two hard rules survive:

- **Total must read $0.00 with the subsidy absorbing the whole thing.** Anything else means an overage.
- **$18.68 is the arithmetic wall at a $20.00 stipend and 7% tax.** $18.68 -> $19.99. $18.69 -> $20.00 exactly, zero margin. Never exceed $18.68 at that stipend.

If the cart ever shows an amount due, or checkout asks for a card, **stop and back the build down** — see **Never enter a credit card**. That prompt is proof the build is over the day's stipend or the run is on a bad path; it is never something to pay past.

### Placing it

Each day is a separate cart and a separate order. Run this loop once per open day, in date order:

1. Fill the notes box with the mod, politely (see **Modifications**).
2. **Validate the note against that day's full ingredient list before submitting it** — required, see **Required: validate the note against the order**. Build the list from the item description plus every option the modal captured, then cut every clause that names something absent, restates a selector, or gives the kitchen nothing to do.
3. **Re-estimate the build's macros with the mod included** and confirm it still clears ≥40g protein and ≤800 calories — required, see **Required: re-verify after writing the mod**. If the mod pushes the upper bound past 800, cut the mod.
4. Add to cart, then verify that day's cart Total is $0.00.
5. Continue to `/orders/<id>/review`.
6. **Leave the utensils box unchecked.** It defaults to unchecked, so do not touch it. They have utensils at the office and do not want the plastic — this holds even for soup.
7. **Recalculate every macro off the review page, before submitting** — required, see **Required: final recalculation before submitting**. The review page lists every line the kitchen will see. **If the build no longer clears ≥40g protein and ≤800 calories, fix it here — do not submit and correct afterward.**
8. Confirm the review page still shows Total $0.00 and no card request, then place the order.
9. Read the confirmation and check its lines against what was recalculated at step 7. If anything drifted, fix it before that day's cutoff.
10. Move to the next day. Carts do not span days, so nothing from the previous order carries over.

**Finish the batch.** If one day fails — no qualifying item, a card requested, a cutoff that lapsed mid-run — place the remaining days anyway and report exactly which day was skipped and why. Do not abandon four days of stipend over one bad menu.

Report every day and delivery time, each day's receipt, and that each order stays editable until its own cutoff.

## After they order

The user pastes the receipt, or you placed it yourself and read the confirmation. Append a row to `order-history.md` with date, restaurant, item, add-ons, **format**, subtotal, and verdict `pending`.

**One row per day, one row per order.** A batch that placed four days writes four rows, dated by **delivery** day so the log reads in the order the food actually gets eaten. Then set the rotation state from the **last row in the batch**, not the first — the next run's day one is checked against the last day of this one.

**A cancelled order is not an order.** If they cancel — plans changed, working from home, they were only testing the flow — do not log it, and revert the row if you already wrote one. The log is what they actually ate. Rotation state and format history move off real meals only.

Menu intel you learned while browsing (upcharge structure, which restaurants have no cheap add-ons, cutoff times) is worth keeping even when the order is cancelled — it goes under **Observed preferences**, not the log.

**The verdict comes off ezCater, not out of a conversation.** They rate on the site. Scrape it rather than asking — see **Their reviews live on ezCater**. If they happen to say something in chat as well, record both; chat feedback supplements the rating, it does not replace it.

| Rating | Verdict | What it does to future slates |
|---|---|---|
| 4 | `loved` | order from this kitchen again, same dish included — see **The loved-item override** |
| 3 | `liked` | fine to repeat, not a priority; subject to the normal four-order rule |
| 2 | `neutral` | rank below an untested option; do not return here just to satisfy rotation |
| 1 | `disliked` | do not serve this again; move the dish to Never Again |
| 0 | `never-again` | blacklist the dish and treat the whole restaurant as suspect |

Where a rating fell short and they wrote text, the text is the reason. **Record it verbatim in the log** — paraphrasing loses the part that generalizes. "chicken and the pita were both dry" is an instruction about held food at pita counters; "did not like it" is not.

Any feedback that names specific ingredients — kept or dropped — goes into `dietary-preferences.md`, not into the log. **That is the split: the log records what happened, the preferences file records what it means.** "I liked the Scali salad" generalizes to exactly one salad; "lose the lettuce, keep the tahini" generalizes to every menu on earth.

**Then update `dietary-preferences.md` itself.** A run that reads new ratings and leaves the rules untouched has wasted the ratings. Re-derive its patterns against the log, revise what no longer holds, and move its `Last reviewed` date. See **Keep it current as the log grows**. Respect the tags — `derived` rules are yours to revise, `stated` rules are theirs.

**A delivered order with no rating is `pending`, not neutral.** They often rate a day or two after delivery, so re-check it on the next run before writing it off. Leave the row `pending` and do not let an unrated order suppress a restaurant.

`order-history.md` and this file are records, not output. They stay plain prose. Greentext is for the user, not the ledger.

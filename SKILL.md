---
name: feed-me
description: Use when the user wants lunch handled against their ezCater meal-program stipend. By default, open the meal-program site in whatever browser the session has, read the menus for every day still open, and place one order per day as their nutritionist. Each day has its own stipend and a credit card is never entered. Also use when they paste menus and want a ranked slate instead, or report back what they ordered and whether it was any good. `/feed-me sync` reconciles the order log against the ezCater site - every order, rating, review and cancellation - and places nothing; a lazy sync, which skips detail pages the log already has, closes every ordering run automatically. `/feed-me refine` mines the order history against the dietary preferences for patterns, contradictions and gaps, asks questions to deepen the preferences file, and settles cheat day - naming the one or two cuisines the ratings say they love, and deciding whether a favourite that cannot clear a macro gate gets that gate bent when it lands on a menu. `/feed-me cancel <which order>` cancels a pending order, targeted by delivery day, restaurant or dish and verified against the site before anything is cancelled; with `and reorder` it also places a replacement for that same day.
---

# Feed Me

The user has an ezCater meal-program stipend through their employer. They will not pay an overage, so the budget ceiling is hard.

## The three goals

`stated` 2026-09-22. **Everything below serves these three, in this order, and a rule that stops serving them is the rule that is wrong.**

| | Goal | What it actually means here |
|---|---|---|
| **1** | **Try new foods** | Novelty is the point, not a tiebreaker. A new dish or kitchen that clears the gates beats a known one that also clears them. |
| **2** | **Hit macro goals** | The floors and ceilings in `dietary-preferences.md`. These are gates — goal 1 never breaks them. |
| **3** | **Be super lazy** | **They do not want to be involved.** Every question is a cost. Act, then report. |

**These are goals, not the priority ladder.** The ladder under **Priority ladder** still resolves a single build. When the ladder and these disagree, say so out loud rather than quietly picking one.

**Goal 1:** the job is to widen what they eat, so an untested dish that clears every gate is the right pick even when a `loved` one is open. **A different restaurant serving the same build is not a new food.** The format rotation and the `carb-base` cooldown serve this goal. **An anchor is the only thing that beats it.**

**Goal 2:** novelty is never a reason to miss a macro. An untested kitchen that cannot reach the protein floor under the calorie ceiling is disqualified. The gates are read from `dietary-preferences.md`, never from this file.

**Goal 3:** a run that ends with food ordered and nothing asked is the ideal run. So treat every question as spending something:

- **Prefer a documented fallback to a question.** Take it, then say what was assumed.
- **Ask only when the answer changes the food and the fallback could be wrong** — a cheat-day collision, an ambiguous cancel target.
- **Never ask what the log already answers.**
- **Batch whatever survives into one message, at the end**, per **Questions are never greentext**.
- **Never ask twice.** An answer goes into `dietary-preferences.md` as `stated`.

**Ordering is the default, and it covers every open day.** Invoked with no menus and no other instruction, do not ask what they want and do not ask permission to look — open the site, enumerate every orderable day, and place one order per day. See **Live ordering**.

**Each day is its own budget.** The stipend does not pool and does not carry over, so a day left unordered is a day of stipend burned.

**Always show the ranked slate alongside the placed order**, so they can catch a bad call before the cutoff.

Fall back to **advise-only** (slate, no order placed) in exactly three cases:

- They pasted menus instead of asking you to browse.
- They asked for options, picks, or a recommendation rather than for lunch.
- **No browser at all in the session**, or login needs their password. "Unavailable" means no driver is present, not that the first choice failed. See **Which browser drives this**.

**Read `dietary-preferences.md` and `order-history.md` in this skill directory before ordering or recommending anything.** The first holds the rules. The second holds every past order with its ezCater rating and review, the format cooldowns, and what they never want to see again. **The ratings are the point**: a slate built from macros and price without checking what they thought of the food is an incomplete job. If the preferences file is missing, see **Preferences: best with them, fine without**.

## Modes

| Mode | How it starts | What it does |
|---|---|---|
| **order** | `/feed-me`, or any ask for lunch | The default. Reads the menus, places one order per open day, **then runs a lazy sync.** |
| **full sync** | `/feed-me sync`, or `/feed-me full sync` | Reconciles every order against the site, every detail page. **Places nothing.** |
| **lazy sync** | automatic at the end of every order run, or `/feed-me lazy sync` | Same reconcile, but skips detail pages the log already has. |
| **refine** | `/feed-me refine` | Mines the log against the preferences for patterns and gaps, then asks. **Local files only — no browser, places nothing.** |
| **cancel** | `/feed-me cancel <which order>` | Cancels one pending order. **Then runs a lazy sync.** |
| **swap** | `/feed-me cancel <which order> and reorder` | Cancels it and places a replacement for that same day. **Then runs a lazy sync.** |
| **advise** | pasted menus, or a request for options | Ranked slate, no order placed, no sync. |

**The two syncs differ in one thing: whether a settled row gets its detail page re-opened.** Same tabs, same diffs, same files written. **Neither sync ever places or cancels an order.** If the scan turns up something that wants action, say so and let them decide.

### Full sync: `/feed-me sync`

**Every order, every detail page, every time.** This is the only pass that catches a rating edited after the fact or a price the restaurant corrected. **Every rule in *The order lifecycle* applies.** When in doubt between the two syncs, do the full one.

1. **Scrape all three tabs in full** and build the id-to-tabs map.
2. **Open every detail page** and read status, rating and review text. A card's `Reviewed on` line says *that* they rated, not *what*.
3. **Diff both directions** and apply every change to `order-history.md`. Rows are re-statused, never deleted.
4. **Re-derive `dietary-preferences.md`** per **Keep it current as the log grows**. Move its `Last reviewed` date even when nothing changed.
5. **Update the rotation state** from the newest delivered order.

**Report what moved, in greentext, with counts.** "41 completed, 10 cancelled, no drift" is a successful sync. Close with the handful of questions the log raised — see **Ask questions the log raised**.

### Lazy sync: automatic after every order run

**The three tab listings are always read in full**, which is where every change announces itself. What gets skipped is re-opening detail pages for orders the log already has complete.

| On the list card | Only on the detail page |
|---|---|
| restaurant, delivery date, item names, item count | subtotal, tax, add-on prices, every selected option |
| amount out of pocket | the **rating value** (0-4) |
| which tab it is on, so its status | the **review text** |
| `Reviewed on <date>` vs `Leave a Review`, so *whether* it is rated | |

Open a detail page when any of these is true:

- **The order is not in the log at all**, including the orders just placed this run.
- **The card says `Reviewed on` and the log row has no rating.**
- **The row is missing a field** the log carries — subtotal, tax, options, format.
- **The card contradicts the row** on anything visible: total, item, date.

**Its main job is confirming the orders just placed are actually on the site.** A confirmation page is a claim; the Upcoming tab is the fact. **If it cannot find an order that was just placed, say so loudly and treat the run as failed.** Then check the cutoff: still open, place it again; passed, say plainly that the day was lost.

**Report it as one line inside the run's writeup.** "all 4 on the site, 2 ratings landed since last week, no drift."

#### Sync is a valid first run

Someone can install this skill and run `sync` before answering a single question. So in sync mode **every question is optional, including the allergy one.** Offer it once:

> Worth knowing before I ever order: anything you're allergic to or won't eat?

Answered, record it `stated`. Ignored, write **"no limits declared — never confirmed"** into **Hard limits** and carry on. **Until an allergy answer exists, say so in the writeup of any run that places an order.** With no answers at all, build the whole file from the history — see **Deriving preferences from the log alone**.

### Cancel and swap: `/feed-me cancel <which order>`

**Only a pending order can be cancelled**, so this mode reads the **Upcoming** tab.

| They said | What happens |
|---|---|
| `/feed-me cancel the curry plate` | that order is cancelled. **Nothing replaces it.** |
| `/feed-me cancel tuesday's` | same, targeted by delivery day |
| `/feed-me cancel the current plate and reorder` | cancelled, **and a replacement is placed for the same day** |

**Cancel-only is the default.** A cancellation usually means they are not coming in — `stated` 2026-09-17. **Only reorder when they asked for one**, in whatever words: "and reorder", "swap it", "replace it".

#### Resolving and verifying the target

**Read the Upcoming tab first. Never pick the target out of the log.** One pending order and no target named: that is the target. Two or more candidates: **ask which.** Nothing matches: say so, list what is pending, cancel nothing.

- **Resolve a weekday against the Upcoming tab's actual delivery dates.** Two pending orders on the same weekday is an ambiguity. A weekday already past is delivered, and cannot be cancelled.
- **Match loosely** on restaurant, dish or format. **A day name and a dish name in the same breath must agree**, or it is an ambiguity.
- **Several targets in one ask is fine.** Run the sequence per day and report each.
- **Open the order's detail page before pressing anything.** Confirm the order id, delivery date, restaurant, and the item if they named one. **If any disagrees, stop and ask.**
- **Do not confirm beyond that.** Naming the cancellation is the confirmation.

**Say the resolved target back, with its date and id.** "tuesday = 2026-09-22, Tabla chicken curry, order 21043117."

#### Check the cutoff before touching anything

- **Cutoff still open** — everything below works.
- **Cutoff passed, cancel-only** — cancel it. Say the day cannot be refilled.
- **Cutoff passed, reorder asked for** — **do neither and say so.** The current order is still coming; let them decide.

#### The swap sequence, in this order

**Build the replacement before destroying the original.** See **Editing a submitted order** in `order-history.md`.

1. **Read that day's menus and build a slate**, exactly as an order run does. Exclude the cancelled build and anything that is obviously the same idea.
2. **Check rotation against the day before and the day after.** A cancelled order holds no format slot.
3. **Remove the old item's line from the existing order**, before placing anything. **The stipend is shared across every order on that day**, so leaving the old line live can push the day over and trigger a card prompt.
4. **Place the replacement** and verify its cart reads **Total $0.00**.
5. **Then cancel the emptied original.** Removing its last line does not cancel it; it sits at a ~$1.00 subtotal looking placed.
6. **Confirm exactly one live order remains for that day** on `/customer_orders`.

**If no replacement clears the gates under the ceiling, stop and leave the original alone.** Say which gate blocked it.

**Close with a lazy sync.** The cancelled row moves to the `Cancelled` table with its shape and reason; the replacement is a new `placed` row; rotation state is set from the replacement.

#### A cancellation they chose is a signal. A cancellation the calendar chose is not.

| Shape | What happened | Signal? |
|---|---|---|
| `swap leftover` | mechanical cleanup of an entree change this skill made | **None.** |
| `day dropped` | no delivery that day, usually not in the office | **None.** Attendance — `stated` 2026-09-17 |
| `rejected` | **they looked at a placed order and did not want that food** | **Yes.** Read it |

**The shape is decided by what they asked for:**

| They asked for | Shape |
|---|---|
| a plain cancel, no reason, or a reason about the day | **`day dropped`** |
| a plain cancel, with a reason about the food | `rejected` |
| **cancel and reorder** | `rejected` |

**A plain cancel is never a rejection.** `stated` 2026-09-17. **A `rejected` build never gets re-picked as though it were untested.** Do not move a `day dropped` kitchen down a slate. A run of dropped days at one restaurant is worth a glance.

##### A stated reason is the strongest signal this skill can get

`stated` 2026-09-17: *"if the user gives a reason why they want to cancel and re-order, that's a strong signal."*

- **About the day** — "too heavy for today". Record it in the log, apply it to this replacement only.
- **About the food** — "nothing creamy", "too much rice". **Write it into `dietary-preferences.md` as `stated`**, in their words, dated.
- **Unclear** — treat it as about the day and say so: "took that as a today thing, tell me if it's a standing rule."

**Use the reason to build the replacement.** "Too heavy" means lighter, not merely different.

**Ask why only on a reorder, only once, and only if they did not already say.** Never on a plain cancel:

> Anything about that plate you want me to avoid next time, or was it just not what you felt like?

Record the answer in the `Reason` column verbatim. **"Not stated" is a normal entry**, so a later run can tell an unasked question from an unanswered one.

## `/feed-me refine`

**The onboarding questions, pointed at a preferences file that already exists.** Same machinery as **Ask questions the log raised**.

**It reads the two local files and nothing else** — no browser, no scrape, no sync. **It requires both files.** With either missing, name it, offer the run that would build it, and wait. If the log looks stale, say so and offer a sync.

| | onboarding | refine |
|---|---|---|
| **Has stated rules to test against** | no | **yes, and that is the point** |
| **Questions** | five, allergies first | up to about eight; allergies only if still unanswered |
| **Reports findings with no question attached** | no | **yes** |
| **Writes** | creates the file | amends it |

**The file itself is the new material:**

- **A `stated` rule the ratings contradict.** Raise it, never overrule it.
- **A `derived` rule the log no longer supports.** Revise it and say so.
- **A `provisional` rule never tested since it was written.** Find evidence or retire it.
- **A `derived` pattern that has held long enough to promote.** Ask; `stated` makes it durable.

### Gaps, which are the reason to run it

Nothing in a log points at what is not in it. Sweep all of these:

- **A format never once ordered.** Deliberate avoidance and never-came-up look identical in a log.
- **A format used once and rated well.**
- **A cuisine carrying a strong average on one or two orders.**
- **A `loved` kitchen that keeps coming up on open days and keeps getting passed over.**
- **Unrated orders that were whole meals.** Skip the side orders.
- **Days cancelled with nothing delivered**, only if several cluster at one restaurant.
- **Axes the file has no opinion on** — spice as a scale, how a dish travels, texture, how much of a plate they finish.

**Lead with the finding, not the question**, and report consistencies even where no question is attached. **Write only `dietary-preferences.md`, and only from answers** — except `derived` corrections, which apply regardless. Move `Last reviewed`. **An unanswered refine is not a failed one.**

## Preferences: best with them, fine without

**Every gate reads its numbers from `dietary-preferences.md`**; this file never hardcodes a dietary number, because the next user's will be different. **Never refuse to order because the file is thin or missing:**

| What exists | What to do |
|---|---|
| **Preferences file** | Read it first. Order against it. |
| **No preferences, but order history** | Derive provisional preferences from the ratings and order against those. Write them down as you go. |
| **Neither** | **Full sync first**, then questions, then order. See below. |

### Cold start: full sync, then questions, then lunch

**With neither file, run a full sync before anything else.** The account almost always has history even when the skill does not. Say so in one line first:

> no history or preferences here yet, so i'm reading your ezCater account first. one sec.

1. **A full sync.** Build `order-history.md` from it.
2. **Derive `dietary-preferences.md`**, per **Deriving preferences from the log alone**.
3. **Ask the clarifying questions**, per **Ask questions the log raised**.
4. **Place the orders anyway.** Do not wait for answers; say orders stay editable until each day's cutoff.
5. **Close with the lazy sync.**

**Allergies are the one thing the log cannot tell you.** Never ordering shellfish is not evidence of an allergy. If they ignore the question, record **"no limits declared — never confirmed"** in **Hard limits** and ask again next run.

### Deriving preferences from the log alone

Estimate macros for every delivered order component by component, then read the distribution:

| Rule | Derive it from | Round |
|---|---|---|
| protein floor | the **median** protein across orders rated 3 or better | down, to a 5g step |
| calorie ceiling | roughly the **75th percentile** of calories across those same orders | up, to a 50 cal step |
| spend ceiling | the arithmetic ceiling for that day's subsidy | to the cent |

Cuisine ranking comes from averaging ratings by cuisine; component preferences from what recurs in highly rated builds; preparation signals from contrasting the 4s against the 1s and 2s. A format that is most of the log is a habit, not a rule.

**These describe what they have been eating, not what they want.** Tag every derived number `provisional`, **show the number with its basis** ("protein floor 40g, the median across your 4s and 3s"), and never present a derived ceiling as a hard gate. The first time they contradict it, they win and it becomes `stated`.

### When there is nothing at all

Order anyway, with sane defaults: one real menu item, inside the day's budget, nothing exotic, nothing that assumes a restriction. Ask the allergy question and nothing else. Then **create `dietary-preferences.md` as a stub** with the section headings, empty rule tables, and the **How this file evolves** section, and say you are ordering blind.

### Ask questions the log raised, not questions off a form

**Mine the log first, then write the questions from what it found.** Ask about ambiguity, never about what the log already settles. Rank candidates by **how much the answer changes tomorrow's order**:

| Pattern | Why it earns a question |
|---|---|
| **A wide spread inside one category** | Latin averaging 3.0 off a 4, a 3 and a 2 means the cuisine label is not driving the rating. |
| **A rule the log argues with** | `carb-base` is 26 of 40 orders and carries 9 of the 16 fours, while the stated rule rations it to one in three. |
| **A contradiction between components and outcomes** | They want tahini, hummus, feta, olives, and rate Mediterranean kitchens lowest. Components or kitchens? |
| **One data point carrying a whole rule** | A single tofu bowl rated 1 is now a hard limit on tofu. |
| **A tight band that might be a constraint** | Every subtotal lands in a $2 window. A floor they want, or just what lunch costs? |
| **An absence** | A format or cuisine never once ordered. |
| **A favourite that collides with a gate** | Cheat day, or does the floor hold? See **Cheat day**. This one outranks most of the table. |

**At most five, and the first one is always allergies.** Each question carries its evidence inline, is answerable in a few words, offers the reading you would take so agreeing is one word, and is skippable. Record a skip as *no preference stated*. Ask them all in one message, in Claude's normal voice.

Worked shape:

1. Anything you're allergic to or won't eat? The only thing your orders can't tell me.
2. 26 of 40 orders are carbs and they hold 9 of your 16 favourites, but the rule rations them to one in three. Drop the rule?
3. You like tahini, hummus and feta, but Mediterranean kitchens are your lowest-rated cuisine. Components, or the kitchens?
4. One tofu bowl, rated 1. Tofu out entirely, or was that just a bad bowl?

**With no log to mine**, fall back to the plain set: hard limits; a calorie ceiling or none; any macro they track; cuisines; components and prep. **"None, I just want good food" is a complete answer.**

**Then write the file** using the structure in this repo, tagging every rule. **Anything you proposed and they confirmed is `stated`.** Summarise what got written, in greentext, and proceed with the run they asked for.

### Keep it current as the log grows

**`dietary-preferences.md` is a living file.** After any run that reads new reviews:

- **A `derived` rule the ratings no longer support** — revise or delete it, and name the change.
- **A pattern holding across three or more rated orders** — write it in as `derived`, citing the orders.
- **A pattern resting on one or two** — `provisional`, or leave it out.
- **A `stated` rule the ratings contradict** — leave it. Raise it once with the evidence.
- **Anything they say in conversation** — `stated`, dated, in their words.

**The tags let the skill correct itself without overwriting the user.** The Mediterranean entry is the worked example: a `derived` belief that survived nine contradicting orders because nothing marked it revisable.

## Their reviews live on ezCater, not in this conversation

**They rate orders on the site.** Do not ask them to re-state a verdict they already left there. Re-scrape whenever an order has been delivered since the last snapshot. They often rate a day or two late, so re-check unrated rows next run.

ezCater renders five stars but stores a **0-4 integer**: 4 is `loved` and 0 is `never-again`. See **After they order**.

### How to scrape it

Ratings and review text live **only** on the details page, in markup the accessibility snapshot does not surface. Use `browser_evaluate` and parse the HTML.

| What | Where |
|---|---|
| Order list | `/customer_orders/past?page=N`, N from 1 until a page yields no `order_details` links |
| Upcoming / Canceled | `/customer_orders` and `/customer_orders/cancelled` |
| Order id | the digits in the `order_details` href. **Record it in the log.** |
| Rated or not | the list card carries `Reviewed on <date>` once rated, and `Leave a Review` while it is not |
| Cancelled | the detail page opens with `This order was canceled.` and has **no** `.order-review-ratings` panel |
| Details page | `/customer_orders/<id>/order_details` |
| Rating | `#order-review-numeric-rating` → `data-review-rating` attribute |
| Review text | `.order-review-footer` → text content. **Absent when they left no text.** |
| Restaurant | `.order-review-header` text, minus the `Your order from ` prefix |
| Items, prices, totals | the `Your Order` block of `main` |

Traps:

- **Every list card duplicates its `order_details` link** (mobile and desktop markup). De-duplicate by order id.
- **`.order-review-header` only exists when a review panel exists.** For unrated orders, take the restaurant name from the list card.
- **Header and footer sit in the same container.** Read the two selectors separately rather than slicing one string.
- **Do not walk up the DOM from a list card to read its rating.** A parent that spans several cards leaks their `Reviewed on` text in. Use the detail page.

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

Loop it over the ids with ~100ms between fetches.

**The Upcoming tab's contents are not evidence that anything is pending.** With no live order, `/customer_orders` returned the ten most recent **completed** ids. **Read it as a set, never a count**, and confirm a just-placed order by finding *its* id there. Its pagination links point at `/customer_orders/past?page=N`, so do not follow them. And a day's order appears there on its delivery day while also appearing on Completed — see the precedence rules in **The order lifecycle**.

Keep the delivery day and time from the details page. Do not copy the name or street address into the log.

### Reading a review

**The star rating is the verdict. The text is the reason.** They write text only when something went wrong, so **a 4 with no words outranks a 3 with a paragraph.** Complaints are about execution — freshness, moisture, seasoning, portion honesty — which the gates cannot see.

### Reviews arrive from two places. Merge them, never overwrite.

**A row's review text is the union of the site and what they tell you in chat.** A sync that finds no `.order-review-footer` must never blank out something they said here.

| Field | Authority |
|---|---|
| **Star rating** | **ezCater, always.** Chat sets one only if they state it outright ("that was a 4") |
| **Review text** | **Both, merged** |
| **Verdict** | derived from the rating. Unrated plus a glowing chat review is still `delivered` |

| Situation | How the cell reads |
|---|---|
| site only | `"Pretty basic sandwich"` |
| chat only | `chat: "ok, too much lettuce"` |
| both | `"Pretty basic sandwich" · chat: "bread was stale too"` |

**Deduplicate on meaning.** When the two conflict in direction — a 4 against a complaint — keep both and raise it once. Their answer is `stated`.

## The data files are personal. Publishing them is the user's call.

**`order-history.md` and `dietary-preferences.md` hold what someone eats and what they cannot safely be served.** Record allergies in full, including severity; never water down a safety line to make a file more shareable. `SKILL.md` and `README.md` are the portable part and carry no personal data.

- **Never run `git push` on your own initiative.** Committing locally is fine.
- **Never add a remote, change one, or make a repo public.**
- **When they ask for a push, do it**, after saying once that the data files carry their records — unless those files are in `.gitignore`, as they are in this repo.
- **Never write a password, session cookie, or payment detail to any file.**

## Never enter a credit card

**Not once, not for a few cents, not to save an order you already built.** A card prompt is a **diagnostic**: the build went over the day's stipend, or the run is on the wrong page, flow or fee. **Stop, do not submit, and fix the build** until the cart reads Total $0.00. If no build on that day's menus can, place nothing for that day and say so.

## Budget

**The number that matters is the total, tax included.** Default stipend is **$20.00 per day**, and crossing it by one cent makes the checkout ask for a card.

**Unused stipend is not wasted stipend.** `stated` 2026-09-17: *"they're buying me lunch, not the other way around."* **The one money rule is $0.00 out of pocket.** There is no spend floor.

**Budgets are per day and strictly independent.** Read each day's own subsidy off its page.

### The menu is pre-tax. The gate is post-tax.

```
total = subtotal x (1 + tax rate)
max subtotal = subsidy / 1.07, rounded DOWN to the cent, then back off one more cent
```

At $20.00 the division gives $18.6915, which rounds down to $18.69 and grosses to exactly $20.00 with no margin. **$18.68** is the number to shop against:

| Subtotal | x 1.07 | Total | Out of pocket |
|---|---|---|---|
| $18.68 | $19.9876 | **$19.99** | $0.00 — the last safe subtotal |
| $18.69 | $19.9983 | **$20.00** | $0.00, but zero margin. Do not use it. |
| $18.88 | $20.2016 | $20.20 | $0.20 — the receipt that proved it |

### Verified constants

From receipts. Update when a new receipt contradicts one.

| Constant | Value | Source |
|---|---|---|
| Meals tax (MA) | **7.00%** | holds across all 40 receipts |
| Delivery fee | $0.00 | all 40 orders |
| Max subsidy | **$20.00** | 2026-09-01: an $18.88 subtotal drew $20.00 and cost $0.20 |

Rules:

- **Spend what the build needs and nothing more.**
- **The budget's job is to buy protein.** Short of the protein floor with room under the ceiling, take the paid add-on — `stated` 2026-09-17.
- **If a day's posted subsidy is not $20.00, recompute that day's ceiling from it.**
- **A delivery fee is pre-tax and comes off that day's max subtotal.**
- **If a receipt shows a different tax rate, it wins.** Update the constants table.
- **Trust the cart over the arithmetic** in order-mode. See **The cart overrides the estimate**.

**Never add a drink or a dessert at all** — a hard limit in `dietary-preferences.md`, `stated` 2026-09-17: *"save that money for protons."* Never add a side to close a price gap either.

## Priority ladder

Resolve every recommendation in this order. Lower rules never override higher ones.

1. **Hard gates, read from `dietary-preferences.md`.** Its **Hard limits**, macro floors and calorie ceiling. Plus: total at or under the spend ceiling, one real menu item, nothing on the Never Again list, nothing they rated 1 or 0.
2. **Their verdict on it, and whether they went back.** Rank by the tiers in **Two signals to return**.
3. **Calories.** Among options with the same standing at rule 2, fewer is better.
4. **Variety.** Variety may spend up to **~150 cal** against rule 3 to avoid a repeat, but never past a stated ceiling. Past that, calories win, and say so.
5. **Component preference.** Wanted components and the grain preference break remaining ties.

**Rule 2 sits above calories deliberately.** A menu cannot tell you the pita will arrive dry; their ratings can.

Before finalising a slate, check every candidate against `order-history.md`: **have they rated this dish or restaurant, did they write anything about it** ("dry", "barely any steak" — fix it with a mod or pick something else), **and does it match every component rule** in `dietary-preferences.md`. Say in the writeup what moved each rank.

### The dietary gates

**Every number is read from `dietary-preferences.md`.** Inventing a gate it does not state is as wrong as ignoring one.

- **A floor is a minimum** — protein, fiber. Floors do not bend.
- **A ceiling is a maximum** — calories, sodium, carbs. It bends only when nothing on a day's menus fits under it, and then with the overage named.
- **When a floor and a ceiling collide**, the floor wins. Say which ceiling got spent.

If nothing on a day clears the gates, place the closest qualifying build and name which gate it missed and by how much. **If the file sets no calorie ceiling, rule 3 is inert.** Do not quietly reintroduce one.

## Estimating macros

**Required. Estimate component by component, never as a single guess.** A whole-plate eyeball once reported a GRECO order at ~830 cal when it was ~1,175 — the pita, the rice pilaf and the dressing oil were not counted.

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

The skipped lines are never small: **a pita** is ~165 cal; **a tablespoon of oil or dressing** ~120 cal; **a starch side** ~220-400 cal; **a 2.5oz sauce cup** ~70 cal.

### Required: re-verify after writing the mod

Re-run the estimate for the build as ordered. Read "heavy on X" as **roughly +30% of that component**, with an upper bound at double.

- **Against a ceiling, count the mod** at the upper bound. If it crosses, the mod comes out.
- **Toward a floor, count nothing.** A note is a request, not a purchase.

The figure in the writeup and the log is the build as ordered, mod included.

### Required: final recalculation before submitting

**On the review page, with the order still unsubmitted,** recalculate every number from the order exactly as listed plus the note as written. Builds drift. If it no longer clears every gate, fix it before submitting: cut the mod, then swap a component, then swap the item. **Never submit a build you know misses a gate** — adding an item to a submitted order opens a new order.

After placing, check the confirmation against this recalculation and correct any drift before the cutoff. The figures reported are always this final recalculation.

## Variety and rotation

Track the **format** of every order in `order-history.md`:

| Format | What counts |
|---|---|
| `salad` | greens-base bowl or plate, protein on top |
| `carb-base` | wrap, sandwich, sub, burrito, pizza, pasta, rice bowl, grain bowl |
| `protein-plate` | grilled meat or fish with non-grain sides, kebab plate, tandoori, egg dishes |
| `bowl-no-grain` | bowl built on beans, protein, or a non-grain base |
| `soup-forward` | protein-heavy soup, stew, chili, or pho as the actual meal |

Rotation rules, strongest first:

- **No format twice in a row.**
- **`carb-base` at most once every three orders.**
- **At least three distinct formats across any rolling four orders.**
- **No exact repeat item within four orders**, unless an anchor or loved restaurant earns it.
- Prefer a restaurant other than the last one.

**Rotation runs through a multi-day batch.** Day one is checked against the last logged order, day two against day one, and so on. **Plan the whole batch before placing anything**, so every day gets a different format, no dish repeats, and restaurants spread out.

Rotation rules are preferences, not gates. If every option that clears the hard gates is a blocked format, order it and name the rule you broke.

### Two signals to return, and they are not the same signal

**A rating is what they said. A re-order is what they did.** `stated` 2026-09-17: *"a re-order is signal and also a perfect rating is a signal — both to reorder again from the same restaurant."*

| Tier | Carries | How it ranks |
|---|---|---|
| **Anchor** | a 4 **and** a re-order | **Beats an untested option on a tie.** Exempt from "prefer a different restaurant." Repeat freely, same dish included. |
| **Loved** | a 4, never re-ordered | Permission to repeat. An untested option still wins a tie. |
| **Habit** | re-ordered, never a 4 | Ranks above untested only when the day's alternatives are weak. |
| **Untested** | neither | The default pick when it clears the gates. |
| **Neutral or worse** | a 2 or below | Never returned to on rotation grounds. |

- **Only count a re-order they chose.** A repeat this skill placed counts only when another qualifying restaurant was open that day.
- **A same-day cancellation is not a re-order.**
- **The newest rating still wins on the food.** A re-order followed by a 3 does not become a 4.
- **Two signals do not clear a gate.**

When a repeat fires, name the signals it rides on: "GRECO, rated 4 in April and re-ordered in September." Recount tiers during any sync that adds an order, and call out a promotion to anchor.

## Standing constraints

**These live in `dietary-preferences.md`.** Two bite during a build:

- **One real menu item**, plus at most one add-on or upgrade. Three à-la-carte sides stacked into a fake entree is not an entree.
- **Buy one protein well rather than two badly.** When a build stacks two paid proteins, the premium one arrives short.

If a menu forces a choice no rule covers, ask, then write the answer into `dietary-preferences.md` as `stated`.

## Modifications

Apply the **Components** rules in `dietary-preferences.md`. Do not use mods to reshape a dish into a different dish.

**Write it short and polite.** `stated` 2026-09-23: *"the kitchen has a lot of orders to fill, the skill needs to be polite and concise."* Earlier runs wrote long notes with several requests. A real person reads that box during a lunch rush, so write one sentence in plain courteous English, with a please and a thank you, phrased as a request the kitchen can decline. No greentext. `Hot sauce on the side, please. Thank you!` is the whole shape.

**One ask per note.** `stated` 2026-09-17. A box with two asks gets one honored at best. **One ask is one action**: "heavy on the feta and olives" is one; "extra jalapeño **and** hot sauce on the side" is two.

| | Ask | Why it outranks the rest |
|---|---|---|
| 1 | **a reduction or removal** | free, honored most often, and it takes off the thing that would ruin it |
| 2 | **a freshness ask**, at a counter whose failure mode is held food | targets the failure that produced both 2s in the log |
| 3 | **heat** — hot sauce on the side — when the modal did not capture it | `stated` standing instruction |
| 4 | **a garnish-tier addition** — extra feta, olives, pickles | marginal, and the first thing to cut |

**An ask for something the plate lacks beats an ask for more of what it has.**

**Never ask for more meat, or more of anything the menu prices separately.** `stated` 2026-09-16. **Buy it in the modal**: `Extra chicken +$3.80` is a real order, "heavy on the chicken please" is not. Garnish-tier asks — cheese, olives, pickles, herbs, sauce on the side — are fine.

### Required: validate the note against the order

Build the order's real ingredient list from the item's description plus every option the modal captured. Then test each clause:

1. **Does the ingredient appear in the list?** If not, cut it.
2. **Is it already set by a selector?** Cut it.
3. **Is it asking for more of something the menu charges for?** Cut it and buy it in the modal.
4. **Can the kitchen act on it?** "mediterranean flavors please" cannot. Cut it.
5. **Is more than one ask still standing?** Keep the highest-ranked one.

What survives is **one ask**, or an empty box.

Worked example, the 2026-09-16 GRECO order: two beef bifteki patties, Greek salad (tomato, cucumber, red onion, Kalamata olives, feta), pita, lemon rice pilaf (selected side), tzatziki.

| Clause | Test | Result |
|---|---|---|
| "lemon rice pilaf for the side" | 2 — the selected side | cut |
| "light on any lettuce" | 1 — no lettuce in the list | cut |
| "heavy on the feta and olives" | passes, one ask | keep |

Worked example of test 5, the 2026-09-17 Cosi order: "extra pickled jalapeño" (rank 4, already on the plate) lost to "hot sauce on the side if you have it" (rank 3, not otherwise on the plate). Final note: `Hot sauce on the side, please. Thank you!` **Both clauses passed tests 1-4**, so run test 5 every time.

## Output style

**Everything the user reads comes back as 4chan /fit/ greentext — except anything that asks them for something.** `stated` 2026-09-17. Slates, receipts, verdict acknowledgements, sync reports.

**Meme-forward and short.** A gym rat who lifts and does not explain itself.

- Every line starts with `>`. Lowercase. No terminal punctuation. Fragments over sentences.
- **One idea per line. Under ten words.** No line whose job is to introduce the next.
- Open with a `> be me` stanza, four or five lines, landing on `> mfw` or `> unacceptable`.
- Close with `> we go again`, once, at the very end.
- **Safe for work.** No slurs, no sexual content, no self-harm bits, no body-shaming the user.
- **Never gender the user.** `stated` 2026-09-18. No `bro`, `my dude`, `man`, `king`, `sir` or `ma'am`, no gendered pronoun. **This binds every word the skill writes and every file it maintains: `they/them` about the user, always.**
- Dialect: protons, gains, mirin, DYEL, mogs, ngmi, natty, bulk, cope, "one (1)", "unacceptable", "we go again".

### Questions are never greentext

`stated` 2026-09-17: "the green text is just for responses that require nothing from the user." **Anything that requires them to act — a question, a choice, a confirmation — goes in Claude's normal voice**, outside the greentext block, numbered, after it. **Never put a question list inside the greentext**; it reads as another finding and gets skimmed past.

### Length

A ranked slate is at most **five stanzas**; a receipt **three**; a skill-update report **two or three**. A stanza past six lines is doing two jobs.

**The numbers stay exact.** Prices, tax, total, protein, calories, format tags and mod text render verbatim. When voice and data collide, data wins.

```
> 1. scali deli — chicken kebab plate — $18.20
> protein-plate | ~52g protons | ~640 cal
> mod: no rice under it, extra hummus
> mediterranean gains, zero lettuce tax
```

```
> boonnoon — kao soi + beef + chili oil — $18.50
> soup-forward | ~52g protons | ~900 cal
> tax $1.30 | subsidy -$19.80 | total $0.00
> twenty cents left on the table
```

**Multi-day runs:** one stanza per day in delivery order, then one closing stanza for the week, then `> we go again`. If the batch cannot fit, cut the commentary, never the numbers.

## The slate

Build **five options** per day, ranked, each a complete order in the shape above. **Rank 1 is what gets placed.** The five must span **at least three formats**. With several days open, rank each day against the batch plan, not in isolation. On a long week, trim runners-up to the top two per day and say so.

Close with a one-line **variety note** on how this moves off the last order, and one line each for **near-misses** — an item just over the ceiling or just under the floor. Say once that the macros are estimates. If the menus cannot produce five qualifying options, give what there is; never pad with items that miss a gate.

### Cheat day

**When a favourite is on the menu and nothing on it can clear a gate, ask whether this is the day the gate bends.** Never silently rank the favourite last.

**A favourite** is a cuisine or format with several 4s, nothing under a 3, and more than two orders, or one `stated` as a favourite. A single 4, or a high average off one or two orders, is not one yet. Write favourites into `dietary-preferences.md` tagged `derived`. **Cap the list at two.**

**If the favourite clears every gate, there is nothing to ask** — order it.

| Bends | Never bends |
|---|---|
| a macro floor | **a hard limit.** An allergy is a wall |
| a calorie, carb or sodium ceiling | **the money rules.** $0.00 out of pocket, the tax-derived ceiling |
| the format rotation for that one day | anything `stated` as absolute |

1. **Build the favourite as well as the menu allows**, every paid protein add-on taken.
2. **Build the best alternative that clears every gate.**
3. **Ask, and call it a cheat day**, naming the gap: *"Sushi lands on Thursday — your best-rated cuisine, five 4s and nothing under a 3. The 8-piece nigiri gets to about 38g, seven under the floor. The chicken curry clears it at 52g. Cheat day, or the curry?"*
4. **Place nothing for that day until they answer** — unless the cutoff is close, then take the favourite and say so.

**Record the answer.** Took it: note it under the log. Passed: the next collision leads with the alternative. Took it three times running: raise the gate itself in the next refine. **Refine is where cheat day gets settled for good**, as a `stated` rule.

## Live ordering

### Which browser drives this

Take the first driver that is actually in the session:

| Order | Driver | Tools | Where it shows up |
|---|---|---|---|
| **1** | **Playwright MCP** | `browser_navigate`, `browser_evaluate`, `browser_click`, `browser_snapshot` | **Claude Code in the terminal.** Every snippet below is written for it |
| **2** | **the built-in browser pane** | `mcp__Claude_Browser__navigate`, `…__javascript_tool`, `…__read_page`, `…__computer` | **the Claude desktop app.** Verified end to end 2026-09-17 |
| **3** | Claude in Chrome | `mcp__claude-in-chrome__*` | when the user asks for it by name, and **always for a scheduled run**. Verified end to end 2026-09-23. See **Running it on a schedule** |
| — | nothing | — | **advise-only** |

**Take driver 2 when Playwright is absent or will not connect.** Never drop to advise-only while a working browser sits in the session.

| This file says | Pane and Chrome equivalent |
|---|---|
| `browser_evaluate` | `javascript_tool` with `action: "javascript_exec"` |
| `browser_click` | an `el.click()` inside `javascript_tool` |
| `browser_snapshot` | `read_page` |
| navigating | `navigate`, or `preview_start` with a `url` to open the pane |

#### Three differences that will cost a run if you miss them

**1. The snippets here are functions. `javascript_tool` evaluates the last expression**, so a bare `async () => {...}` returns the function object. Wrap it:

```js
async () => { ... }              // as written here, for browser_evaluate
(async () => { ... })()          // the pane's javascript_tool
await (async () => { ... })()    // Claude in Chrome's javascript_tool
```

**2. Set modal options with `el.click()`, never `el.checked = true`.** The live price in `input[name="commit"]` is recalculated by a change handler that `.checked` does not fire, so the price silently stays at the base. For the notes box, set `.value`, then dispatch `input` and `change`.

**3. Under Claude in Chrome, `await` the IIFE.** Without it the promise is not awaited and the call returns `{}`, with no error. An empty scrape of the order tabs looks like an account with no orders.

### Getting in

Start at `https://mealprogram.ezcater.com/users/sign_in`. A live session lands on `/schedule`.

**Never type their password.** On the password screen, stop and tell them to sign in in the open browser window themselves. Never ask for a password in the conversation. A "Sign in" link in the nav means not authenticated.

### Reading the menus

`/schedule` lists **every orderable day**, and each day holds several restaurants as `/schedule_entries/<id>` links with their own **order-by cutoff**, delivery time, fee and subsidy. Today is `/schedule`; other days are `/schedule/<YYYY-MM-DD>`.

**Enumerate the days first.** A covered day shows its item instead of a restaurant list. Never re-order a covered day.

```js
async () => [...document.querySelectorAll('a[href^="/schedule/"]')]
  .map(a => ({ day: a.innerText.replace(/\s+/g, ' ').trim(), href: a.getAttribute('href') }));
```

**A closed day's restaurants read `Time's up!` and `Stopped accepting orders at <time>`**; an open one reads `Order by <time>`. Two days showing does not mean two days open. One open day is a complete run.

**Cutoffs are per restaurant and early** (typically 9:20-9:30 AM). Read **every restaurant on every open day** before placing anything, because the batch is planned whole.

Pull menus compactly rather than with a full snapshot:

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

Listed prices are base prices. The real number lives behind the item's option modal.

**Navigating or fetching an `order_items/new` URL silently redirects back to the menu** with a `200`. You have to click the link — `a[href*="menu_item_id=<id>"]` — and wait ~2s for the modal to mount.

The modal's submit button **value is the live running price**:

```js
() => document.querySelector('input[name="commit"]').value   // "Add to cart - $18.50"
```

To survey several items, click each link, scrape the largest `div[class*="odal"]`'s `innerText`, then click the `×`, with ~1.5-2s waits.

#### Selecting the options, and reading the price back

Every option group renders as an input named `options[<groupId>]choices[]` — a **radio** for a required single-select, a **checkbox** for an optional add group. The choice id is the input's `value`; the label is `label[for="<input id>"]` or the enclosing `<label>`.

```js
async () => {
  const m = [...document.querySelectorAll('div[class*="odal"]')]
    .sort((a, b) => b.innerText.length - a.innerText.length)[0];
  return [...m.querySelectorAll('input')].map(i => ({
    type: i.type, value: i.value,
    label: (m.querySelector(`label[for="${i.id}"]`) || i.closest('label'))
             ?.innerText.replace(/\s+/g, ' ').trim()
  }));
}
```

Then click by value, pausing so each recalculation lands, and read `commit` back:

```js
async () => {
  const pick = v => [...document.querySelectorAll('input')]
    .find(i => i.value === v && i.type !== 'hidden')?.click();
  pick('<base choice id>');    await new Promise(r => setTimeout(r, 600));
  pick('<side choice id>');    await new Promise(r => setTimeout(r, 600));
  pick('<add-on choice id>');  await new Promise(r => setTimeout(r, 1200));
  const ta = document.querySelector('textarea[name="order_item[notes]"]');
  ta.value = '<the validated note>';
  ta.dispatchEvent(new Event('input',  { bubbles: true }));
  ta.dispatchEvent(new Event('change', { bubbles: true }));
  return {
    commit:  document.querySelector('input[name="commit"]').value,   // "Add to cart - $17.48"
    checked: [...document.querySelectorAll('input:checked')].map(i => i.value)
  };
}
```

**Confirm `checked` is exactly the choices you meant** — an unset required group blocks the add, and a stray checkbox is an add-on nobody asked for. A required protein swap is usually cheaper than the same protein added as an extra.

### The cart overrides the estimate

The cart renders **Subtotal, Delivery fee, Sales tax, Company subsidy, and Total** before you commit. Build to the highest subtotal the build needs whose Total reads **$0.00 with the subsidy absorbing all of it**, and never past $18.68 at a $20.00 stipend and 7% tax. An amount due or a card request means back the build down.

### Placing it

Each day is a separate cart and a separate order. Once per open day, in date order:

1. Fill the notes box with the mod.
2. **Validate the note** — see **Required: validate the note against the order**.
3. **Re-estimate with the mod included** — see **Required: re-verify after writing the mod**.
4. Add to cart, then verify that day's cart Total is $0.00.
5. Continue to `/orders/<id>/review`.
6. **Leave the utensils box unchecked.** It defaults to unchecked; they do not want the plastic, even for soup.
7. **Run the final recalculation** — see **Required: final recalculation before submitting**.
8. Confirm the review page still shows Total $0.00 and no card request, then place the order.
9. Check the confirmation against step 7.
10. **When every day is placed, run the lazy sync.** The run is not finished until the site confirms every order.

**Finish the batch.** If one day fails, place the remaining days anyway and report which day was skipped and why.

Report every day and delivery time, each day's receipt, and that each order stays editable until its cutoff.

## Running it on a schedule

**Never offer this unprompted.** If they ask, give them this. Verified 2026-09-23.

**Why it needs Chrome.** The Claude desktop app's built-in browser pane does not keep ezCater's login cookie between runs. It keeps other SSO logins, just not ezCater's, so every scheduled run in the pane lands on the sign-in screen and orders nothing. Chrome keeps it. So a **Claude desktop scheduled task** is the scheduler, and it drives **Claude in Chrome** as the browser.

### Setup, once

1. **Chrome:** install the Claude in Chrome extension, sign in with the same Claude account as the desktop app, and sign in to `https://mealprogram.ezcater.com` once.
2. **Claude desktop:** enable the Claude in Chrome connection and create a scheduled task that runs daily in the morning. Daily because the open days are irregular (`stated` 2026-09-21) and a run with nothing to do orders nothing; morning so it can claim a day before the earliest cutoff, ~9:10 AM.
3. **Run the task once by hand** and approve its tools, including `open` and `osascript`. An unapproved task stalls on a permission prompt with nobody there.

**Create the task only when they ask.** It places real orders on days nobody reviewed.

### What has to be true when it fires

- **Claude desktop is running.** Minimized is fine; quit means the run silently does not happen.
- **The Mac is awake.** Locked is fine; asleep is not.
- **Chrome does not need to be open.** The run fixes that itself.

### What the scheduled prompt must do

Each run starts with no memory, so the prompt carries all of this:

- **Invoke this skill by name**, and say this file and the two data files outrank the prompt.
- **Drive Claude in Chrome (`mcp__claude-in-chrome__*`), never the pane.** Load its deferred tools in one `ToolSearch` call.
- **Get Chrome connected before anything else:**
  1. If `list_connected_browsers` is empty and Chrome is not running, `open -g -a "Google Chrome"`.
  2. **Always make sure Chrome has a window**: `osascript -e 'tell application "Google Chrome" to make new window'`. A Chrome with no windows drops the extension's connection, even with the process running. Closing every window on macOS leaves Chrome in exactly that state.
  3. Poll `list_connected_browsers` every ~15 seconds for up to 2 minutes.
- **Say it is unattended.** Take the documented fallbacks and never block on input.
- **Restate the hard lines** — no credit card, no password, no `git push`.
- **Make a failure the headline**, in plain English, with every open day and its cutoff so they can order by hand. A login failure means ezCater needs signing in again in Chrome. Still not connected with Chrome running and a window open means the extension is signed out, usually after a hard reset of Chrome. **Only they can fix that: open the Claude panel in Chrome once.**

## The order lifecycle

**The ezCater site is the source of truth. `order-history.md` is a cache of it.** When the two disagree, the site is right and the log gets corrected. A row saying `placed` is a belief, not a fact.

| Status | Means | How the site shows it |
|---|---|---|
| `placed` | submitted, not yet delivered | on the **Upcoming** tab |
| `delivered` | arrived, not yet rated | on **Completed** with a `Leave a Review` link |
| `rated` | has their star rating | on **Completed** with a `Reviewed on <date>` line |
| `cancelled` | cancelled by anyone, for any reason | on the **Canceled** tab; its detail page says `This order was canceled.` |
| `missing` | in the log, nowhere on the site | investigate, do not delete |

### Write the row the moment it is placed

**Log an order as soon as the confirmation comes back**, at status `placed`, with delivery date, restaurant, item and every add-on, **format**, subtotal, and the order id from the confirmation URL. The lazy sync corrects it. **One row per order, not per day**, dated by **delivery** day. Set the rotation state from the last row in a batch.

### Reconcile against the site every run

**Before building any slate, scrape all three tabs and check the log against them.**

| Tab | URL | Paging |
|---|---|---|
| Upcoming | `/customer_orders` | single page |
| Completed | `/customer_orders/past?page=N` | **loop until a page returns no orders** |
| Canceled | `/customer_orders/cancelled` | single page, but **probe `?page=2`** — a full page of ten renders no pager |

**An order can sit on two tabs at once.** On its delivery day it appears on Upcoming *and* Completed. Build an id-to-**tabs** map and resolve status by precedence:

1. **On Canceled** wins over everything else.
2. Otherwise, **a rating on the detail page** makes it `rated`.
3. Otherwise, **on Completed** makes it `delivered`.
4. Otherwise, **on Upcoming only** makes it `placed`.

Then walk the log: advance statuses that moved forward; set `cancelled` for anything on Canceled, **whether or not this skill did it**; add orders on the site but not in the log; set `missing` for rows on no tab and say so.

**Finish with a set comparison, not an impression.** Diff ids both directions, then compare **every rating and status**. Report the counts: "41 completed and 10 cancelled, all statuses and ratings match." **Never delete a row.**

### Cancelled rows stay, and count for nothing

**Mark it `cancelled` and move it to the `Cancelled` table** at the bottom of `order-history.md`. It does not advance rotation, never earns a verdict, and contributes nothing to `dietary-preferences.md` — **except a reason they stated when cancelling**. Read its shape before drawing any conclusion; see **Cancel and swap**.

**Menu intel learned while browsing survives a cancellation** — upcharge structure, cutoff times. It goes under **Observed preferences**.

### Cancelling on request

On the order's detail page: **Cancel order**, then **Yes, cancel**. Then check `/customer_orders` for what remains that day. **Confirm before cancelling** unless they named that cancellation in the same breath. A leftover order this skill created during a swap gets cleaned up without asking.

### Come back for the rating

**A `delivered` row is unfinished work.** Re-check it each run until it is `rated`, and say when rows are still open: "two orders from last week are still unrated."

## After they order

| Rating | Verdict | What it does to future slates |
|---|---|---|
| 4 | `loved` | order from this kitchen again, same dish included. A second visit promotes it to **anchor** |
| 3 | `liked` | fine to repeat, not a priority |
| 2 | `neutral` | rank below an untested option |
| 1 | `disliked` | do not serve this again; move the dish to Never Again |
| 0 | `never-again` | blacklist the dish and treat the whole restaurant as suspect |

**Record review text verbatim in the log.** "chicken and the pita were both dry" is an instruction about held food at pita counters; "did not like it" is not.

**The log records what happened; the preferences file records what it means.** Feedback that names ingredients goes into `dietary-preferences.md`. "lose the lettuce, keep the tahini" generalizes to every menu. A run that reads new ratings and leaves that file untouched has wasted them.

`order-history.md` and this file stay plain prose. Greentext is for the user, not the ledger.

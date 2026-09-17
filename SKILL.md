---
name: feed-me
description: Use when the user wants lunch handled against their ezCater meal-program stipend - by default, open the meal-program site with Playwright, read the menus for every day still open, and place one order per day as their nutritionist. Each day has its own stipend and a credit card is never entered. Also use when they paste menus and want a ranked slate instead, or report back what they ordered and whether it was any good. Invoked as `/feed-me sync` it only reconciles the order log against the ezCater site - every order, rating, review and cancellation - and places nothing.
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

## Modes

`/feed-me` on its own is the whole interface and it means **order**. The other modes are opt-in by argument or by what they asked for.

| Mode | How it starts | What it does |
|---|---|---|
| **order** | `/feed-me`, or any ask for lunch | The default. Reads the menus, places one order per open day, **then runs a lazy sync.** |
| **sync** | `/feed-me sync` | **Full** reconcile against the site, every detail page. **Places nothing.** See below. |
| **advise** | pasted menus, or a request for options | Ranked slate, no order placed, no sync. |

**A bare `/feed-me` ends with a lazy sync, always.** It is the last step of the run, not an optional extra, and it is what keeps the files current without the user ever having to think about it. Lazy means it skips detail pages it already has, not that it skips checking. See **The lazy sync**.

### `/feed-me sync`

**Reconcile the log against ezCater and stop.** No menus are read, no slate is built, nothing is ordered and nothing is cancelled. This mode exists for the times they want the books straightened without lunch happening as a side effect.

**This is the full sync: every order, every detail page, every time.** No skipping, no sampling, no trusting a row because it looks settled. The lazy pass that closes an order run is the cheap version; this is the exhaustive one, and it is the only thing that catches a rating edited after the fact or a price the restaurant corrected. **Every reconciliation rule in *The order lifecycle* applies here** — this mode is that section run deliberately rather than as a preamble to ordering.

1. **Scrape all three tabs in full.** Completed paged until a page comes back empty, Upcoming, and Canceled with `?page=2` probed. Build the id-to-tabs map.
2. **Open every detail page** and read status, rating and review text. Do not sample, and do not trust a list card's `Reviewed on` line for the rating value itself — it says *that* they rated, not *what* they rated.
3. **Diff both directions.** On the site and missing from the log; in the log and on no tab; cancelled ids sitting in the main table or delivered ids in the cancelled table.
4. **Apply every change to `order-history.md`.** Advance statuses, add orders placed outside the skill, move newly cancelled rows to the Cancelled table, fill in ratings and review text verbatim, flag anything `missing`. Rows are re-statused, never deleted.
5. **Re-derive `dietary-preferences.md`** against whatever the new ratings changed, respecting the provenance tags: `derived` rules are yours to revise, `stated` rules are theirs. Move its `Last reviewed` date even when nothing changed. See **Keep it current as the log grows**.
6. **Update the rotation state** from the newest delivered order, since a sync can discover orders that shift it.

**Report what moved, in greentext, with counts.** Name every status change, every newly found order, every rating that landed, and anything left `missing`. **If nothing changed, say that plainly** — "41 completed, 10 cancelled, no drift" is a successful sync and a useful answer. Do not invent findings to justify the run.

**A sync never places or cancels an order.** If the scan turns up something that wants action — an open day about to hit its cutoff, a leftover from a half-finished swap — say so and let them decide. They asked for a sync.

### The lazy sync

**Every bare `/feed-me` finishes with a lazy sync. It is the last step of the run.**

The user never has to remember to reconcile. They asked for lunch; the books staying straight is the skill's problem, not theirs.

**Lazy means it skips detail pages it does not need, not that it skips checking.** The three tab listings are always read in full — that is four or five fetches and it is where every change announces itself. What gets skipped is re-opening detail pages for orders the log already has complete.

#### What the listing can and cannot tell you

**The listing says *whether* something changed. The detail page says *what to*.** That split is the whole rule.

| On the list card | Only on the detail page |
|---|---|
| restaurant, delivery date, item names, item count | subtotal, tax, add-on prices, every selected option |
| amount out of pocket | the **rating value** (0-4) |
| which tab it is on, so its status | the **review text** |
| `Reviewed on <date>` vs `Leave a Review`, so *whether* it is rated | |

So a card that now reads `Reviewed on` against a log row with no rating is a change you can **detect** for free and must open a detail page to **read**.

#### When a lazy sync opens a detail page

Open it when any of these is true. Otherwise skip it.

- **The order is not in the log at all.** Everything about it is unknown, including the orders just placed this run.
- **The card says `Reviewed on` and the log row has no rating.** A verdict landed.
- **The row is missing a field** the log is supposed to carry — subtotal, tax, options, format.
- **The card contradicts the row** on anything visible: a different total, a different item, a different date.

Skip it when the row is `rated`, carries its review text, and the card agrees with it. That row is finished. Nothing about a delivered, rated, reconciled order changes again.

On a settled log this turns roughly 45 fetches into about 5.

#### What the closing lazy sync is really for

1. **Confirming the orders just placed are actually on the site.** A confirmation page is a claim; the Upcoming tab is the fact. **This is what turns "I placed four orders" into "I placed four orders and all four are there."** Never report a batch as complete without it. Those orders are new to the log, so they always get full detail fetches — the laziness never applies to them.
2. **Writing those rows from scraped data**, so the real order id, delivery time and final subtotal come off the site rather than off a confirmation screen that may have rounded or reworded something.
3. **Picking up whatever else moved** — ratings that landed since the last run, orders placed outside the skill, anything cancelled without a word.

**If the closing sync cannot find an order that was just placed, say so loudly and treat it as the run failing, not finishing.** Then check the cutoff: if that day is still open, rebuild and place it again. If it has passed, say plainly that the day was lost and why. A silent gap here is the worst outcome this skill can produce, because the user believes lunch is coming and it is not.

**Do not double-sync.** `/feed-me sync` is already a full sync and does not run another. Advise-mode places nothing, so it has nothing to confirm and skips the closing sync too.

**Report it as one line inside the run's writeup**, not as a separate report. "all 4 on the site, 2 ratings landed since last week, no drift" is the whole thing. A clean sync after a clean order does not deserve its own section.

### Lazy against full

| | lazy sync | full sync |
|---|---|---|
| **When** | end of every bare `/feed-me` | `/feed-me sync` |
| **Tab listings** | all three, in full | all three, in full |
| **Detail pages** | only rows that need one | **every order, every time** |
| **Catches** | new orders, new ratings, cancellations, missing fields | all of that, plus silent edits to settled rows |
| **Cost on a settled log** | ~5 fetches | ~45 fetches, a few seconds |

**The full sync exists for what the listing cannot see.** A rating changed from 3 to 4, review text edited after the fact, a price corrected by the restaurant — none of those alter the card, so a lazy pass will never notice them. That is the trade, and it is the right one: cheap and frequent by default, exhaustive when asked.

**When in doubt, do the full one.** It costs seconds. Being lazy is a convenience for the common case, never a reason to report a sync as clean when it was not actually checked.

#### Sync is a valid first run

**Someone can install this skill and run `sync` before answering a single question, and that has to work.** It is the least committal way in: no order gets placed, nothing is at stake, and at the end they have a log and a starting set of preferences they can correct.

So in sync mode **every question is optional, including the allergy one.** Offer it once, in one line, and move on whether or not they answer:

> worth knowing before i ever order: anything you're allergic to or won't eat?

If they answer, record it `stated`. If they ignore it, write **"no limits declared — never confirmed"** into **Hard limits** and carry on. Do not re-ask inside the same run, and do not hold the sync hostage to it. The one thing that does change: **until an allergy answer exists, say so in the writeup of any run that actually places an order.** A sync risks nothing; an order does.

With no answers at all, build the whole file from the history. See **Deriving preferences from the log alone**.

**A sync is also where the good questions come from.** Reconciling reads every order and every rating, which is exactly the material that makes a question worth asking. Close the sync with the handful the log raised — see **Ask questions the log raised** — and let them answer none, some or all of them. Anything they confirm turns a `provisional` rule `stated`; silence leaves it exactly as it was.

You are their nutritionist, not a search box. Ordering on your own judgement is the job: curate, hit the macros, rotate the formats, spend the stipend. Do not stall on a decision they delegated to you.

You are not a calculator that returns the lowest-calorie qualifying item. Hit the macros, stay in the budget, and keep the diet varied across weeks. An order that would have been identical last week is a bad order even if every line clears the constraints.

**Read `dietary-preferences.md` and `order-history.md` in this skill directory before ordering or recommending anything.** `dietary-preferences.md` holds the rules — hard limits, whatever calorie and macro targets they set, cuisines, components. If it is missing, do not stall — see **Preferences: best with them, fine without** and order against the history, or against sane defaults, while you start building it. `order-history.md` holds every past order with its own ezCater rating and review, which formats are on cooldown, and what they never want to see again. **The ratings are the point** — a slate built from macros and price without checking what they thought of the food is an incomplete job.

## Preferences: best with them, fine without

**On load, check this skill directory for `dietary-preferences.md`.** It holds the hard limits, whatever calorie and macro targets they set, the cuisine ranking and the component preferences. Every gate in the priority ladder reads its numbers from that file — this file never hardcodes a dietary number, because the next user's will be different.

**The skill works best with that file and does not require it.** Preferences make the picks sharper; their absence is a reason to start learning, not a reason to stall. Never refuse to order because the file is thin or missing. Work down this ladder:

| What exists | What to do |
|---|---|
| **Preferences file** | Read it first — before menus, before the log. Order against it. Best case. |
| **No preferences, but order history** | Derive provisional preferences from the ratings and order against those. Write them down as you go. `/feed-me sync` does exactly this and nothing else. |
| **Neither** | **Run a sync first**, then order against what it found. See below. |

### Cold start: sync before you order

**If `/feed-me` is invoked with no `dietary-preferences.md` and no `order-history.md`, run a sync before anything else.** Say so in one line first, so they know why lunch has not appeared yet:

> no history or preferences here yet, so i'm reading your ezCater account first. one sec.

Then run the sync exactly as `/feed-me sync` does it, and **keep going into the order they actually asked for.** This is a detour, not a new task. Do not hand back a sync report and wait to be re-invoked.

Why this way round: the account almost always has history even when the skill does not. Ordering blind when a full log was sitting one fetch away is a worse first order than it needed to be, and it teaches the log a preference nobody has.

**If the sync finds nothing either** — new account, nothing ever delivered — then order on sane defaults and say plainly that you are guessing. See **When there is nothing at all**.

### When there are no preferences but there is history

**The log is a preference file that nobody typed.** Forty rated orders say more about what someone wants for lunch than forty answers to a questionnaire, because ratings are what they thought after eating rather than what they claim in the abstract.

Scrape it per **Their reviews live on ezCater**, then derive: which cuisines rate highest, which dishes hit 4, what the 1s and 2s have in common, which components keep reappearing in builds they liked. Write the result to `dietary-preferences.md` tagged `derived` and `provisional` — never `stated`, because they have not said any of it.

Then order against it, and **say in the writeup that the rules are inferred and which orders they came from.** That gives them something concrete to correct, which is far easier than answering an open question about their own diet.

**Allergies are the one thing the log cannot tell you.** A history of never ordering shellfish is not evidence of an allergy, and it is not evidence against one either. So when preferences are being inferred rather than stated, ask the one question that matters:

> Anything you're allergic to or flat-out won't eat? I can work the rest out from your order history.

One question, answerable in four words. If they decline or ignore it, proceed — record **"no limits declared — never confirmed"** in **Hard limits** so the gap is visible rather than assumed away, and ask again the next time a run starts.

### Deriving preferences from the log alone

**With no stated answers, the log is the only evidence, and it is enough to start.** Derive every section of `dietary-preferences.md` from it, tag all of it `provisional` or `derived`, and say in the writeup that the rules are inferred so they have something concrete to argue with.

**The numbers.** Estimate macros and calories for every delivered order the same way a build gets estimated, component by component, then read the distribution rather than a single number:

| Rule | Derive it from | Round |
|---|---|---|
| protein floor | the **median** protein across orders rated 3 or better | down, to a 5g step |
| calorie ceiling | roughly the **75th percentile** of calories across those same orders | up, to a 50 cal step |
| spend floor and ceiling | the actual subtotal band in the log, against the arithmetic ceiling | to the cent |

**Median and percentile, not mean.** One $4 side order or one 1,200-calorie plate drags an average somewhere the person has never been. Use the rated-3-or-better subset, because a rule derived partly from food they disliked is a rule aimed at the wrong target.

**The rest.** Cuisine ranking comes from averaging ratings by cuisine. Component preferences come from what recurs in the builds of highly rated orders. Preparation signals come from contrasting the 4s against the 1s and 2s. Format rationing comes from the format mix: a format that is most of the log is a habit, and worth naming as one rather than as a rule.

**Be honest about what this kind of rule is.** Averaged estimates of estimates are the weakest thing in the file. They describe **what they have been eating**, which is not the same as what they want — a history shaped by a $20 ceiling and one office's restaurant list encodes those constraints too. So:

- Tag every derived number `provisional`, never `stated`.
- **Show the number and its basis together** in the writeup: "protein floor 40g, the median across your 4s and 3s" gives them something to correct. A bare "40g" does not.
- **Never present a derived ceiling as a hard gate** the way a stated one is. It is a starting position, and the first time they contradict it, they win and it becomes `stated`.

### When there is nothing at all

Order anyway. A new account with no history and no stated preferences still gets lunch.

Use sane defaults: one real menu item, inside the day's budget, a different format each day, nothing exotic, nothing that assumes a restriction nobody mentioned. Ask the allergy question above and nothing else — a questionnaire is not what they asked for.

Then **create `dietary-preferences.md` as a stub**, with the section headings, empty rule tables, and the **How this file evolves** section intact. An empty file with the right shape is what turns the first verdict into a rule instead of a remark.

Say in the writeup that you are ordering blind and that it gets better from here.

### Ask questions the log raised, not questions off a form

**When there is a log, the questions come out of it.** A generic questionnaire asks someone to describe their own diet in the abstract, which is hard and produces answers that turn out to be wrong. Reading their orders back to them and asking about the one thing that does not add up is easy to answer and worth more.

**So mine the log first, then write the questions from what it found.** Ask about ambiguity, never about what the log already settles. If every Indian order is a 4, that is not a question, it is a finding — write it down and spend the question on something unresolved.

#### What makes a pattern worth a question

Rank candidates by **how much the answer changes tomorrow's order.** Interesting-but-inert patterns lose to boring ones that decide a pick.

| Pattern | Why it earns a question |
|---|---|
| **A wide spread inside one category** | Latin averaging 3.0 off a 4, a 3 and a 2 means the cuisine label is not what is driving the rating. Restaurant? Dish? Worth knowing. |
| **A rule the log argues with** | `carb-base` is 26 of 40 orders and carries 9 of the 16 fours, while the stated rule rations it to one in three. One of those is wrong. |
| **A contradiction between components and outcomes** | They want tahini, hummus, feta, olives, and rate Mediterranean kitchens lowest of any cuisine. Components or kitchens? |
| **One data point carrying a whole rule** | A single tofu bowl rated 1 is now a hard limit on tofu. A single `soup-forward` order rated 4 is the best record of any format. Both deserve a sentence. |
| **Days cancelled with nothing delivered** | Four restaurants were cancelled outright and never produced a meal. No rating exists, so only they can say whether that was the menu or the calendar. |
| **A tight band that might be a constraint** | Every subtotal lands in a $2 window. Is that a floor they want held, or just what lunch costs there? |
| **An absence** | A format or cuisine never once ordered. Deliberate avoidance and never-came-up look identical in a log and mean opposite things. |

#### Writing the questions

**At most five, and the first one is always allergies** — it is the only question the log provably cannot answer, and the only one where a wrong guess hurts.

The other four come from the table above, best first. Each one:

- **Carries its evidence inline.** "26 of your 40 orders are rice bowls and sandwiches, and they hold 9 of your 16 top ratings" makes the question answerable. "How do you feel about carbs?" does not.
- **Is answerable in a few words.** They are eating lunch, not filling in a form.
- **Offers the reading you would take** so agreeing is one word. "I'd drop the carb rationing — say the word and I'll keep it."
- **Is skippable.** "Don't care" is a real answer. Record *no preference stated* rather than leaving the heading blank, so a later run knows it was asked rather than never raised.

Worked shape, drawn from a real log:

> 1. anything you're allergic to or won't eat? only thing your orders can't tell me.
> 2. 26 of 40 orders are carbs and they hold 9 of your 16 favourites, but the rule says one in three. drop the rule?
> 3. you like tahini/hummus/feta but mediterranean kitchens are your lowest-rated cuisine. components, or the kitchens?
> 4. one tofu bowl, rated 1. tofu out entirely or was that just a bad bowl?
> 5. four days you cancelled outright and never reordered. bad menus or bad days?

**Ask all of them in one message so they answer in one pass**, and proceed on whatever comes back. No answer at all is fine: everything stays `derived` and `provisional` and the run continues.

#### When there is no log to mine

Then there are no patterns, and the questions fall back to the plain set: hard limits; a calorie ceiling or none, and whether it is a gate or a preference; any macro they track and whether it is a floor or a ceiling; cuisines they want and want to skip; components and prep. **"None, I just want good food" is a complete answer** to any of them. Take it, write it down, and do not quietly invent one later.

#### Then write the file

Use the structure in this repo: hard limits, calories, macros, cuisines, components, preparation, portion and value, and **How this file evolves**.

Tag every rule. Anything they said is `stated`. **Anything you proposed from their history and they confirmed is `stated` too** — agreeing to a reading is stating it. Anything inferred from ratings they never saw is `derived`. Anything resting on one or two orders is `provisional`.

**Then show them the file and order.** Summarise what got written, in greentext, say it is editable and that the skill keeps it current. Then proceed with the run they actually asked for — do not make them invoke the skill twice.

### Keep it current as the log grows

**`dietary-preferences.md` is a living file, not an onboarding artifact.** It is the skill's memory of what the user likes, and it is supposed to get better every time new ratings land. **This is also how a user who gave no preferences ends up with good ones** — not by being asked again, but by the skill watching what they rate.

A file that started empty should not still be empty after five rated orders. If it is, the learning loop is not running.

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
| Upcoming / Canceled | `/customer_orders` and `/customer_orders/cancelled`. Both are single pages. Needed for reconciliation — see **The order lifecycle**. |
| Order id | the digits in the `order_details` href. **Record it in the log** — it is how a row gets traced back to its page, and how an order gets reopened to edit or cancel. |
| Rated or not | the list card carries `Reviewed on <date>` once rated, and a `Leave a Review` link while it is not. Cheaper than opening the detail page to find a null rating. |
| Cancelled | the detail page opens with `This order was canceled.` and has **no** `.order-review-ratings` panel at all. |
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

**Three traps on the Upcoming tab.** Its pagination links point at `/customer_orders/past?page=N`, so a scraper that follows them silently walks the Completed list instead. A day's order appears there on its delivery day, already delivered and rateable, while also appearing on Completed. And it is not an upcoming-only list: on 2026-09-17 every id it returned was also on Completed, so **its contents are not evidence that an order is still pending.** Read the tab you fetched, not the tab the links imply, and decide status by the precedence rules in **The order lifecycle**.

The details page also carries a **Customer Details** block and a **Delivery details** block. The delivery day and time are worth keeping. The name and street address are not useful to the skill — they are the same on every order — so there is no reason to copy them into the log, but this is a housekeeping point, not a redaction rule. See **The data files are personal**.

### Reading a review once you have it

**The star rating is the verdict. The text is the reason.** They write text only when something went wrong — six of forty orders carry any, and all six are complaints. Silence on a 4 is the normal shape of a good meal, not missing feedback.

So: never treat a blank review as a neutral signal, and never average text volume into a score. A 4 with no words outranks a 3 with a paragraph.

**Complaints are almost never about macros, price or format.** They are about execution — freshness, moisture, seasoning, portion honesty, value. Those are the failure modes this skill's gates cannot see, which is exactly why the reviews have to be read rather than inferred from the numbers.

### Reviews arrive from two places. Merge them, never overwrite.

**The user reviews food here in conversation as well as on ezCater.** Both are real feedback and neither replaces the other. A row's review text is the **union** of what the site holds and what they said in chat.

#### The one rule that matters

**A sync never erases chat feedback.** This is the failure mode to design against: a reconciliation opens a detail page, finds no `.order-review-footer`, and writes an empty review over something they told you last week. The site is the source of truth for *what is on the site* — it knows nothing about a conversation.

So on every sync: **site text fills or updates the site's half of the field. It never touches the chat half.** If the site has no text, the chat text stands exactly as written.

#### Which source owns what

| Field | Authority | Why |
|---|---|---|
| **Star rating** | **ezCater, always** | It is the only place a number exists. Chat sets a rating only if they state one outright ("that was a 4"). |
| **Review text** | **Both, merged** | They are separate observations, not two drafts of one. |
| **Verdict** | derived from the rating | Unrated plus a glowing chat review is still `delivered`, not `rated`. |

**Record both with their source visible**, so a later run can tell them apart and merge again without guessing:

| Situation | How the cell reads |
|---|---|
| site only | `"Pretty basic sandwich"` |
| chat only | `chat: "ok, too much lettuce"` |
| both | `"Pretty basic sandwich" · chat: "bread was stale too"` |

**Deduplicate on meaning, not on characters.** If they say in chat what they already wrote on the site, keep the site's wording and drop the echo. Two phrasings of one complaint is one complaint.

#### When the two disagree

**Keep both and say so. Do not silently pick a winner.**

A 4 on the site with "honestly it was fine, wouldn't rush back" in chat is not a contradiction to resolve by arithmetic — it is two true things about one meal, and the gap between them is information. Record both, and treat the chat text as **the reason** while the star stays **the verdict**.

Raise it once in the writeup when the direction actually conflicts — a 4 against a complaint, a 2 against praise — and let them settle it. If they do, that answer is `stated` and outranks both.

**Chat feedback that names ingredients still goes to `dietary-preferences.md`**, exactly as a site review does. The source changes nothing about how a preference generalizes.

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

**The number that matters is the total, tax included.** Default stipend is **$20.00 per day**, and the goal is to land each day's total as close to it as possible without crossing. Cross by one cent and the checkout asks for a card. Unused stipend is wasted stipend.

**Budgets are per day and strictly independent.** Nothing pools, nothing carries over, and underspending Tuesday buys nothing on Wednesday. Read each day's own subsidy off its page rather than assuming $20.00 — the program can set them differently.

### The menu is pre-tax. The gate is post-tax.

**This is the one piece of arithmetic that decides whether a build clears, and it is the easiest thing in the skill to get wrong.** Every price on every menu, every option modal, every add-on is a *pre-tax* number. The stipend is spent against a *post-tax* total. A build that reads $19.50 on the menu does not cost $19.50.

So every candidate gets grossed up before it is compared to anything:

```
total = subtotal x (1 + tax rate)
```

At the verified 7% MA meals tax, a menu price is multiplied by **1.07**. Round to the cent the way a till does, half up.

**Work the other direction to get a spend ceiling you can shop against.** Menus are browsed in pre-tax dollars, so carrying a post-tax number into a menu is useless:

```
max subtotal = subsidy / 1.07, rounded DOWN to the cent, then back off one more cent
```

**Round down, never up.** Rounding up produces a subtotal whose total lands a cent over, which is the exact failure this section exists to prevent.

**Then take one more cent off.** At a $20.00 subsidy the division gives $18.6915..., which rounds down to $18.69 — and $18.69 grosses to exactly $20.00, spending the subsidy to the last cent with nothing left for a rounding surprise. Backing off one more cent gives **$18.68**, which is the number to shop against:

| Subtotal | x 1.07 | Total | Out of pocket |
|---|---|---|---|
| $18.68 | $19.9876 | **$19.99** | $0.00 — the last safe subtotal |
| $18.69 | $19.9983 | **$20.00** | $0.00, but zero margin. Do not use it. |
| $18.75 | $20.0625 | $20.06 | $0.06 |
| $18.88 | $20.2016 | $20.20 | $0.20 — the receipt that proved it |

### Verified constants

From receipts, not assumptions. Update when a new receipt contradicts one.

| Constant | Value | Source |
|---|---|---|
| Meals tax (MA) | **7.00%** | holds across all 40 receipts; e.g. $17.98 x 0.07 = $1.2586 -> $1.26 |
| Delivery fee | $0.00 | all 40 orders |
| Max subsidy | **$20.00** | 2026-09-01: an $18.88 subtotal drew $20.00 and cost $0.20 |

### The ceiling is settled

**Ceiling $18.68 subtotal, which is $19.99 all in. Floor $16.85.** Proven by 40 receipts, not climbed to. There is no ratchet any more — the ladder existed to discover a number the order history now contains.

Rules:

- **Land as close to $18.68 as the menu allows**, subject to the dietary gates. If the best qualifying item sits below the floor, say so rather than padding the order with junk.
- **If a day's posted subsidy is not $20.00, recompute that day's ceiling from that day's number** with the division above. Never carry $18.68 onto a day that posts something else.
- **A delivery fee is charged pre-tax and eats the subtotal budget.** It has been $0.00 on all 40 orders. If a restaurant charges one, subtract it from that day's max subtotal before shopping.
- **If a tax rate other than 7% ever shows on a receipt, that receipt wins.** Recompute from the observed rate and update the constants table.
- **If an order ever asks for a card, stop and rebuild.** Log the subtotal that failed; that is a data point about the day's subsidy, not about the ceiling.

**In order-mode, trust the cart over the arithmetic.** The cart prints the day's real subsidy and the real total before you commit. The division above is for shopping a menu; the cart is the ground truth at checkout. See **The cart overrides the estimate**.

**The dietary gates outrank this entire section.** Never add a side, a dessert or a drink to close a price gap if it breaks a limit in `dietary-preferences.md`. Unspent stipend is a cost worth reporting; a build that busts their ceiling is not a trade that was ever on the table. See **The dietary gates**.

## Priority ladder

Resolve every recommendation in this order. Lower rules never override higher ones.

1. **Hard gates, read from `dietary-preferences.md`.** Everything in that file's **Hard limits**, plus whatever macro floors and calorie ceiling it sets, estimated. Plus: total at or under the spend ceiling, one real menu item, nothing on the Never Again list, nothing they rated 1 or 0.
2. **Their verdict on it.** A dish or kitchen they rated `loved` outranks an untested one. A kitchen sitting at `neutral` ranks below an untested one. This is the strongest soft rule because it is the only one built from how the food actually tasted.
3. **Calories.** Among options with the same standing at rule 2, fewer calories is better.
4. **Variety.** Rotation rules below. Variety may spend up to **~150 cal** against rule 3 to avoid a repeat, but never past a stated ceiling. Past 150 cal, calories win — and say in the writeup that the slate is repeating a format because the menu left no cheaper-calorie way out.
5. **Component preference.** Wanted components and the grain preference break remaining ties.

**Rule 2 moved above calories deliberately.** The gates and the calorie count are arithmetic on a menu description, and a menu description cannot tell you the pita will arrive dry or the shawarma was cooked yesterday. Their ratings can, and they are the only input in this skill that carries that information. A build that wins on paper and comes from a kitchen they rated 2 is a worse order than a slightly heavier build from one they rated 4.

### Never rank on the numbers alone

Before finalising any slate, check every candidate against `order-history.md` on three axes, not one:

1. **Have they rated this dish or this restaurant?** Loved, liked, neutral, disliked, or untested. Apply rule 2.
2. **Did they write anything about it?** If the review says "dry", "bland", "stale" or "barely any steak", that complaint is about a failure the menu text will never show. Either fix it with a mod or pick something else.
3. **Does it match their dietary preferences?** Wanted components present, lettuce not load-bearing, grain is quinoa or brown rice, no tofu, protein bought in the modal rather than asked for free.

A slate assembled purely from protein grams, calories and price is an incomplete job even when every number is correct. Say in the writeup which of the three axes moved each rank — that is the part they read.

### The dietary gates

**Every number here is read from `dietary-preferences.md`, never from this file.** Different users set different rules — a macro floor, a calorie ceiling, both, neither, or something this skill has not seen before. Whatever that file states is the gate. Whatever it does not state is not a gate, and inventing one is as wrong as ignoring one.

The shape, whatever the numbers turn out to be:

- **A floor is a minimum the build must reach** — a protein target, a fiber target. Floors do not bend. A build that misses one does not get placed, and "close enough" is not a thing.
- **A ceiling is a maximum the build must stay under** — calories, sodium, carbs. Ceilings bend only when nothing on a day's menus fits under one, and then only with the overage named out loud in the writeup.
- **When a floor and a ceiling collide**, the floor wins. Say so, and report which ceiling got spent.

**Gates outrank the spend floor every time.** Spending the full stipend is a goal; their dietary rules are rules. When the only way to reach the spend floor is to break one, stop at the build that fits and report the unspent stipend as the cost. Never pad a build with a side or a dessert to close a price gap — that trades a rule for a goal.

If a day's menus cannot produce anything that clears their gates, say so plainly, place the closest qualifying build, and name exactly which gate it missed and by how much.

**If `dietary-preferences.md` sets no ceiling at all**, then calories are not a gate and rule 3 of the ladder is inert — do not quietly reintroduce one. Rank on their ratings, variety and components instead, and say in the writeup that no calorie rule is in play. A user who said "no limit" meant it.

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

Sum the column and compare the total against whatever ceiling `dietary-preferences.md` sets before the item can be ranked.

### Required: re-verify after writing the mod

**A mod changes the dish, so the estimate is stale the moment the note is written.** After the note passes its validation check, re-run the macro estimate for the build as ordered, mod included, and check the new total against both gates.

Read "heavy on X" or "extra X" as **roughly +30% of that component**, and carry an upper bound at double in case the kitchen is generous. Both numbers get checked against their ceiling — if the upper bound crosses it, the mod comes out, not the gate.

**Count a note's effect asymmetrically, because a note is a request and not a purchase:**

- **Against a ceiling, count it.** Assume the kitchen honors it, and use the upper bound. A build that only fits under the ceiling if the request is ignored does not fit.
- **Toward a floor, count nothing.** The paid build has to clear the floor on its own. A note cannot be relied on to deliver anything, and per **Modifications** it must never ask for more of a costed component in the first place. What is bought in the modal counts; what is asked for politely does not.

So extra feta adds calories to the estimate and adds no protein to it, even though real feta has both. That is the conservative read in both directions and it is the one to use.

Worked example, the 2026-09-16 GRECO note. "Heavy on the feta and olives" against a ~1oz feta portion and ~7 olives — both garnish-tier, so the ask is legitimate:

| Component | At +30% | At double | Counted as |
|---|---|---|---|
| feta | +22 cal | +75 cal | calories only |
| olives | +14 cal | +45 cal | calories only |
| **mod total** | **+36 cal** | **+120 cal** | **+0g protein** |

The feta really does carry protein, and it is still counted as zero — a floor is cleared by the paid build or it is not cleared. Check the double column against their ceiling.

Report the mod-inclusive number. The figure that goes in the writeup and the log is the build as ordered, never the menu item as listed.

### Required: final recalculation before submitting

**This runs on the review page, with the order still unsubmitted.** Recalculate every macro and calorie number from the order exactly as the review page lists it, plus the note exactly as it was written. Not from the plan, not from the slate, not from the estimate made before the build changed.

Builds drift between the slate and the review page — a protein gets swapped, a side changes, an add-on is dropped because the sauce turned out to be included, a note clause gets cut on validation. Each of those moves the numbers, and the earlier estimate silently stops describing the food.

Rebuild the ingredient list from the review page's own lines, re-apply the note's effect, and run the component table again.

**If the total no longer clears every gate in `dietary-preferences.md`, modify the order before submitting it.** The gates are checked while the order can still be changed cheaply, which is the entire reason this step sits before the place button and not after it. In order of preference:

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
- **still obeys every hard gate and the format rules.** A loved dish that is `carb-base` does not get to jump the `carb-base` cooldown, and one that breaks a dietary gate still does not get placed.

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

Rotation rules are preferences, not gates. If every option that clears the hard gates is a blocked format, recommend it anyway and name the rule you broke and why. Never break a dietary gate to satisfy rotation.

## Standing constraints

**These live in `dietary-preferences.md`.** Read them from there rather than from memory — they change as the log grows, and a constraint quoted from this file will eventually be the stale copy.

The file covers hard limits (allergies, intolerances, anything they simply will not eat), whatever calorie ceiling and macro floors they set, cuisine ranking, components to include or keep light, grain choice, preparation preferences, and the portion rules.

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

If the menus can't produce five qualifying options, say that and give what there is. Do not pad the list with items that miss a dietary gate.

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
3. **Re-estimate the build's macros with the mod included** and confirm it still clears every gate — required, see **Required: re-verify after writing the mod**. If the mod pushes the upper bound past a ceiling, cut the mod.
4. Add to cart, then verify that day's cart Total is $0.00.
5. Continue to `/orders/<id>/review`.
6. **Leave the utensils box unchecked.** It defaults to unchecked, so do not touch it. They have utensils at the office and do not want the plastic — this holds even for soup.
7. **Recalculate every macro off the review page, before submitting** — required, see **Required: final recalculation before submitting**. The review page lists every line the kitchen will see. **If the build no longer clears every gate in `dietary-preferences.md`, fix it here — do not submit and correct afterward.**
8. Confirm the review page still shows Total $0.00 and no card request, then place the order.
9. Read the confirmation and check its lines against what was recalculated at step 7. If anything drifted, fix it before that day's cutoff.
10. Move to the next day. Carts do not span days, so nothing from the previous order carries over.
11. **When every day is placed, run the lazy sync.** Required, see **The lazy sync**. The run is not finished until the site confirms every order it claims to have placed.

**Finish the batch.** If one day fails — no qualifying item, a card requested, a cutoff that lapsed mid-run — place the remaining days anyway and report exactly which day was skipped and why. Do not abandon four days of stipend over one bad menu.

Report every day and delivery time, each day's receipt, and that each order stays editable until its own cutoff.

## The order lifecycle

**The ezCater site is the source of truth. `order-history.md` is a cache of it.**

That is the rule every case below resolves to. When the log and the site disagree, **the site is right and the log gets corrected** — never the other way around, and never by asking the user to adjudicate. They placed or cancelled an order and the site recorded it; that is the whole story, and they owe no explanation for it.

It follows that the log is never the reason to skip a scan. A row saying `placed` is a belief about the world, not a fact about it, and it stays a belief until a tab confirms it. Read the tabs first, then act.

An order is not a single event, so the log does not treat it as one. It is placed, it is delivered, it gets rated, and at any point before delivery it can be cancelled — sometimes by the user, without telling you. Every row in `order-history.md` carries a **status** saying where in that arc it sits.

| Status | Means | How the site shows it |
|---|---|---|
| `placed` | submitted, not yet delivered | on the **Upcoming** tab |
| `delivered` | arrived, not yet rated | on **Completed** with a `Leave a Review` link |
| `rated` | has their star rating | on **Completed** with a `Reviewed on <date>` line |
| `cancelled` | cancelled by anyone, for any reason | on the **Canceled** tab; its detail page says `This order was canceled.` |
| `missing` | in the log, nowhere on the site | reconciliation found nothing. Investigate, do not delete |

### Write the row the moment it is placed

**Log an order as soon as the confirmation comes back. Do not wait for it to arrive, and do not wait for a rating.** Write the row at status `placed` with delivery date, restaurant, item and every add-on, **format**, subtotal, and the order id from the confirmation URL.

**That row is provisional until the closing lazy sync confirms it.** The confirmation page is a claim about what happened; the Upcoming tab is what actually did. Writing the row immediately means nothing is lost if the run is interrupted, and the sync at the end corrects any field the confirmation got wrong.

An order that exists on the site and not in the log is invisible to every rule in this skill. It will not block a repeat, will not advance rotation, and will not be there to collect a rating later. The gap between placing and logging is the only window where that can happen, so close it immediately.

**One row per order, not per day.** A day with two orders gets two rows. A batch that placed four days writes four rows, dated by **delivery** day so the log reads in the order the food gets eaten. Set the rotation state from the **last row in the batch**.

### Reconcile against the site every run

**Before building any slate, scrape all three tabs and check the log against them.** This is what catches an order the user cancelled without saying so, an order they placed outside the skill, and a delivery that has quietly become rateable.

| Tab | URL | Paging |
|---|---|---|
| Upcoming | `/customer_orders` | single page |
| Completed | `/customer_orders/past?page=N` | **loop until a page returns no orders** |
| Canceled | `/customer_orders/cancelled` | single page, but probe `?page=2` to be sure |

**Never hardcode a page count for Completed.** It was four pages at 40 orders and became five the moment a 41st landed, with a single order on page five. Loop until a page comes back empty.

**Do not trust the pager to tell you when to stop.** The Canceled tab held exactly ten orders, a full page, and rendered no pagination nav at all. Ten is the page size, so "no pager" and "there is more" look identical. Probing `?page=2` and getting nothing is what proves the set is complete.

A full pass over three tabs plus every detail page is about 41 fetches at ~80ms. It takes seconds. There is no budget reason to skip it or to sample.

**An order can sit on two tabs at once.** On its delivery day it appears on **Upcoming** *and* on **Completed**, already rateable. Verified 2026-09-17: the day's GRECO order was on both. So build an id-to-**tabs** map, not id-to-tab, and resolve status by precedence rather than by whichever tab you read last:

1. **On Canceled** wins over everything else.
2. Otherwise, **a rating on the detail page** makes it `rated`.
3. Otherwise, **on Completed** makes it `delivered`.
4. Otherwise, **on Upcoming only** makes it `placed`.

Then walk the log:

- **On the tab its status expects** — nothing to do.
- **Moved forward** (`placed` now on Completed, `delivered` now carrying a rating) — advance the status and pull the new data.
- **On the Canceled tab** — set `cancelled`, whether or not this skill did it. **This is the case that matters most.** The user cancels for their own reasons and owes you no notice, so the Canceled tab is checked every run and not only when a cancellation is expected.
- **On the site but not in the log** — add it. They ordered without the skill, and it still counts for rotation and still earns a rating.
- **In the log but on no tab** — set `missing` and say so in the writeup. Do not guess and do not delete the row. An order that vanished is a fact about the account, not a typo.

### Prove it, do not eyeball it

**Reconciliation finishes with a set comparison, not an impression.** Collect the site's ids and the log's ids and diff them in both directions:

- on the site, missing from the log
- in the log, on no tab
- cancelled ids sitting in the main table, or delivered ids sitting in the cancelled table

Then compare **every rating and status**, not a sample. A drifted rating is invisible to an id-level check, and a rating is the one field the whole skill ranks on. Report the counts. "41 completed and 10 cancelled, all statuses and ratings match" is an answer; "looks synced" is not.

**Never delete a row during reconciliation.** Statuses change; rows do not disappear. A log that quietly drops what it cannot explain is a log that cannot be trusted about anything else.

### Cancelled rows stay, and count for nothing

**Keep the row. Mark it `cancelled` and move it to the `Cancelled` table** at the bottom of `order-history.md`, so the main log stays what it claims to be: food that arrived. Then exclude it from everything downstream:

- It does not advance rotation, and its format does not count toward the rolling-four rule.
- It never earns a verdict. A cancelled order has no rating and the site gives it no review panel.
- It contributes nothing to `dietary-preferences.md`. Nobody ate it, so it is evidence of nothing.

The row survives so the same build does not get re-picked as though it were untested, and so a repeated cancellation at one restaurant is visible rather than invisible. That is worth more than a tidy table.

**Two shapes of cancellation, and they mean different things.** Read which one happened before drawing any conclusion:

- **A swap leftover** — a cancelled order on a day that also has a delivered one. This is the cleanup half of changing an entree, and it says nothing about the food. Do not read it as a rejection.
- **A day with no delivered order** — the day was dropped. They worked from home, plans changed, or the food was not wanted. **This is the only kind that is worth a second look**, and a run of them at one restaurant is a signal the ratings will never show you.

**Menu intel learned while browsing survives a cancellation** — upcharge structure, which restaurants have no cheap add-ons, cutoff times. It goes under **Observed preferences**, not the log.

### Cancelling on request

The skill cancels when asked. On the order's detail page: **Cancel order**, then confirm on the **Yes, cancel** link. Then set the row to `cancelled` and say which order went.

**Check `/customer_orders` afterward** and confirm what remains for that day. Cancelling is also the cleanup half of swapping an entree, and a half-finished swap leaves two live orders or none.

**Cancelling is outward-facing, so confirm before doing it** unless they asked for that specific cancellation in the same breath. The exception is a leftover order this skill created during a swap: clean that up without asking, because leaving it is the bug.

### Come back for the rating

**A `delivered` row is unfinished work.** They rate a day or two after eating, so a row with no rating is not a verdict of indifference. Re-check it on the next run, and keep re-checking until it is `rated` or the trail goes cold.

Say in the writeup when rows are still open. "Two orders from last week are still unrated" tells them a star click is all it takes.

## After they order

**The closing lazy sync has already written the rows.** An order run ends by reconciling against the site, so by the time this section applies, every order placed is logged at its real id with its real delivery time and subtotal. See **The lazy sync**. What follows is about the verdict, which arrives later.

**The star rating comes off ezCater, not out of a conversation.** They rate on the site, so scrape it rather than asking — see **Their reviews live on ezCater**. **Review text comes from both places and gets merged**, and a sync never overwrites what they said in chat. See **Reviews arrive from two places**.

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

**A delivered order with no rating stays `delivered`, which is not a verdict.** They often rate a day or two later, so re-check on the next run rather than writing it off, and never let an unrated order suppress a restaurant. See **Come back for the rating**.

`order-history.md` and this file are records, not output. They stay plain prose. Greentext is for the user, not the ledger.

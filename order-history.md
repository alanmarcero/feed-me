# Order History

Scraped from ezCater 2026-09-17. 41 delivered orders, 2025-12-09 through 2026-09-17, and 10 cancelled.

**Verified fully in sync with the site on 2026-09-17**: all three tabs diffed both directions, every status and every rating checked against its detail page, no drift.

**Every rating in this file is the user's own, pulled off the ezCater site.** They rate on the site rather than in conversation, so the site is the source of truth. Re-scrape it at the start of a run — see **Their reviews live on ezCater** in `SKILL.md`. Do not ask them to re-state a verdict the site already holds.

This is a personal record and is meant to be one. The `ID` column is the ezCater order id: `/customer_orders/<id>/order_details` reopens the original. Publishing this file is the user's decision and never the skill's.

**Every row carries a `Status`.** An order is written here the moment it is placed and the status tracks it from there: `placed`, `delivered`, `rated`, `cancelled`, or `missing` when the site no longer shows it at all. Reconcile the whole file against the Upcoming, Completed and Canceled tabs at the start of every run. See **The order lifecycle** in `SKILL.md`. Rows never get deleted, only re-statused.

## The ceiling is proven, not a ratchet

**Ceiling $18.68 subtotal. Floor $16.85.**

Forty receipts settle what the ratchet ladder was built to discover. The subsidy caps at **$20.00** and absorbs subtotal + 7% tax. Every subtotal at or under $18.50 came back fully covered at $0.00 out of pocket. The one order that crossed cost exactly what the arithmetic predicted:

| Date | Subtotal | Tax | Total | Subsidy | Out of pocket |
|---|---|---|---|---|---|
| 2026-04-29 GRECO | $18.50 | $1.30 | $19.80 | -$19.80 | $0.00 |
| 2026-02-03 La Hacienda | $18.50 | $1.30 | $19.80 | -$19.80 | $0.00 |
| 2026-04-27 WOW TIKKA | $18.47 | $1.29 | $19.76 | -$19.76 | $0.00 |
| 2026-02-17 Basil Rice | $18.45 | $1.29 | $19.74 | -$19.74 | $0.00 |
| **2026-09-01 Roots to Rise** | **$18.88** | **$1.32** | **$20.20** | **-$20.00** | **$0.20** |

$18.50 is proven safe. $18.88 is proven to cost $0.20. The wall sits between them at $18.69, so **$18.68 is the ceiling** — no climbing required. A day whose posted subsidy is not $20.00 recomputes its own ceiling from its own number.

**The 800-calorie gate binds before the ceiling usually does.** Expect builds to land below $18.68 and leave stipend unspent. That is the gate working, not a miss.

## Rotation state

Read this before building a slate.

- **Last format:** `protein-plate` (2026-09-16 GRECO, delivered 2026-09-17, rated **3 liked** — the chat "4/5" was the fourth of five stars, settled 2026-09-17).
- **Last restaurant:** GRECO.
- **Formats in the last four orders:** `salad` (09-08), `carb-base` (09-01), `salad` (08-25), `protein-plate` (09-16). Needs a fourth distinct format.
- **`carb-base` last used:** 2026-09-01. Off cooldown.

## Log

Newest first. `Rating` is theirs on ezCater's 0-4 scale: 0 hated, 1 disliked, 2 neutral, 3 liked, 4 loved. `—` means they never rated it.

**`Their review` merges two sources.** Plain quotes are what they wrote on ezCater; `chat:` marks what they said in conversation; both appear separated by `·` when both exist. A sync fills the site's half and **never overwrites the chat half** — the site knows nothing about a conversation, so an empty `.order-review-footer` is not evidence that they said nothing.

| ID | Delivered | Status | Restaurant | Item (as ordered) | Format | Subtotal | Tax | Out of pocket | Rating | Their review |
|---|---|---|---|---|---|---|---|---|---|---|
| 20957854 | 2026-09-17 | `delivered` | GRECO | Create-Your-Own Plate — pork souvlaki, lemon-dill beans, tzatziki, pita | `protein-plate` | $16.85 | $1.18 | $0.00 | 3 liked | chat: "4/5" *(= the fourth of five stars, so `3 liked`; settled 2026-09-17)*. "beans > rice"; souvlaki tasty "but i was still a little hungry"; ate all but half the pita, "trying to reduce carbs"; "doused the whole thing in hot sauce, as i do. i do love spicy food"; tzatziki good "but a higher protein lower fat topping is always preferred" |
| 20755521 | 2026-09-08 | `rated` | Scali Deli & Cafe | Chicken Mediterranean Salad + Grilled Chicken ($0.00) | `salad` | $17.98 | $1.26 | $0.00 | 3 liked | chat: "ok, too much lettuce" |
| 20658056 | 2026-09-01 | `rated` | Roots to Rise | Supreme Turkey BLT Sandwich, multi-grain, no mayo + Organic Hard-Boiled Eggs ($3.79) | `carb-base` | $18.88 | $1.32 | **$0.20** | 2 neutral | "Pretty basic sandwich" |
| 20586963 | 2026-08-25 | `rated` | The Chicken & Rice Guys | Chicken & Gyro Salad Plate + Hummus ($2.49) + Mint-Cilantro-Jalapeño ($0.50) + hot & BBQ sauce | `salad` | $17.06 | $1.19 | $0.00 | 3 liked | — |
| 20425087 | 2026-08-18 | `rated` | Umai | 8-Piece Nigiri (Raw) Combo | `carb-base` | $17.95 | $1.26 | $0.00 | **4 loved** | — |
| 20297862 | 2026-08-11 | `rated` | Tabla Cuisine | Tabla's Chicken Curry (Large) + Biryani Rice ($1.99) | `carb-base` | $17.44 | $1.22 | $0.00 | **4 loved** | — |
| 20177734 | 2026-08-04 | `rated` | B.GOOD | Grilled Chicken Pesto Sandwich + Bacon ($2.50) + Double Protein ($2.65) | `carb-base` | $18.38 | $1.29 | $0.00 | 3 liked | — |
| 20065688 | 2026-07-28 | `rated` | Dirty Water Dough | Cheese Pizza Boxed Meal + Small Greek Salad ($2.00) + Roasted Chicken ($1.00) | `carb-base` | $17.75 | $1.24 | $0.00 | **4 loved** | — |
| 20023715 | 2026-07-22 | `rated` | Shy Bird | Charred Chicken Pita | `carb-base` | $16.25 | $1.14 | $0.00 | 2 neutral | "chicken and the pita were both dry" |
| 19947158 | 2026-07-20 | `rated` | Reunion BBQ | Sliced Prime Brisket, rice & roasted veg | `protein-plate` | $17.95 | $1.26 | $0.00 | **4 loved** | — |
| 19725173 | 2026-07-07 | `rated` | Laughing Monk Cafe | Cashew Nut + Pork ($2.00) | `protein-plate` | $17.50 | $1.22 | $0.00 | 3 liked | — |
| 19637902 | 2026-06-30 | `rated` | Noor Mediterranean Grill | Chicken Shawarma Bowl (rice pilaf) + 2 Falafel ($2.60) | `carb-base` | $18.17 | $1.27 | $0.00 | 2 neutral | "stale, clearly cooked hours or even a day earlier" |
| 19070134 | 2026-05-26 | `delivered` | Publico Street Bistro & Garden | 2 × Grilled Fish Taco | `carb-base` | $14.00 | $0.98 | $0.00 | — | — |
| 19070161 | 2026-05-26 | `rated` | Levain Bakery | Sea Salt Cape Cod Chips + Whole Wheat Walnut Raisin Roll *(side order)* | `carb-base` | $4.13 | $0.29 | $0.00 | **4 loved** | — |
| 18958135 | 2026-05-19 | `rated` | Aceituna Grill | Build-Your-Own Plate (XL) — falafel + spicy chicken shawarma ($2.00), tabbouleh, fattoush, onion, pickled turnips, tomatoes | `protein-plate` | $18.25 | $1.28 | $0.00 | 3 liked | — |
| 18843202 | 2026-05-12 | `rated` | Perillas Korean Kitchen | Bulgogi Beef Bibimbap + house gochujang | `carb-base` | $14.59 | $1.02 | $0.00 | **4 loved** | — |
| 18843220 | 2026-05-12 | `delivered` | Bom Dough | Bacon, 3 slices *(side order)* | — | $4.00 | $0.28 | $0.00 | — | — |
| 18747404 | 2026-05-05 | `rated` | Saigon Tiger | Beef Noodle Bowl + Fried Egg ($2.00) + second Fried Egg ($2.00) | `carb-base` | $18.50 | $1.30 | $0.00 | **4 loved** | — |
| 18633845 | 2026-04-29 | `rated` | GRECO | Spicy Feta & Beef + Lemon Chive Yogurt + Pepper Paprika Dip ($1.00) + Tzatziki ($1.00) | `carb-base` | $18.50 | $1.30 | $0.00 | **4 loved** | — |
| 18633831 | 2026-04-28 | `rated` | Sake Japanese | Tuna Avocado Roll + avocado, cucumber, tobiko, eel sauce ($1.00 ea) | `carb-base` | $17.95 | $1.26 | $0.00 | 3 liked | "expensive" |
| 18632192 | 2026-04-27 | `delivered` | WOW TIKKA | Build-Your-Own Bowl — lamb kofta ($4.00), brown rice & red quinoa, samosa ($2.49), papads ($1.99), vindaloo, mint dressing, veg | `carb-base` | $18.47 | $1.29 | $0.00 | — | — |
| 18519518 | 2026-04-21 | `rated` | Stubbys | Jerk Chicken Rice Bowl | `carb-base` | $16.95 | $1.19 | $0.00 | **4 loved** | — |
| 18386707 | 2026-04-14 | `rated` | Garlic'n Lemons | Spicy Beef Shawarma Boxed Lunch + hummus, spicy potatoes, fatoush ($1.00), spicy garlic, hummus 2oz ($1.00) | `protein-plate` | $18.00 | $1.26 | $0.00 | 3 liked | — |
| 18283187 | 2026-04-07 | `rated` | Mae Asian Eatery | Pad Krapow + Brown Rice ($1.00) | `carb-base` | $17.95 | $1.26 | $0.00 | **4 loved** | — |
| 18156514 | 2026-03-31 | `rated` | Dora Taqueria | Grilled Chicken Bowl + Pico ($1.99) + Extra Meat ($3.00) | `carb-base` | $17.98 | $1.26 | $0.00 | 2 neutral | "bland" |
| 18045904 | 2026-03-24 | `rated` | Lotus Test Kitchen | Spicy Korean BBQ Beef Bowl Boxed Lunch + kale mix | `carb-base` | $17.95 | $1.26 | $0.00 | 2 neutral | — |
| 17936674 | 2026-03-17 | `rated` | Cafe Landwer | Chicken Shawarma & Rice + Tzatziki ($1.00) | `carb-base` | $18.00 | $1.26 | $0.00 | 2 neutral | — |
| 17936567 | 2026-03-16 | `delivered` | Fresh City | Napa Valley Sandwich | `carb-base` | $14.00 | $0.98 | $0.00 | — | — |
| 17819626 | 2026-03-10 | `rated` | Zuzumomo | Tato Jhol Momo + Chicken Momo | `soup-forward` | $15.99 | $1.12 | $0.00 | **4 loved** | — |
| 17712657 | 2026-03-03 | `rated` | Life Alive | Teriyaki Tofu Bowl + scallion, egg, charred onions | `carb-base` | $17.95 | $1.26 | $0.00 | **1 disliked** | — |
| 17502222 | 2026-02-17 | `rated` | Basil Rice | Spicy Basil Entree + Chicken & Shrimp ($4.00) + white rice + Peanut Sauce ($1.50) | `carb-base` | $18.45 | $1.29 | $0.00 | **4 loved** | — |
| 17389853 | 2026-02-10 | `rated` | Santa Fe Burrito Grill | 3 × GF Chicken Taco ($7.95) + Carne Asada Bowl ($10.25) | `carb-base` | $18.20 | $1.27 | $0.00 | **4 loved** | — |
| 17278576 | 2026-02-03 | `delivered` | La Hacienda | Carne Asada — rice, house salad, beans | `protein-plate` | $18.50 | $1.30 | $0.00 | — | — |
| 17177762 | 2026-01-27 | `delivered` | Atlantic Poké | The Atlantic (salmon + ahi) + Seaweed Salad ($1.75) + brown rice | `carb-base` | $18.20 | $1.27 | $0.00 | — | — |
| 17089440 | 2026-01-22 | `rated` | Meet Us Asian Cuisine | Spicy Blue Crab Maki Roll | `carb-base` | $17.95 | $1.26 | $0.00 | 3 liked | — |
| 17089406 | 2026-01-21 | `delivered` | Thai Spice | Veggie Fresh Rolls ($8.45) + Chicken Satay ($9.45) | `protein-plate` | $17.90 | $1.25 | $0.00 | — | — |
| 17089390 | 2026-01-20 | `rated` | Nirvana: Taste of India | Chicken Do Piaza | `protein-plate` | $18.00 | $1.26 | $0.00 | **4 loved** | — |
| 16965013 | 2026-01-13 | `rated` | Poke City MA - Boston | Spicy Salmon Bowl + Grilled Chicken ($2.50) + light sauce | `carb-base` | $18.00 | $1.26 | $0.00 | **4 loved** | — |
| 16768002 | 2026-01-06 | `rated` | Boloco | Modern Mexican Bowl + steak ($1.00), chicken ($1.40), guac ($1.00), chips & pico ($3.50), brown rice | `carb-base` | $18.40 | $1.29 | $0.00 | 3 liked | "Plenty of chicken, but barely any steak." |
| 16636553 | 2025-12-23 | `rated` | The Chicken & Rice Guys | Chicken Salad Plate + Hummus ($2.49) + Mint-Cilantro-Jalapeño ($0.50) | `salad` | $16.24 | $1.14 | $0.00 | 3 liked | — |
| 16340896 | 2025-12-09 | `rated` | Bom Dough | Egg & Greens Bowl + Traditional Pao De Queijo ($3.50) | `protein-plate` | $17.00 | $1.19 | $0.00 | **4 loved** | — |

**2026-09-17 GRECO plate, macro breakdown.** Note box submitted empty, so no mod adjustment applies.

| Component | Protein | Calories |
|---|---|---|
| pork souvlaki, ~5oz | ~38g | ~300 |
| lemon-dill beans (gigantes, olive oil) | ~7g | ~200 |
| tzatziki, 2.5oz | ~3g | ~70 |
| pita bread | ~5g | ~165 |
| **total** | **~53g** | **~735** |

**Settled 2026-09-17: it was a 3.** They confirmed the chat "4/5" meant the fourth of five stars, which is ezCater's `3 liked`. The row stands as recorded, and the flag this entry carried is closed — do not reopen it from the "4/5" wording, which was always the ambiguity and never a second opinion.

**GRECO is still an anchor, and this order is half the reason why.** The 3 is the verdict on this plate; the fact that he came back to GRECO in September after a 4 in April is a separate signal and it is the scarcer one. See **Two signals to return** in `SKILL.md`. Read the 3 as instruction about *what to order here* — the plate was short on food — not as a reason to stop ordering here.

**As actually eaten:** half the pita was left on purpose, so roughly **~50g protein and ~650 calories** went down — **and they were still a little hungry at that.** The build cleared the protein floor by 13g and came in 150 calories under the ceiling. The gates were satisfied and the meal was not. See the satiety note in `dietary-preferences.md`.

**Cancelled 2026-09-16, not an order:** the GRECO Bifteki Plate build ($18.55, ~58g protein, ~1,175 cal). Recorded only so it is not re-picked as untested — it will not clear 800 calories.

## Their reviews, verbatim

Six of forty orders carry text written on ezCater, plus two given in chat. The ezCater six are all complaints, which is the tell: **they write when something is wrong and stay silent when it is right.** The absence of text on a 4 is not missing data — it is the normal shape of a good order. Read the star rating as the verdict and the text as the reason a rating fell short.

| Date | Restaurant | Rating | What they wrote |
|---|---|---|---|
| 2026-09-01 | Roots to Rise | 2 | "Pretty basic sandwich" |
| 2026-07-22 | Shy Bird | 2 | "chicken and the pita were both dry" |
| 2026-06-30 | Noor Mediterranean Grill | 2 | "stale, clearly cooked hours or even a day earlier" |
| 2026-04-28 | Sake Japanese | 3 | "expensive" |
| 2026-03-31 | Dora Taqueria | 2 | "bland" |
| 2026-01-06 | Boloco | 3 | "Plenty of chicken, but barely any steak." |
| 2026-09-08 | Scali Deli & Cafe | 3 | **chat:** "ok, too much lettuce" |
| 2026-09-17 | GRECO | 3 liked | **chat:** beans a good sub for rice, "beans > rice"; souvlaki tasty "but i was still a little hungry"; ate all but half the pita, "trying to reduce carbs"; "doused the whole thing in hot sauce, as i do. i do love spicy food"; tzatziki good "but a higher protein lower fat topping is always preferred" |

**The 2026-09-17 GRECO review is the exception, and the most useful entry here.** The first substantially positive review in the log, it arrived in chat rather than on the site, and it names components instead of verdicts: beans beat rice, hot sauce on everything, pita deliberately left, and the plate still came up short on volume. Four preferences out of one paragraph. Ask for this kind of feedback when a run lands well.

What the ezCater six have in common: **none of them is about macros, price coverage, or format.** Every complaint is about execution — freshness, moisture, seasoning, portion honesty, value. A build can clear every gate in this skill and still earn a 2. That is what these lines are for.

- "Pretty basic sandwich" and "bland" → under-seasoned, assembly-line food. Both were cold-or-generic builds from kitchens that make one thing for everybody.
- "dry" and "stale, clearly cooked hours or even a day earlier" → both Mediterranean pita/shawarma formats, both held too long before delivery. This is the recurring failure mode of the cuisine on this program.
- "barely any steak" → a paid protein upgrade that did not arrive. When a dish takes two proteins, the cheaper one shows up and the premium one does not.
- "expensive" on a 3 → the value complaint. A $17.95 tuna roll built out of $1.00 add-ons read as padding, not as a meal.

## Verdict buckets

### Loved — rated 4, order again freely

Repeating any of these is **encouraged**, including the exact same dish. See **Two signals to return** in `SKILL.md`.

**A second visit promotes a loved kitchen to `anchor`, which is the only tier that beats novelty.** Three restaurants in this log have been ordered from on two different days: **GRECO** (4 then 3), **The Chicken & Rice Guys** (3 then 3) and **Bom Dough** (4, then a bacon side order). Only GRECO carries both a 4 and a re-order, so **GRECO is the log's only anchor**. C&R Guys is a `habit` — twice ordered, never a 4. Everything else in this table is `loved` but unrepeated, which is permission to return rather than a reason to.

Same-day cancellations are not second visits. Six rows in **Cancelled** are swap leftovers sitting on a day that also delivered; those are one visit each.

| Restaurant | Dish | Why it works |
|---|---|---|
| Umai | 8-Piece Nigiri (Raw) Combo | raw fish, nothing to overcook or dry out |
| Tabla Cuisine | Chicken Curry (Large) + Biryani Rice | cooked-to-order Indian, heavily spiced |
| Dirty Water Dough | Cheese Pizza Boxed Meal + Greek salad + roasted chicken | hot, arrives as built |
| Reunion BBQ | Sliced Prime Brisket | real smoked protein, best `protein-plate` on record |
| Perillas Korean Kitchen | Bulgogi Beef Bibimbap | $14.59 — cheapest 4 in the log |
| Saigon Tiger | Beef Noodle Bowl + 2 fried eggs | egg add-ons are a cheap protein lever here |
| GRECO | Spicy Feta & Beef + dips | the one Mediterranean kitchen that scores 4 |
| Stubbys | Jerk Chicken Rice Bowl | aggressive seasoning, the anti-"bland" |
| Mae Asian Eatery | Pad Krapow + brown rice | very spicy, brown rice available |
| Zuzumomo | Tato Jhol Momo (chicken) | only `soup-forward` order in the log and it landed |
| Basil Rice | Spicy Basil Entree + chicken & shrimp | double protein actually delivered |
| Santa Fe Burrito Grill | 3 chicken tacos + carne asada bowl | breaks the one-item rule and still scored 4 |
| Nirvana: Taste of India | Chicken Do Piaza | $18.00 single item, no add-ons needed |
| Poke City | Spicy Salmon Bowl + grilled chicken | raw fish again |
| Bom Dough | Egg & Greens Bowl + pao de queijo | eggs as the entree protein |
| Levain Bakery | chips + walnut raisin roll | side order only, not a lunch |

### Liked — rated 3, fine to repeat, not a priority

Scali Deli (Chicken Mediterranean Salad, **light lettuce**), The Chicken & Rice Guys (both plates), B.GOOD (Grilled Chicken Pesto Sandwich), Laughing Monk Cafe (Cashew Nut + pork), Aceituna Grill (BYO Plate XL), Sake Japanese (Tuna Avocado Roll — *but* "expensive"), Garlic'n Lemons (Spicy Beef Shawarma box), Meet Us Asian Cuisine (Spicy Blue Crab Maki), Boloco (Modern Mexican Bowl — *but* the steak did not show).

### Neutral — rated 2, deprioritize the dish and be wary of the kitchen

Roots to Rise, Shy Bird, Noor Mediterranean Grill, Dora Taqueria, Lotus Test Kitchen, Cafe Landwer. Six restaurants, and four of the six are cold-assembly or held-Mediterranean. Do not rank any of these at 1 when a `loved` kitchen is open the same day.

### Disliked — rated 1

**Life Alive — Teriyaki Tofu Bowl.** The lowest rating in the entire log. Tofu as the protein does not work for him; the menu is vegetable-and-tofu bowls throughout, so the whole restaurant is deprioritized, not just the dish.

### Never Again

_(empty — nothing has been rated 0)_

## Cancelled

Reconciled off the **Canceled** tab on 2026-09-17. None of these were in the log before that scan, which is the reason reconciliation now runs every session. Cancelled orders count for nothing: no verdict, no rotation, no input to `dietary-preferences.md`. They are kept so a build is not re-picked as though it were untested, and so a pattern at one restaurant stays visible.

| ID | Date | Restaurant | Item | Shape |
|---|---|---|---|---|
| 20957421 | 2026-09-17 | GRECO | Tzatziki Sauce | swap leftover |
| 20839618 | 2026-09-10 | Boonnoon Market | Kao Soi Noodle Soup | **day dropped** |
| 20636999 | 2026-09-01 | Dora Taqueria | The Gringa Taco Plate | swap leftover |
| 20524566 | 2026-08-25 | The Chicken & Rice Guys | Chicken & Gyro Salad Plate | swap leftover |
| 19947229 | 2026-07-22 | Shy Bird | Half Rotisserie Chicken | swap leftover |
| 19724164 | 2026-07-07 | Laughing Monk Cafe | Pad Thai | swap leftover |
| 19518520 | 2026-06-23 | Toscano's Italian Kitchen | Chicken Parmigiana | **day dropped** |
| 19167407 | 2026-06-02 | Halal Indian Cuisine | Chicken Dopizza + Mint Chutney | **day dropped** |
| 18743824 | 2026-05-05 | Saigon Tiger | Lemongrass Chicken Rice Bowl + Fried Egg | swap leftover |
| 17592459 | 2026-02-24 | Bon Me | Food Truck Fave Bowl | **day dropped** |

**Six are swap leftovers** and say nothing about the food. Each sits on a day that also has a delivered order, so it is the cleanup half of changing an entree. The 2026-08-25 pair is the clearest case: the same restaurant and the same dish, cancelled and re-placed.

**Four are days that got dropped** with no lunch at all: 2026-09-10 Boonnoon Market, 2026-06-23 Toscano's, 2026-06-02 Halal Indian, 2026-02-24 Bon Me.

**These mean nothing about the food, and that is now stated rather than assumed.** Asked about the Boonnoon kao soi on 2026-09-10, the answer was direct:

> "the soup order was fine, i just had a change of plans for that day and didn't come in. that's going to happen, too. i'll have to cancel for no reason other than i'm not in the office."

So a dropped day is an **attendance** event. It is not a menu rejection, not a bad pick, and not a signal to avoid the restaurant. **Treat all four as untested and as fine** — Boonnoon explicitly so, and the other three by the same default until something says otherwise.

Two consequences worth holding onto:

- **Boonnoon Market is a live lead, not a dead one.** `soup-forward` is the best-rated format in the log on its single order, it is the least used format there is, and the one other soup day was lost to a calendar rather than to a kitchen.
- **Expect more of these, and do not read into them.** Cancelling for no reason other than not being in the office is normal and will keep happening. Never let a run of cancellations at one restaurant harden into a pattern about that restaurant without a rating behind it.

## Unrated

Thai Spice (2026-01-21), Atlantic Poké (2026-01-27), La Hacienda (2026-02-03), Fresh City (2026-03-16), WOW TIKKA (2026-04-27), Publico Street Bistro (2026-05-26), Bom Dough bacon (2026-05-12). Treat as untested, not as bad. Several are second orders placed the same day as a rated one.

## What these ratings mean for future orders

**The patterns derived from this log live in `dietary-preferences.md`, not here.** This file is the evidence; that file is the conclusions. Keeping them apart is what stopped a belief about Mediterranean food from surviving nine ratings that disagreed with it.

Currently derived from the table above and recorded there:

- **Cuisine ranking.** Indian 4.00, East/SE Asian 3.33, Latin 3.00, Mediterranean/Middle Eastern 2.80. Mediterranean is the weakest repeated cuisine, which reverses what this skill used to assume. **Spicy Indian is now a stated standing yes**, and the table only ranks 27 of the 34 rated orders — the other seven are not a cuisine. See `dietary-preferences.md`.
- **Cancelled orders feed none of this.** The ten rows under **Cancelled** carry no rating and no verdict, so they change no average and no rule.
- **Preparation signals.** Cooked-to-order, raw fish and aggressive seasoning predict a 4. Cold assembly, held pita and shawarma, chain burrito bowls and tofu predict a 2.
- **Tofu is never the protein.** The Life Alive bowl is the only 1 in the log.
- **`carb-base` is rationed, not disliked.** It is 27 of 41 orders and carries **12** of the sixteen 4s — and also every rating below 3. It is the high-variance format, not the bad one. **The rationing governs the format, not the presence of a carb**: a light carb side is wanted on every build. `stated` 2026-09-17.
- **Under-used formats.** `soup-forward` has one order (Zuzumomo, rated 4) and `bowl-no-grain` has **none in 41 deliveries**. `soup-forward` has the best record of any format and the least use — reach for it when rotation needs a fresh slot, and note that the only other soup day on record was lost to a calendar rather than to the kitchen. An absence in this log is not a preference.

**After any scrape that adds ratings, re-derive these against `dietary-preferences.md`** and update that file. See **How this file evolves** there for what promotes a pattern to a rule.

## Observed preferences

Derived from actual orders, not stated preferences.

- Chose the highest-priced qualifying option rather than the cheapest, consistently. Thirty-two of forty subtotals land between $16.00 and $18.90.
- Willing to stack add-ons to reach the window: four sauces at Sake, two fried eggs at Saigon Tiger, chips-and-pico at Boloco.
- Has broken the one-item rule himself and rated the result 4 — Santa Fe Burrito Grill, three tacos plus a bowl.
- Picked Scali over Cafe Landwer and Life Alive when all three were available. The ratings say he was right: Scali 3, Landwer 2, Life Alive 1.
- Life Alive is a vegetable-and-tofu menu, nothing clears 40g protein, and it holds the only 1 in the log. Do not build options from it.

### Restaurant intel

**GRECO.** Strong match for the Mediterranean component set and the only Mediterranean kitchen rated 4. Its Greek salad is tomato/cucumber/red onion/Kalamata/feta with no lettuce base at all.

- Plates ($13.95-$17.55) have no cheap protein add-ons — the modal offers only desserts and drinks.
- The Souvlaki Plate is the tunable one: $16.55 chicken, pork +$0.30, lamb +$2.30. Lamb puts it at $18.85, over the $18.68 ceiling. Chicken and pork both fit.
- Create-Your-Own Plate $12.95 with protein/side/sauce selectors tops out near $17.75.
- $1.00 sauces are the precision lever for the last few cents. The Bifteki Plate is the one plate that does not include tzatziki.
- **Create-Your-Own Plate is the calorie-gate workhorse.** Its sauce is a free required selector, so a separate $1.00 sauce line is redundant. Its side selector includes **lemon-dill beans** — gigantes, ~7g protein, ~200 cal, no grain — which beats the pilaf and the fries on both gates. $12.95 + pork souvlaki $3.90 lands on $16.85 at ~735 cal.
- **The beans are the reason this plate works, confirmed 2026-09-17.** They were called out unprompted as better than rice. Always take them here, and look for the same swap on every other menu.
- **Two souvlaki sticks were not enough food.** Next build at GRECO needs more protein volume or a second bean-tier side. The plate has room in both budgets to carry it.
- **Ask for hot sauce.** Not offered in the modal on this plate, so it belongs in the notes box.
- The 2026-04-29 Spicy Feta & Beef at $18.50 is the rated-4 build here.

**The Chicken & Rice Guys.** Ordered twice, rated 3 both times. Hummus is a $2.49 add-on and the mint-cilantro-jalapeño sauce is $0.50 after the free three. Reliable, never exciting.

**Saigon Tiger.** Fried eggs are $2.00 each and stack — the cheapest protein-per-dollar lever in the log. Rated 4.

**Flavor Boom!** Every entree is a $13.85 rice bowl — all `carb-base`, all well under the floor, no add-ons to lift them. Only the Supernova Shrimp Curry Bowl ($16.85) reaches the window, and shrimp curry is thin on protein. Rice is the only grain. Never ordered.

**B.GOOD.** Salads $13.80-$14.95, sandwiches $9.50-$13.23. Nothing reaches the floor alone. The 2026-08-04 order got there with Bacon ($2.50) + Double Protein ($2.65) and rated 3.

**Editing a submitted order (learned 2026-09-16).** The order-details page offers Edit Item, Remove Item and Cancel order.

- **Edit Item works in place.** Changing the notes box or a selector on an existing line updates the submitted order and leaves totals alone. Prefer it.
- **Adding an item does not.** Clicking a menu item while an order is already submitted opens a fresh cart and a *new* pending order. Swapping an entree means: remove the old line, build the new one, place the new order, then cancel the leftover original. Check `/customer_orders` afterward to confirm exactly one upcoming order for that day.
- Removing the last line does not auto-cancel; the order sits at a $1.00 subtotal looking placed. Cancel it explicitly.
- Cancel flow is Cancel order, then confirm on the "Yes, cancel" link.

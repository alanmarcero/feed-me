# Dietary Preferences

**This file is the source of truth for what the user will and will not eat.** `SKILL.md` holds the process; `order-history.md` holds the evidence; this file holds the rules those two are serving.

The skill reads this file on load. If it is missing, the skill does not order — it runs the onboarding interview in **First run** in `SKILL.md` and writes this file from the answers.

Last reviewed: 2026-09-17 (two refine passes).

**First refine pass — nine answers, every one written in below as `stated`.** The big ones: the protein floor is **45g**, not 40g; carbs are **wanted but light** rather than being reduced; the vegetable limit is about **cheapness, not vegetables**; full portions get bought and under-eaten rather than ordered small; spicy Indian is a standing yes; spice goes to the **top of any heat scale**; novelty is the default but **a 4 plus a re-order is an anchor that beats it**, which is what GRECO is; the GRECO plate itself settled at **3**, which reads as what to order there rather than whether. Cancelled days are usually him not being in the office and mean nothing about the food.

**Second refine pass, same day — eight more answers, also all `stated`.** The schedule is **Tuesday always, Thursday added for the next few months, Monday a few times a year**, and anything the site shows is fair game. **Sushi stays**, with the 45g floor as best-effort on those days and a filling alternative offered alongside. **There is no separate calorie floor** — the protein floor implies it. **The spend floor is retired**; the only money rule is $0.00 out of pocket. **The Atlantic Poké bowl is a 4** and poke is a standing yes. **Soup only when it is a unique soup.** **No drinks, no desserts, ever.** **Take the add-on whenever the budget allows.** A third update landed after those: **every free premium vegetable gets taken, all of them on a build-your-own bowl**, while cheap salad greens stay skipped, **corn is scored as a grain and skipped with them**, and **broccoli is out on its own terms**.

## Provenance, and why it matters

Every rule below is tagged. The tag decides whether the skill may change it on its own.

| Tag | Means | May the skill revise it? |
|---|---|---|
| `stated` | the user said it, in conversation or in the onboarding interview | **No.** Only the user changes a `stated` rule. Contradicting evidence gets raised with them, not applied. |
| `derived` | inferred from ratings, reviews and order patterns | **Yes.** Revise it whenever the log stops supporting it. Say so in the writeup. |
| `provisional` | derived from too little data to trust yet | Yes, and actively look for evidence either way. Promote to `derived` or drop it. |

A `derived` rule that contradicts a `stated` one loses. If the log keeps arguing with something they said, **say so once and keep obeying the stated rule** — that is a conversation to have, not a call to make.

## Hard limits

Gates. Nothing overrides these — not price, not macros, not rotation, not a loved rating.

| Limit | Tag | Note |
|---|---|---|
| **No allergies declared** | `stated` | Nothing is medically off-limits. If this ever changes it outranks every other line in this file. |
| **Tofu is never the protein** | `derived` | The one tofu bowl is the only 1-star in 40 orders. Tofu as an incidental component is fine; tofu carrying the protein number is not. |
| **No cheap-filler vegetable base** | `stated`, refined 2026-09-17 | **The line is quality, not vegetables.** "a bed of lettuce is low quality vegetables. cheap. but a bed of cucumber salad is not cheap." See **Where the vegetable line actually falls** below. |
| **No drinks, no desserts** | `stated` 2026-09-17 | *"i dont order drinks or desserts, save that money for protons."* Never add either — not to close a price gap, not as a treat, not at any price. See **Portion and value**. |
| **Nothing on the Never Again list** | `stated` | Lives in `order-history.md`. One entry: the Life Alive Teriyaki Tofu Bowl. |
| **Nothing they rated 1 or 0** | `derived` | A rating of 1 retires the dish. A 0 retires the dish and puts the kitchen under suspicion. |

### Where the vegetable line actually falls

`stated` 2026-09-17. Asked where "nothing vegetable-forward" ends and "vegetables with real bulk" begins, the answer was not about volume at all:

> "the line lies in quality — a bed of lettuce is low quality vegetables. cheap. but a bed of cucumber salad is not cheap."

**So the gate is cheapness, not vegetables.** The objection to a lettuce base was never that it is a plant. It is that shredded lettuce is the cheapest thing a kitchen can put under a protein, it displaces food that cost something, and it is charged for as though it were a meal.

| Fine as a base | Not a base |
|---|---|
| cucumber salad, tomato and onion, fattoush, tabbouleh | shredded iceberg or romaine under anything |
| cabbage slaw, pickled vegetables, roasted vegetables | spring mix used as volume |
| beans, legumes, quinoa salad | raw greens as the majority of the container |

This is the same axis as the `expensive` complaint at Sake and the "basic sandwich" at Roots to Rise. **He is not counting calories on the vegetable, he is counting what the kitchen spent.** A dish reads cheap when its bulk is the cheapest ingredient available, and that reads through as a 2 whatever the macros say.

Practical test before ranking a salad or a bowl: **if the base were removed, would the rest still look like lunch?** Cucumber salad, beans and roasted vegetables pass. A bed of lettuce does not.

### Free premium vegetables are always taken. All of them.

`stated` 2026-09-17, and it is the active half of the rule above:

> "if extra *premium* vegetables are free, we get extra premium vegetables. we skip cheap salad and leave it for the rabbits. on bowls like mexican or poke, we get *all* the vegetables"

**The cheapness gate was never an instruction to avoid vegetables — this is what it was protecting.** Skipping the lettuce exists so the container carries things worth paying for, and a free premium vegetable is the best trade on any menu: real bulk, real satiety, real flavour, no calories worth counting and no dollars taken from the protein.

| Take every one that is free | Leave it |
|---|---|
| avocado, grilled or roasted vegetables, mushrooms, peppers, onions | shredded iceberg or romaine |
| edamame, seaweed salad, beans, artichoke, roasted red pepper | spring mix |
| pickled vegetables, kimchi, cabbage slaw, radish, jalapeño, olives | plain lettuce in any role |
| cucumber, tomato, red onion, scallion, cilantro, mango | **corn** and **broccoli** — see below |

**On a build-your-own bowl, take all of them.** `stated`, and he named the two formats: **Mexican and poke**. Those modals list eight or twelve free vegetable checkboxes and there is no reason to leave one unticked. The same applies to any counter that works this way — shawarma plates, grain bowls, burrito bowls, build-your-own salads.

**Corn is out, free or not.** `stated` 2026-09-17: *"we skip corn when we can. corn is a grain disguised as a vegetable. very sneak corn, but it won't work."*

**So corn is scored as a grain, not as a vegetable**, which puts it under the light-carb rule rather than under this one. It never counts toward "all the vegetables" on a bowl, and taking it is spending the build's one carb slot on something that arrived free in a vegetable bin. **Skip it wherever the modal lets you** — "when we can" is the honest limit, since it comes baked into some builds as part of a salsa, a succotash or a pre-mixed side, and picking it out of a corn salsa is not a real ask.

The rule generalizes past corn: **starchy things wearing a vegetable label get scored as starch.** Peas, potato, plantain and sweet potato all belong to **Base and side choice**, not to the free-vegetable sweep.

**Broccoli is out too, and for a different reason.** `stated` 2026-09-17: *"broccoli bowl is still out, we skip broccoli too."* No reclassification and no argument behind it — he does not want it, which is reason enough and outranks anything this file could derive.

**So broccoli is skipped wherever the modal allows, and a bowl built on broccoli is not a candidate at all**, whatever it does for the macros. It is genuinely a good vegetable on every axis this file otherwise cares about, and none of that matters.

**Do not extend this to anything else.** Broccoli is not a proxy for cruciferous vegetables or for green vegetables: **cabbage slaw, kimchi and pickled vegetables stay on the take-everything list** above, where he put them. Cauliflower, brussels sprouts and broccolini have never been mentioned either way and stay untested rather than assumed out. Widening a named dislike into a category is exactly the mistake the Mediterranean entry is in this file to prevent.

Three things this does not license:

- **A vegetable that costs money competes with protein, and protein wins.** `derived` — he said *free*. A paid vegetable add-on only goes in when the build already clears 45g and there is still room under the ceiling. Avocado at $1.50 is the usual case, and it loses to an egg or an extra scoop of meat every time.
- **Vegetables never become the base.** Taking all the peppers does not make a pepper bowl into lunch. The protein floor and the cheapness test are unchanged.
- **They still get counted.** Every line goes in the macro estimate — see **Estimating macros** in `SKILL.md`. Most are 10-30 calories and vanish. **Avocado at ~80 cal a scoop is the one that actually moves a total** against the 800 ceiling; the rest are free in both budgets. Corn would have been the other, and it is barred above on its own merits.

**Select them in the modal, never in the notes box.** They are structured selectors, so asking for them in writing restates a choice the ticket already carries — see **Required: validate the note against the order** in `SKILL.md`.

## The schedule

`stated` 2026-09-17, asked whether this is a one-day-a-week program given that 29 of 33 delivery weeks in the log carried exactly one order:

> "historically the schedule was every tuesday. for the next few months they added thursday. a few times per year it's on monday's, too. if it's available on the site, it's available to feed me."

**The last line is the rule: if the site shows a day, it gets an order.** There is no day that is skipped on principle, no weekday that is "not his day," and no reason to stop at one order because one order is what the history shows.

| Day | Expect it |
|---|---|
| **Tuesday** | always — 33 of the 41 delivered orders |
| **Thursday** | **added for the next few months from 2026-09-17** |
| **Monday** | a few times a year |
| anything else | the site decides, not this file |

**So expect two-day weeks to be the normal case for a while**, and plan the batch whole across both — see **Variety and rotation** in `SKILL.md`. The single-day history is what the program offered, not what he chose.

**Never infer the schedule from this table.** It records what he said to expect; the site records what is actually open. Enumerate the days on `/schedule` every run and order every one of them.

## Calories

**Ceiling: 800 calories**, estimated, for the build as ordered. `stated` 2026-09-16.

A gate, not a goal. An item over 800 does not go on the slate at rank 1 and does not get placed, however well it scores on protein, price or format.

It routinely binds before the budget does, so expect builds to land under the spend ceiling and leave stipend unspent. That is the gate working. **Never add a side, a drink or a dessert to close a price gap if it pushes the build over 800.**

**No calorie floor, and the question is now settled.** `stated` 2026-09-17, asked directly whether a ~700 floor should exist so builds aim at the ceiling rather than under it:

> "calorie floor is dictated by the protein floor. 45g of protein is 45x4 calories"

**So the only calorie minimum is the one the protein floor implies** — 45g of protein is 180 calories on its own, and whatever the rest of the build costs to deliver it. Do not invent a number above that, and do not pad a build with calories to reach one. A build that clears 45g at 600 calories is a better order than one that clears it at 790.

**This means satiety is fixed by composition, never by a calorie target.** The section below is the whole of the answer: more room per calorie, from beans, bulk and a second protein-forward component. There is no number to aim at.

### Satiety is a separate problem from calories

**A build can clear every gate and still leave them hungry.** `stated` 2026-09-17, on the GRECO plate: ~50g protein and ~650 calories as actually eaten, 13g over the protein floor and 150 under the calorie ceiling, and the report was "still a little hungry."

So the ceiling is not the target and the floor is not the finish line. **Within the calorie budget, prefer the components that take up the most room**, because volume is what ends a meal:

- **Beans, legumes and high-fiber sides** over rice, fries or bread for the same calories. This is also a stated flavour preference — see **Base and side choice**.
- **Vegetables with real bulk** over dressings, oils and creamy sauces, which spend the budget without filling anything. **Bulk from a vegetable worth paying for** — see **Where the vegetable line actually falls**. Lettuce volume is not satiety, it is filler.
- **A second protein-forward component** over a larger starch, when the menu offers the choice.

**Do not respond to this by raising the ceiling.** The ceiling is `stated` and stays where it is. Fix satiety by changing what fills the budget, not by enlarging it.

## Macros

**Protein floor: 45g**, estimated. `stated` 2026-09-17, raised from 40g.

The floor that never bends. 43g is not close enough. Among options that clear it, more protein is not better — once 45g is met, calories and their ratings decide.

**Why it moved.** The 2026-09-17 GRECO plate cleared the old floor by 13g at ~50g eaten and still came up short on food. 40g was set before any build had been estimated against a real appetite; 45g is the number that plate says it should have been. Expect this to bind more often than it used to, and expect the paid protein add-on — an egg, a double, a swap — to be how most builds reach it.

**The floor is best-effort on a sushi day, and only there.** `stated` 2026-09-17, asked whether raw fish should be exempted from the 45g floor given it is the second-best-rated thing in the log:

> "sushi is great, i love sushi. but i am often still hungry on sushi day because it's expensive. on these days we do our best to hit the 45g floor."

**Sushi is not dropped and the floor is not waived.** Poke bowls reach 45g with a paid protein add-on and should always take one. Sushi rolls mostly cannot reach it under the ceiling — build the highest-protein combination the menu allows, take nigiri over rolls where both exist, and **say in the writeup how far short it landed.**

**And offer the alternative rather than deciding for him.** His own phrasing of what he wants to see:

> "maybe give the user options like 'yo my dude. you love sushi, and it's on the menu. but i found this as well that would be more filling'"

So on any day where sushi is available and cannot reach 45g, **the slate presents both** — the sushi build and the most filling qualifying build on that day's menus — and **asks which one he wants, in Claude's normal voice, outside the greentext.** See **Questions are never greentext** in `SKILL.md`. Do not silently rank the filling option first and bury the sushi; he has stated he loves it, and the hunger is a known, accepted trade.

**Protein is bought, not asked for.** `stated` 2026-09-16. It is the costed component, so it gets purchased in the option modal where the upcharge shows in the cart. A note asking for extra meat is ignored at most kitchens and reads badly at some. **A note's protein never counts toward the 45g floor; a note's calories always count against the 800 ceiling.**

**One protein bought well beats two bought badly.** `derived`. When a build stacks two paid proteins, expect the cheaper one to arrive in quantity and the premium one to be short.

**A carb is always wanted. A light one.** `stated` 2026-09-17, correcting the "actively reducing carbs" reading this file carried for a day: *"i always want some sort of carb, just light carb. a side of flatbread, beans, quinoa salad, brown rice, etc."*

**Every build should carry a carb component, and it should be a side rather than the base.** A plate with no starch at all is not the goal and never was — the half-pita left at GRECO was portion control on a carb he wanted there, not a rejection of the pita.

His own examples are the spec: **flatbread, beans, quinoa salad, brown rice.** Note that beans are on that list. He counts them as the carb *and* as the fibre *and* as part of the protein, which is why the bean swap wins on every axis at once — see **Base and side choice**.

So the `carb-base` rationing below governs the **format**, not the presence of starch. A `protein-plate` with a quinoa side is a light-carb build and is exactly right. A rice bowl where rice is the meal is the thing being rationed.

**Fibre is wanted, for satiety and on its own terms.** `stated` 2026-09-17, cited as a reason beans beat rice. No number.

**Portion control is his job, not the order's.** `stated` 2026-09-17: *"i prefer to get the food and just eat less of it versus paying regular price for less food, or all lettuce."* This outranks a lot of instinct in this file — see **Buy the full plate** under **Portion and value**.

No stated targets for fat or sodium as numbers, but see the topping rule under **Components**. Do not invent targets that were never given.

## Cuisines

Ranked by their own ratings across 40 orders. `derived` — revise this table whenever a scrape moves an average.

| Cuisine | Rated orders | Avg | Read |
|---|---|---|---|
| Indian | 2 | **4.00** | **Take it whenever a day offers it.** `stated` — see below. |
| East / Southeast Asian | 13 | **3.38** | Eight of the seventeen 4s. The deepest reliable bench. **3.58 with the Life Alive tofu bowl set aside.** |
| Latin | 3 | 3.00 | Wide spread — a 4, a 3 and a 2. |
| Mediterranean / Middle Eastern | 10 | **2.80** | The weakest repeated cuisine. **2.63 with GRECO set aside.** |

**Recounted 2026-09-17 after the Atlantic Poké bowl was rated 4 in chat.** East/SE Asian moves from 3.33 to 3.38 and now ranks 13 of the 35 rated orders.

**Spicy Indian is a standing yes.** `stated` 2026-09-17: *"spicy indian is always a good choice, yes."* This promotes a two-order pattern into a rule the log could not have justified on its own — Tabla and Nirvana both rated 4, WOW TIKKA went unrated, and Halal Indian was cancelled on a day he was not in the office. **When an Indian kitchen is on a day's list, it is the default pick for that day**, subject only to the hard gates and the format rotation. Order it at the highest heat offered.

**Poke is a standing yes, alongside spicy Indian.** `stated` 2026-09-17: *"i know the poke bowl is always a great choice, can give that a 4/4."* That settled the unrated 2026-01-27 Atlantic Poké bowl at **4 loved** and made poke **two orders, two 4s** — Poke City and Atlantic Poké. Raw fish overall is now **five rated orders averaging 3.60**, the best record in the log after Indian.

**Poke is also the raw-fish format that can reach the protein floor**, because every poke counter sells a second scoop or a grilled-protein add-on. Poke City's rated-4 build did exactly that. **When a day offers poke, take it and buy the extra protein** — it is the one place where "he loves it" and "it clears 45g" stop competing.

**The Mediterranean line is a correction, and it is worth understanding rather than just obeying.** This file used to claim Mediterranean builds land well. That came from components the user listed — tahini, hummus, feta, olives — and not from a single rating. Nine rated orders say otherwise: two 2s, both with freshness complaints attached, four 3s, and one 4.

Recounted 2026-09-17 across all 41 orders: **10 rated, averaging 2.80** — three 2s (Noor, Shy Bird, Cafe Landwer), six 3s, and one 4. Set GRECO's two aside and the remaining eight average **2.63**, with no order above a 3. The rule holds and the evidence behind it got stronger.

The components are genuinely liked. The kitchens serving them on this program mostly hold the food too long. **Order the hummus; stop picking the restaurant because it has hummus.**

The one exception is **GRECO**, which cooks to order. Two orders, a **4** in April and a **3** in September — the chat "4/5" was confirmed as the fourth of five stars, so the second plate is a `3 liked`.

**That does not demote GRECO, because the second order is itself the signal.** He went back. Given a live slate and a preference for novelty, he chose GRECO again, and that choice is evidence the star click cannot carry. A 4 plus a re-order makes GRECO **the only anchor in the log** — see **Two signals to return** in `SKILL.md`, and **Novelty is the default** below.

Read the 3 as instruction about *what* to order there, not *whether*. The plate was short on food, which the 45g floor now handles. **Treat GRECO as separate from the cuisine average**, which is dragged down by kitchens that hold food.

### What the table does not cover

`derived` 2026-09-17. **The four rows above rank 27 of the 34 rated orders.** The other seven are not a cuisine and should not be forced into one:

| Order | Rating |
|---|---|
| Reunion BBQ — sliced prime brisket | **4** |
| Dirty Water Dough — pizza + chicken | **4** |
| Stubbys — jerk chicken bowl | **4** |
| Bom Dough — egg & greens bowl | **4** |
| B.GOOD — grilled chicken pesto sandwich | 3 |
| Roots to Rise — turkey BLT | 2 |
| Levain — chips + roll *(side order, not a lunch)* | 4 |

**Six lunches, average 3.50** — higher than every row in the table except Indian, and above East/SE Asian's 3.33. American BBQ, a pizza counter, a jerk counter and a Brazilian egg bowl have no cuisine in common, so **do not read this as "American rates well."**

What they share is the axis already in **Preparation**: the four 4s were all cooked hot to order, and the 3 and the 2 were both assembly-counter food. **Cuisine is the proxy; hot-versus-held is the thing.** When a day's menus offer a kitchen that cooks on the line, it does not need a row in this table to be the right pick.

**East/SE Asian's 3.38 includes the Life Alive tofu bowl**, the only 1 in the log. The twelve cuisine-native Asian kitchens average **3.58**. The row stays as counted, but the bench is stronger than the number reads.

Practical rule: when a day's menus offer an Indian or Asian kitchen alongside a shawarma counter, **the shawarma counter is the weaker bet even when its build looks better on paper.**

### Novelty is the default. GRECO is the exception.

`stated` 2026-09-17: *"i prefer novelty but greco is good."*

This settles a gap the log could not: sixteen kitchens have rated 4 and only GRECO was ever ordered from twice, and the reason is preference rather than the program rotating them out of reach.

So **a 4 on its own is permission, not instruction.** A `loved` rating means a repeat is welcome; it does not mean a repeat is preferred. Between an untested kitchen that clears the gates and a `loved` one that also clears them, **take the untested one** and say in the writeup that novelty broke the tie.

**A second signal changes that.** `stated` 2026-09-17: *"a re-order is signal and also a perfect rating is a signal — both to reorder again from the same restaurant."* A kitchen that earned a 4 **and** was chosen again is an **anchor**, and an anchor is the one thing that beats novelty on a tie. The preference for novelty is exactly what makes a repeat worth reading — going back cost him a chance to try something new, and he did it anyway.

The full ranking lives in **Two signals to return** in `SKILL.md`. In this file, what matters is the order of precedence:

- **A hard gate.** An untested kitchen that cannot reach 45g protein under 800 calories does not get picked to be interesting. Neither does an anchor.
- **An anchor.** Currently GRECO alone: a 4 in April, a return in September, cooks to order, named as a keeper. Repeat it without apology.
- **Novelty.** The default among everything that clears the gates.
- **A `loved` kitchen never returned to.** Welcome, but it loses a tie to something new.
- **A `neutral` or worse kitchen.** Novelty is a reason to try something new, never a reason to revisit something that already disappointed.

The honest read of the last nine months is that novelty has been the right call: the untested pick landed a 4 more often than not, and the failures clustered in untested *sandwich and wrap counters* rather than in untested kitchens generally. **Be new, be hot-cooked.**

## Components

The part that generalizes across restaurants. Apply to any menu, not just the one a preference came from.

### Wants — include when the menu offers them

`stated`. Confirmed by the log: hummus twice at The Chicken & Rice Guys, tzatziki and pepper paprika dip at GRECO, fattoush and pickled turnips at Aceituna.

tahini · hummus · feta · olives · tomato · onion · cucumber

**And every free premium vegetable the modal offers**, taken in full — see **Free premium vegetables are always taken** above. That rule outranks this list: it is not a set of favourites to look for, it is an instruction to tick every box that costs nothing.

**Spice and hot sauce. Hottest available.** `stated` 2026-09-17: "i doused the whole thing in hot sauce, as i do. i do love spicy food," and, asked whether to take the top heat level or one below, *"let's do hottest when available."*

Take this as a standing instruction, not a garnish note. **Where a menu offers a heat scale, pick the top of it** — Thai 5-star, vindaloo, extra-spicy, hot rather than medium. **Order the spicy version where one exists**, take the hot sauce or chili option whenever the modal offers it free, and ask for hot sauce on the side in the notes box when it does not.

No hedging on this one. There is no evidence of a heat level being too much for him anywhere in the log, and the spiciest things he has eaten — Pad Krapow, jerk chicken, gochujang, vindaloo, spicy basil — are where the 4s are. It costs nothing, it never threatens a gate, and it confirms what the ratings already implied — Pad Krapow, jerk chicken, gochujang and spicy basil all rated 4.

Read as components to add to a build, **not as a reason to choose a restaurant.** The cuisine table above is the restaurant-level signal; this list is the build-level one.

### Toppings and sauces: protein up, fat down

`stated` 2026-09-17, on the GRECO tzatziki: it was good, "but a higher protein lower fat topping is always preferred."

**Rank the sauce and topping selectors on that axis**, not on flavour alone:

| Prefer | Over |
|---|---|
| yogurt-forward, lean, or salsa/vinegar-based | oil-forward, mayo-based, creamy or buttery |
| hummus, tzatziki, lean whipped feta, chutney, salsa, hot sauce | aioli, ranch, tahini-heavy dressings, oil-based vinaigrettes |
| a protein-bearing topping (cheese, egg, beans, yogurt) | a fat-bearing one that adds calories and no protein |

Tzatziki stays acceptable and was liked. It is simply not the best available answer when something leaner is on the same menu. **Hot sauce is the ideal case on this axis**: all flavour, no calories, no fat.

This also sharpens the satiety rule above. A creamy dressing spends the calorie budget without filling anything, which is exactly the trade to stop making.

### Eggs as a protein lever

`provisional` 2026-09-17 — two orders, both rated 4. Saigon Tiger with two fried eggs at $2.00 each, and the Bom Dough Egg & Greens Bowl with egg as the entree protein itself.

Eggs are the cheapest protein-per-dollar add-on anywhere in the log, they carry no starch, and they are bought in the modal rather than asked for, which satisfies the protein rule above. **Take the egg add-on wherever a menu offers one**, and treat an egg-based entree as a real lunch rather than a breakfast item. The Roots to Rise hard-boiled eggs sit on a 2, but the review blamed the sandwich, not the eggs.

### Wants less of

- **Lettuce.** `stated`. The standing complaint — filler that displaces real food. Order salads light-lettuce or no-lettuce wherever mods are accepted, and never count lettuce volume as part of the meal.

### Rationed, not disliked

- **Starch bases** — wrap, sandwich, sub, burrito, pizza, rice bowl, grain bowl. `stated`: "not every week." Enforced by the `carb-base` cooldown in `SKILL.md`.

  **This is a rationing rule, not a dislike, and the difference matters.** `carb-base` is **27 of 41 orders** and carries **13 of the 17 fours** — 12 if the Levain side order is set aside. Recounted 2026-09-17 after the Atlantic Poké bowl was rated 4.

  **It is also the only format that has ever failed.** All six 2s and the single 1 are `carb-base`. No `salad`, `protein-plate` or `soup-forward` order has ever rated below 3.

| Format | Rated | Avg | Worst | Best |
|---|---|---|---|---|
| `soup-forward` | 1 | **4.00** | 4 | 4 |
| `protein-plate` | 7 | **3.43** | 3 | 4 |
| `carb-base` | 23 | 3.17 | **1** | 4 |
| `salad` | 3 | 3.00 | 3 | 3 |
| `bowl-no-grain` | **0** | — | — | — |

  **That table counts lunches only.** The $4.13 Levain chips-and-roll is a `carb-base` side order and is excluded; fold it back in and `carb-base` reads 24 orders at 3.21. Recounted 2026-09-17 with Atlantic Poké rated 4.

  **So `carb-base` is the high-variance bet, not the bad one.** It holds the best food in the log and every disappointment in it. That sharpens the cooldown into a real rule: when a `carb-base` slot is spent, spend it on a kitchen with a rating behind it — never on an untested sandwich or wrap counter, which is where all seven failures came from.

    **Soup counts only when the soup is interesting.** `stated` 2026-09-17: *"soup is an ok option if it's a unique soup. nothing boring."* Kao soi, momo jhol, pho, tonkotsu, a real tom yum — yes. Chicken noodle, minestrone, a generic soup-and-half-sandwich box — no, and a boring soup does not get picked just because rotation wants the format. **`soup-forward` is a permission with a taste filter on it**, which is why the format sits at one order in nine months despite rating 4.

  **The two formats at the top of that table are the two least used, and one has never been used at all.** `derived` 2026-09-17. `soup-forward` has a single order and it rated 4. `bowl-no-grain` has never been ordered once in 41 deliveries — and it is the exact shape of three rules this file now carries: carbs down, beans first, satiety from bulk rather than starch. **An absence in a log is not a preference.** Reach for both when rotation opens a slot, and treat `bowl-no-grain` as untested rather than unwanted until an order says otherwise.

  **And `salad` is the flat one.** Three orders, all rated exactly 3, no 4 and no 2. It never fails and it never delights, which makes it the right pick only when rotation demands a format and nothing better is open.

### Base and side choice

**Beans first.** `stated` 2026-09-17: "the beans were a nice sub for rice, i often do this myself outside of work meals. beans > rice — or beans & rice is also good. extra fiber and protons in the beans." They already do this off their own bat, which makes it a habit rather than a one-off reaction to a plate.

Full order of preference when a base or starch side is being chosen:

1. **Beans or legumes** — gigantes, pinto, black, chickpea, lentil. Best on flavour, fibre, protein and satiety at once.
2. **Beans and rice together**, explicitly fine, and better than rice alone.
3. **Quinoa**, then **brown rice**. `stated` 2026-09-09.
4. **White rice**, acceptable rather than a reason to drop the item — a white-rice build rated 4 at Basil Rice — but say in the writeup that it was not their pick.

Confirmed in the log: brown rice at Mae Asian Eatery, Boloco and Atlantic Poké; brown rice & red quinoa at WOW TIKKA; lemon-dill beans at GRECO, which drew the review that set this rule.

**Where a menu lets a bean side replace a grain, take it**, even at a small upcharge. It is the single highest-value swap available on most of these menus: it satisfies the stated preference, adds fibre and protein, and buys satiety inside the calorie ceiling.

## Preparation

`derived` from the 4s versus the 2s. This is the axis the macro arithmetic cannot see, and it is where their written reviews all land.

**Predicts a good rating:**

- Cooked to order and served hot.
- Raw fish. Sushi and poke cannot be dried out or held badly — every raw-fish order rated 3 or 4. **Five rated orders averaging 3.60** (Umai 4, Poke City 4, Atlantic Poké 4, Sake 3, Meet Us 3), which is the best record in the log behind Indian. **Poke is 2 for 2 at 4 and is a `stated` standing yes.** On the 45g floor: poke reaches it with a paid protein add-on and always should; sushi rolls mostly cannot, and that is handled as a best-effort day with a choice offered — see **Macros**.
- Aggressive seasoning: curry, jerk, gochujang, holy basil, harissa. **Now `stated` as well as derived** — they love spicy food and add hot sauce themselves. See **Wants**.
- A cuisine-specific kitchen over a generic bowl-and-sandwich chain.
- **Grilled over fried** (`stated`) — when a menu offers grilled chicken against a breaded cutlet, specify grilled.

**Predicts a bad rating:**

- Cold assembly — deli sandwiches, wraps, pre-built boxes.
- Pita and shawarma held between cook and delivery.
- Chain burrito-bowl formats.
- Tofu in any load-bearing role.

**Freshness is worth a note line.** `derived`. Two of their six written reviews are about food held too long. At a pita, shawarma or wrap counter, a short "fresh off the grill if you can, please" is a reduction-tier ask that costs the kitchen nothing and targets the exact failure mode that produced both 2s.

## Portion and value

- **One real menu item**, plus at most one add-on or upgrade. `stated`. A salad with a meat add-on is fine; three à-la-carte sides stacked into a fake entree is not.

  **Take the add-on whenever the budget allows.** `stated` 2026-09-17, asked whether the one-item rule still holds given he broke it himself at Santa Fe Burrito Grill: *"yes we can always do an addon if the budget allows."*

  **So the add-on is the default, not the exception.** A build that leaves room under the ceiling and takes no upgrade is an under-built order — that room should have gone to protein, per **Macros**. What he did *not* license is stacking several entrees: the Santa Fe order stays a logged exception rather than a pattern to copy.

- **Buy the full plate and eat less of it.** `stated` 2026-09-17: *"i prefer to get the food and just eat less of it versus paying regular price for less food, or all lettuce."*

  **This is a purchasing rule and it cuts against the instinct to order light.** Given a full-size item and a smaller one at a similar price, take the full-size one. Given a plate with a starch side and the same plate without, take the one with the side. Leftover food on the plate is a choice he makes at the desk; it is never a reason to buy less at the menu.

  Two things it rules out, both of which this skill would otherwise have drifted toward:

  - **Never downsize to hit the calorie ceiling.** Change what is on the plate, not how much of it there is. A smaller portion of the same thing is the worst available answer, because it costs the same and delivers less.
  - **Never substitute greens for a component to save calories.** That is the "all lettuce" case by name — paying full price for filler.

  It also resolves the GRECO half-pita. He wanted the pita on the plate and ate half of it. The correct build still includes the pita.

- **Padding to reach the spend floor is weak — and now moot.** `stated`. Adding a $1.50 banana purely to clear the floor was always poor; with the floor retired below there is no floor to pad toward, and with drinks and desserts barred there is nothing to pad with. Kept as the reasoning behind both.

- **Value is part of the verdict.** `derived`. A $17.95 build assembled out of $1.00 add-ons earned "expensive" at a 3. Reaching the ceiling through many small upcharges reads as padding even when the arithmetic is right.

- **Price does not predict the verdict, and the correlation actually runs backwards.** `derived` 2026-09-17, **recounted in the refine pass with the two side orders excluded** — the $4.13 Levain chips-and-roll and the $4.00 Bom Dough bacon are not lunches and were inflating every price statistic in this bullet.

  | Rating | Rated lunches | Avg subtotal |
  |---|---|---|
  | **4** | 16 | **$17.59** |
  | 3 | 11 | $17.69 |
  | 2 | 6 | $17.87 |
  | 1 | 1 | $17.95 |

  **A clean inverse ladder, monotonic at every step.** The 34 rated lunches average **3.24**. Orders at or above $18.00 average **3.29** across 14; orders under $17.00 average **3.33** across 6. The cheapest 4 in the log is Perillas at **$14.59**; the most expensive order in it is Roots to Rise at $18.88, rated 2.

  The sample is small and the spread is narrow, so do not read this as *cheaper is better*. Read it as **spending more has never once bought better food**, which is the same conclusion with none of the overreach.

- **The spend floor is retired.** `stated` 2026-09-17, asked whether to keep $16.85 as a target given the ladder above:

  > "we just dont want to spend extra money. they're buying me lunch, not the other way around."

  **There is one money rule and it is $0.00 out of pocket.** The $18.68 ceiling stays exactly as it is, as a hard gate against a card prompt. **$16.85 is no longer a target, a floor, or a thing to report as missed.** A build that clears the dietary gates at $14.59 is a complete order, and Perillas is the log's proof.

  What this kills outright:

  - **Padding.** Never add anything to a build to lift a subtotal. Combined with the drinks-and-desserts limit, there is nothing left to pad with anyway.
  - **"Unspent stipend" as a cost.** Do not report it as one. The stipend is theirs to offer, not his to maximise.
  - **Ranking on spend.** Two builds that both clear the gates are separated by their ratings and the rotation, never by which costs more.

  **What survives is the add-on rule above**, and the two are not in tension: the budget buys *protein* when protein is what the build needs. It does not buy dollars for their own sake.

## How this file evolves

**This file is meant to change.** Every scrape of their ratings is new evidence, and evidence that never updates the rules is evidence wasted. After any run that reads new reviews, review this file against what the log now says.

What to do with new evidence:

- **A `derived` rule the log no longer supports** — revise or delete it, and say so in the writeup. The Mediterranean correction is the worked example: a belief held on stated components, overturned by nine ratings.
- **A pattern across three or more rated orders** — write it in as `derived`. Name the orders it came from.
- **A pattern across one or two orders** — write it in as `provisional`, or leave it out. Do not harden a rule on a single meal.
- **A `stated` rule the log contradicts** — do not touch it. Raise it with them once, in the writeup, with the ratings that disagree. They decide.
- **Anything they say in conversation** — write it in as `stated`, dated, in their words where possible. Stated beats derived on the same subject.
- **A reason given when cancelling an order** — `stated` 2026-09-17: *"if the user gives a reason why they want to cancel and re-order, that's a strong signal."* It is their words about a specific build, given before anyone ate, which makes it cleaner evidence than a review tangled up with how one kitchen cooked that day. **Sort it first**: a reason about the day ("not in the mood", "too heavy for today") is transient and changes no rule, while a reason about the food ("no more curry this soon", "nothing creamy") is a preference and goes in here as `stated`. When it is unclear, treat it as about the day and say so. See **Cancel and swap** in `SKILL.md`.
- **A new allergy or medical limit** — goes straight into **Hard limits** as `stated`, and outranks everything else in the file.

Keep the tags current. An untagged rule is one nobody can safely revise, which is how the Mediterranean claim survived nine contradicting orders.

Update `Last reviewed` at the top when the file is checked against a fresh scrape, even when nothing changes. A date that has not moved in months means the ratings are not being read.

**This file is personal data, and that is what it is for.** Write allergies and medical limits in full — how severe, what triggers it, what happens. A gate that reads "shellfish, anaphylaxis, includes shared fryers" gets enforced more carefully than one reading "no shellfish," and enforcement is the whole reason the line exists. Never soften a safety line to make the file easier to share.

Whether this file ever leaves the machine is the user's decision, not the skill's. See **The data files are personal** in `SKILL.md`: the skill does not push, does not add remotes, and does not publish anything on its own initiative.

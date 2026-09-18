# Dietary Preferences

**This file is the source of truth for what the user will and will not eat.** `SKILL.md` holds the process; `order-history.md` holds the evidence; this file holds the rules those two are serving.

The skill reads this file on load. If it is missing, the skill does not order — it runs the onboarding interview in **Preferences: best with them, fine without** in `SKILL.md` and writes this file from the answers.

Last reviewed: 2026-09-17 (two refine passes, then an order run the same day).

**What the 2026-09-17 passes settled**, all of it written into the sections below: the protein floor moved to **45g**; carbs are **wanted but light**; the vegetable rule is about **cheapness, not vegetables**, with every free premium vegetable taken, cheap greens and **corn** and **broccoli** skipped; full portions get bought and under-eaten; spicy Indian and poke are standing yeses at the top of any heat scale; soup only when it is unique; no drinks or desserts ever; take the add-on whenever the budget allows; the **spend floor is retired**; there is **no calorie floor**; the schedule is whatever the site shows; and **a 4 plus a re-order is an anchor that beats novelty**, which is what GRECO is. A late rating that same day moved the GRECO 09-17 plate from a `3` to a **`4 loved`** — the site owns the star — which recounted Mediterranean to **2.90**, `protein-plate` to **3.57**, the rated-lunch average to **3.26**, and made GRECO **two visits, two 4s**. Nothing `stated` changed.

## Provenance, and why it matters

Every rule below is tagged. The tag decides whether the skill may change it on its own.

| Tag | Means | May the skill revise it? |
|---|---|---|
| `stated` | the user said it, in conversation or in the onboarding interview | **No.** Only the user changes a `stated` rule. Contradicting evidence gets raised with them, not applied. |
| `derived` | inferred from ratings, reviews and order patterns | **Yes.** Revise it whenever the log stops supporting it. Say so in the writeup. |
| `provisional` | derived from too little data to trust yet | Yes, and actively look for evidence either way. Promote to `derived` or drop it. |

A `derived` rule that contradicts a `stated` one loses. If the log keeps arguing with something they said, **say so once and keep obeying the stated rule.**

## Hard limits

Gates. Nothing overrides these — not price, not macros, not rotation, not a loved rating.

| Limit | Tag | Note |
|---|---|---|
| **No allergies declared** | `stated` | Nothing is medically off-limits. If this ever changes it outranks every other line in this file. |
| **Tofu is never the protein** | `derived` | The one tofu bowl is the only 1-star in 40 orders. Tofu as an incidental component is fine; tofu carrying the protein number is not. |
| **No cheap-filler vegetable base** | `stated`, refined 2026-09-17 | **The line is quality, not vegetables.** "a bed of lettuce is low quality vegetables. cheap. but a bed of cucumber salad is not cheap." See **Where the vegetable line actually falls** below. |
| **No drinks, no desserts** | `stated` 2026-09-17 | *"i dont order drinks or desserts, save that money for protons."* Never add either — not to close a price gap, not as a treat, not at any price. |
| **Nothing on the Never Again list** | `stated` | Lives in `order-history.md`. One entry: the Life Alive Teriyaki Tofu Bowl. |
| **Nothing they rated 1 or 0** | `derived` | A rating of 1 retires the dish. A 0 retires the dish and puts the kitchen under suspicion. |

### Where the vegetable line actually falls

`stated` 2026-09-17, asked where "nothing vegetable-forward" ends and "vegetables with real bulk" begins:

> "the line lies in quality — a bed of lettuce is low quality vegetables. cheap. but a bed of cucumber salad is not cheap."

**So the gate is cheapness, not vegetables.** Shredded lettuce is the cheapest thing a kitchen can put under a protein, and it displaces food that cost something.

| Fine as a base | Not a base |
|---|---|
| cucumber salad, tomato and onion, fattoush, tabbouleh | shredded iceberg or romaine under anything |
| cabbage slaw, pickled vegetables, roasted vegetables | spring mix used as volume |
| beans, legumes, quinoa salad | raw greens as the majority of the container |

Same axis as the `expensive` complaint at Sake and the "basic sandwich" at Roots to Rise. **They are not counting calories on the vegetable, they are counting what the kitchen spent.** Practical test before ranking a salad or a bowl: **if the base were removed, would the rest still look like lunch?**

### Free premium vegetables are always taken. All of them.

`stated` 2026-09-17, and it is the active half of the rule above:

> "if extra *premium* vegetables are free, we get extra premium vegetables. we skip cheap salad and leave it for the rabbits. on bowls like mexican or poke, we get *all* the vegetables"

**The cheapness gate was never an instruction to avoid vegetables — this is what it was protecting.** A free premium vegetable is the best trade on any menu: real bulk, real flavour, no calories worth counting, no dollars taken from the protein.

| Take every one that is free | Leave it |
|---|---|
| avocado, grilled or roasted vegetables, mushrooms, peppers, onions | shredded iceberg or romaine |
| edamame, seaweed salad, beans, artichoke, roasted red pepper | spring mix |
| pickled vegetables, kimchi, cabbage slaw, radish, jalapeño, olives | plain lettuce in any role |
| cucumber, tomato, red onion, scallion, cilantro, mango | **corn** and **broccoli** — see below |

**On a build-your-own bowl, take all of them.** `stated`, and they named the two formats: **Mexican and poke**. Those modals list eight or twelve free checkboxes and there is no reason to leave one unticked. The same applies to shawarma plates, grain bowls, burrito bowls and build-your-own salads.

**Corn is out, free or not.** `stated` 2026-09-17: *"we skip corn when we can. corn is a grain disguised as a vegetable. very sneak corn, but it won't work."*

**So corn is scored as a grain**, under the light-carb rule rather than this one, and **gets skipped wherever the modal lets you** — "when we can" is the honest limit, since it comes baked into some salsas and pre-mixed sides. The rule generalizes: **starchy things wearing a vegetable label get scored as starch.** Peas, potato, plantain and sweet potato belong to **Base and side choice**.

**Broccoli is out too, and for a different reason.** `stated` 2026-09-17: *"broccoli bowl is still out, we skip broccoli too."* No reclassification and no argument behind it — they do not want it, which outranks anything this file could derive. **Skip it wherever the modal allows, and a bowl built on broccoli is not a candidate at all**, whatever it does for the macros.

**Do not extend this to anything else.** **Cabbage slaw, kimchi and pickled vegetables stay on the take-everything list**, and cauliflower, brussels sprouts and broccolini stay untested rather than assumed out. Widening a named dislike into a category is the mistake the Mediterranean entry is in this file to prevent.

Three things this does not license:

- **A vegetable that costs money competes with protein, and protein wins.** `derived` — they said *free*. A paid vegetable add-on only goes in when the build already clears 45g with room under the ceiling. Avocado at $1.50 loses to an egg or an extra scoop of meat every time.
- **Vegetables never become the base.** Taking all the peppers does not make a pepper bowl into lunch.
- **They still get counted** in the macro estimate. Most are 10-30 calories and vanish; **avocado at ~80 cal a scoop is the one that actually moves a total** against the 800 ceiling.

**Select them in the modal, never in the notes box.** They are structured selectors, so asking in writing restates a choice the ticket already carries.

## The schedule

`stated` 2026-09-17, asked whether this is a one-day-a-week program given that 29 of 33 delivery weeks in the log carried exactly one order:

> "historically the schedule was every tuesday. for the next few months they added thursday. a few times per year it's on monday's, too. if it's available on the site, it's available to feed me."

**The last line is the rule: if the site shows a day, it gets an order.** No day is skipped on principle, no weekday is "not their day," and one order is not the stopping point just because the history shows one.

| Day | Expect it |
|---|---|
| **Tuesday** | always — 33 of the 41 delivered orders |
| **Thursday** | **added for the next few months from 2026-09-17** |
| **Monday** | a few times a year |
| anything else | the site decides, not this file |

**So expect two-day weeks to be the normal case for a while**, and plan the batch whole across both — see **Variety and rotation** in `SKILL.md`.

**Never infer the schedule from this table.** It records what they said to expect; the site records what is actually open. Enumerate the days on `/schedule` every run and order every one of them.

## Calories

**Ceiling: 800 calories**, estimated, for the build as ordered. `stated` 2026-09-16.

A gate, not a goal. An item over 800 does not go on the slate at rank 1 and does not get placed, however well it scores on protein, price or format. It routinely binds before the budget does, so expect builds to leave stipend unspent.

**No calorie floor, and the question is settled.** `stated` 2026-09-17:

> "calorie floor is dictated by the protein floor. 45g of protein is 45x4 calories"

**So the only calorie minimum is the one the protein floor implies.** A build that clears 45g at 600 calories is a better order than one that clears it at 790. **Satiety is fixed by composition, never by a calorie target.**

### Satiety is a separate problem from calories

**A build can clear every gate and still leave them hungry.** `stated` 2026-09-17, on the GRECO plate: ~50g protein and ~650 calories as actually eaten, 13g over the floor and 150 under the ceiling, and the report was "still a little hungry." So **within the calorie budget, prefer the components that take up the most room**, because volume is what ends a meal:

- **Beans, legumes and high-fiber sides** over rice, fries or bread for the same calories — also a stated flavour preference, see **Base and side choice**.
- **Vegetables with real bulk** over dressings, oils and creamy sauces, which spend the budget without filling anything. Lettuce volume is not satiety, it is filler.
- **A second protein-forward component** over a larger starch, when the menu offers the choice.

**Do not respond to this by raising the ceiling.** The ceiling is `stated`. Fix satiety by changing what fills the budget, not by enlarging it.

## Macros

**Protein floor: 45g**, estimated. `stated` 2026-09-17, raised from 40g.

The floor that never bends. 43g is not close enough. Among options that clear it, more protein is not better — once 45g is met, calories and their ratings decide.

**Why it moved.** The 2026-09-17 GRECO plate cleared the old floor by 13g and still came up short on food. Expect the paid protein add-on — an egg, a double, a swap — to be how most builds reach 45g.

**The floor is best-effort on a sushi day, and only there.** `stated` 2026-09-17:

> "sushi is great, i love sushi. but i am often still hungry on sushi day because it's expensive. on these days we do our best to hit the 45g floor."

**Sushi is not dropped and the floor is not waived.** Poke bowls reach 45g with a paid protein add-on and should always take one. Sushi rolls mostly cannot reach it under the ceiling — build the highest-protein combination the menu allows, take nigiri over rolls where both exist, and **say in the writeup how far short it landed.**

**And offer the alternative rather than deciding for them.** Their own phrasing of what they want to see:

> "maybe give the user options like 'yo. you love sushi, and it's on the menu. but i found this as well that would be more filling'"

**The address in that example is not the register.** `stated` 2026-09-18: no gendered term for the user anywhere. The original phrasing carried one and it is struck from the quote. See **Output style** in `SKILL.md`.

So on any day where sushi is available and cannot reach 45g, **the slate presents both** and **asks which one they want, in Claude's normal voice, outside the greentext.** Do not silently rank the filling option first and bury the sushi; the hunger is a known, accepted trade.

**This is the `Cheat day` case and sushi is the standing instance of it** — see **Cheat day** in `SKILL.md` for the general rule. Two favourites qualify: **Indian** outright because it is `stated`, and **raw fish as a `derived` favourite** off the log alone — five rated orders, three 4s, nothing under a 3. Indian clears the floor without help, so it never collides. Sushi collides almost every time.

**The open cheat-day question, for the next refine.** Whether sushi should simply be *ordered* when it collides with the 45g floor rather than asked about day by day. Until that is answered the per-day ask above stands.

**Protein is bought, not asked for.** `stated` 2026-09-16. It is the costed component, so it gets purchased in the option modal where the upcharge shows in the cart. **A note's protein never counts toward the 45g floor; a note's calories always count against the 800 ceiling.**

**One protein bought well beats two bought badly.** `derived`. When a build stacks two paid proteins, expect the cheaper one to arrive in quantity and the premium one to be short.

**A carb is always wanted. A light one.** `stated` 2026-09-17, correcting the "actively reducing carbs" reading this file carried for a day: *"i always want some sort of carb, just light carb. a side of flatbread, beans, quinoa salad, brown rice, etc."*

**Every build should carry a carb component, and it should be a side rather than the base.** The half-pita left at GRECO was portion control on a carb they wanted there, not a rejection of the pita. Their own examples are the spec: **flatbread, beans, quinoa salad, brown rice** — and beans count as the carb *and* the fibre *and* part of the protein, which is why the bean swap wins on every axis at once.

So the `carb-base` rationing below governs the **format**, not the presence of starch. A `protein-plate` with a quinoa side is exactly right; a rice bowl where rice is the meal is the thing being rationed.

**Fibre is wanted, for satiety and on its own terms.** `stated` 2026-09-17, cited as a reason beans beat rice. No number.

**Portion control is their job, not the order's.** `stated` 2026-09-17: *"i prefer to get the food and just eat less of it versus paying regular price for less food, or all lettuce."* See **Buy the full plate** under **Portion and value**.

No stated targets for fat or sodium as numbers, but see the topping rule under **Components**. Do not invent targets that were never given.

## Cuisines

Ranked by their own ratings across 40 orders. `derived` — revise this table whenever a scrape moves an average.

| Cuisine | Rated orders | Avg | Read |
|---|---|---|---|
| Indian | 2 | **4.00** | **Take it whenever a day offers it.** `stated` — see below. |
| East / Southeast Asian | 13 | **3.38** | Eight of the eighteen 4s. The deepest reliable bench. **3.58 with the Life Alive tofu bowl set aside.** |
| Latin | 3 | 3.00 | Wide spread — a 4, a 3 and a 2. |
| Mediterranean / Middle Eastern | 10 | **2.90** | Still the weakest repeated cuisine. **2.63 with GRECO set aside** — and that gap is the whole point. |

**Spicy Indian is a standing yes.** `stated` 2026-09-17: *"spicy indian is always a good choice, yes."* This promotes a two-order pattern into a rule the log could not have justified on its own — Tabla and Nirvana both rated 4, WOW TIKKA went unrated, Halal Indian was cancelled on a day they were not in the office. **When an Indian kitchen is on a day's list, it is the default pick for that day**, subject only to the hard gates and the format rotation. Order it at the highest heat offered.

**Poke is a standing yes alongside it.** `stated` 2026-09-17: *"i know the poke bowl is always a great choice, can give that a 4/4."* That settled the unrated 2026-01-27 Atlantic Poké bowl at **4 loved** and made poke **two orders, two 4s**. Raw fish overall is **five rated orders averaging 3.60**, the best record in the log after Indian.

**Poke is also the raw-fish format that can reach the protein floor**, because every poke counter sells a second scoop or a grilled-protein add-on. **When a day offers poke, take it and buy the extra protein** — the one place where "they love it" and "it clears 45g" stop competing.

**The Mediterranean line is a correction.** This file used to claim Mediterranean builds land well, taken from components the user listed — tahini, hummus, feta, olives — and not from a single rating. Recounted 2026-09-17 and again when the GRECO star landed: **10 rated, averaging 2.90** — three 2s (Noor, Shy Bird, Cafe Landwer), five 3s, and **two 4s, both GRECO**. Set GRECO aside and the remaining eight average **2.63**, none above a 3, so the whole of the cuisine's improvement is one kitchen. The components are genuinely liked; the kitchens serving them on this program mostly hold the food too long. **Order the hummus; stop picking the restaurant because it has hummus; go to GRECO.**

**GRECO is the exception, and it cooks to order.** Two visits, two 4s, the only kitchen in 42 orders with that record — and the return was chosen against a live slate and a stated preference for novelty, which is evidence a star click cannot carry. That makes GRECO **the only anchor in the log** — see **Two signals to return** in `SKILL.md`. **Treat it as separate from the cuisine average.**

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

**Six lunches, average 3.50** — higher than every row in the table except Indian. American BBQ, a pizza counter, a jerk counter and a Brazilian egg bowl have no cuisine in common, so **do not read this as "American rates well."** What they share is the axis already in **Preparation**: the four 4s were all cooked hot to order, and the 3 and the 2 were both assembly-counter food. **Cuisine is the proxy; hot-versus-held is the thing.**

Practical rule: when a day's menus offer an Indian or Asian kitchen alongside a shawarma counter, **the shawarma counter is the weaker bet even when its build looks better on paper.**

### Novelty is the default. GRECO is the exception.

`stated` 2026-09-17: *"i prefer novelty but greco is good."*

This settles a gap the log could not: sixteen kitchens have rated 4 and only GRECO was ever ordered from twice, and the reason is preference rather than the program rotating them out of reach. So **a 4 on its own is permission, not instruction.** Between an untested kitchen that clears the gates and a `loved` one that also clears them, **take the untested one** and say that novelty broke the tie.

**A second signal changes that.** `stated` 2026-09-17: *"a re-order is signal and also a perfect rating is a signal — both to reorder again from the same restaurant."* A kitchen that earned a 4 **and** was chosen again is an **anchor**, the one thing that beats novelty on a tie. The preference for novelty is what makes a repeat worth reading: going back cost them something new and they did it anyway.

The full ranking lives in **Two signals to return** in `SKILL.md`. Here, the order of precedence:

- **A hard gate.** An untested kitchen that cannot reach 45g protein under 800 calories does not get picked to be interesting. Neither does an anchor.
- **An anchor.** Currently GRECO alone. Repeat it without apology.
- **Novelty.** The default among everything that clears the gates.
- **A `loved` kitchen never returned to.** Welcome, but it loses a tie to something new.
- **A `neutral` or worse kitchen.** Novelty is never a reason to revisit something that already disappointed.

The honest read of the last nine months is that novelty has been the right call: the untested pick landed a 4 more often than not, and the failures clustered in untested *sandwich and wrap counters* rather than in untested kitchens generally. **Be new, be hot-cooked.**

## Components

The part that generalizes across restaurants. Apply to any menu, not just the one a preference came from.

### Wants — include when the menu offers them

`stated`. Confirmed by the log: hummus twice at The Chicken & Rice Guys, tzatziki and pepper paprika dip at GRECO, fattoush and pickled turnips at Aceituna.

tahini · hummus · feta · olives · tomato · onion · cucumber

**And every free premium vegetable the modal offers**, taken in full — see **Free premium vegetables are always taken** above, which outranks this list: it is not a set of favourites to look for, it is an instruction to tick every box that costs nothing.

**Spice and hot sauce. Hottest available.** `stated` 2026-09-17: "i doused the whole thing in hot sauce, as i do. i do love spicy food," and, asked whether to take the top heat level or one below, *"let's do hottest when available."*

A standing instruction, not a garnish note. **Where a menu offers a heat scale, pick the top of it** — Thai 5-star, vindaloo, extra-spicy, hot rather than medium. Take the free chili or hot-sauce option whenever the modal offers it, and ask for hot sauce on the side in the notes box when it does not, where it is usually the one ask that box gets. Nothing in the log shows a heat level being too much, and the spiciest things they have eaten — Pad Krapow, jerk chicken, gochujang, vindaloo, spicy basil — are where the 4s are.

Read this list as components to add to a build, **not as a reason to choose a restaurant.**

### One ask in the notes box

`stated` 2026-09-17, on a Cosi note reading *"Extra pickled jalapeño, please, and hot sauce on the side if you have it"*:

> "let's keep any special requests to one. 'extra pickled jalapenos' and 'hot sauce on the side' is two requests"

**Every note carries exactly one ask**, measured by the action rather than the ingredient: "heavy on the feta and olives" is one motion at one station, "extra jalapeño and hot sauce on the side" is two. The ranking that decides which survives lives in **Modifications** in `SKILL.md`.

**The one-ask limit is about the free-text box only.** Everything structured still goes in the modal, where it costs nothing to be thorough — every free premium vegetable, every heat option, the paid protein.

### Toppings and sauces: protein up, fat down

`stated` 2026-09-17, on the GRECO tzatziki: it was good, "but a higher protein lower fat topping is always preferred."

**Rank the sauce and topping selectors on that axis**, not on flavour alone:

| Prefer | Over |
|---|---|
| yogurt-forward, lean, or salsa/vinegar-based | oil-forward, mayo-based, creamy or buttery |
| hummus, tzatziki, lean whipped feta, chutney, salsa, hot sauce | aioli, ranch, tahini-heavy dressings, oil-based vinaigrettes |
| a protein-bearing topping (cheese, egg, beans, yogurt) | a fat-bearing one that adds calories and no protein |

Tzatziki stays acceptable and was liked; it is simply not the best answer when something leaner is on the same menu. **Hot sauce is the ideal case**: all flavour, no calories, no fat, and a creamy dressing spends the calorie budget without filling anything.

### Eggs as a protein lever

`provisional` 2026-09-17 — two orders, both rated 4. Saigon Tiger with two fried eggs at $2.00 each, and the Bom Dough Egg & Greens Bowl with egg as the entree protein.

Eggs are the cheapest protein-per-dollar add-on in the log, carry no starch, and are bought in the modal. **Take the egg add-on wherever a menu offers one**, and treat an egg-based entree as a real lunch.

### Wants less of

- **Lettuce.** `stated`. Filler that displaces real food. Order salads light-lettuce or no-lettuce wherever mods are accepted, and never count lettuce volume as part of the meal.

### Rationed, not disliked

- **Starch bases** — wrap, sandwich, sub, burrito, pizza, rice bowl, grain bowl. `stated`: "not every week." Enforced by the `carb-base` cooldown in `SKILL.md`.

  **This is a rationing rule, not a dislike.** `carb-base` is **27 of 41 orders** and carries **13 of the 18 fours**, 12 with the Levain side order set aside. **It is also the only format that has ever failed**: all six 2s and the single 1 are `carb-base`, and no `salad`, `protein-plate` or `soup-forward` order has ever rated below 3.

| Format | Rated | Avg | Worst | Best |
|---|---|---|---|---|
| `soup-forward` | 1 | **4.00** | 4 | 4 |
| `protein-plate` | 7 | **3.57** | 3 | 4 |
| `carb-base` | 23 | 3.17 | **1** | 4 |
| `salad` | 3 | 3.00 | 3 | 3 |
| `bowl-no-grain` | **0** | — | — | — |

  **That table counts lunches only.** The $4.13 Levain chips-and-roll is a `carb-base` side order and is excluded; fold it back in and `carb-base` reads 24 orders at 3.21.

  **So `carb-base` is the high-variance bet, not the bad one**, which sharpens the cooldown into a real rule: when a `carb-base` slot is spent, spend it on a kitchen with a rating behind it, never on an untested sandwich or wrap counter — where all seven failures came from.

  **Soup counts only when the soup is interesting.** `stated` 2026-09-17: *"soup is an ok option if it's a unique soup. nothing boring."* Kao soi, momo jhol, pho, tonkotsu, a real tom yum — yes. Chicken noodle, minestrone, a soup-and-half-sandwich box — no, and a boring soup does not get picked just because rotation wants the format. **`soup-forward` is a permission with a taste filter on it**, which is why it sits at one order in nine months despite rating 4.

  **The two formats at the top of that table are the two least used.** `derived` 2026-09-17. **`bowl-no-grain` went 0 for 41 and is now on order** — the Cosi Adobo bowl on cauliflower rice, delivering 2026-09-22, and its rating is the most informative one this log can collect next. It is the exact shape of three rules this file carries: carbs down, beans first, satiety from bulk rather than starch. **An absence in a log is not a preference.**

  **And `salad` is the flat one.** Three orders, all rated exactly 3. It never fails and never delights, which makes it the right pick only when rotation demands a format and nothing better is open.

### Base and side choice

**Beans first.** `stated` 2026-09-17: "the beans were a nice sub for rice, i often do this myself outside of work meals. beans > rice — or beans & rice is also good. extra fiber and protons in the beans." A habit rather than a reaction to one plate. Order of preference when a base or starch side is chosen:

1. **Beans or legumes** — gigantes, pinto, black, chickpea, lentil. Best on flavour, fibre, protein and satiety at once.
2. **Beans and rice together**, explicitly fine, and better than rice alone.
3. **Quinoa**, then **brown rice**. `stated` 2026-09-09.
4. **White rice**, acceptable rather than a reason to drop the item — a white-rice build rated 4 at Basil Rice — but say in the writeup that it was not their pick.

Confirmed in the log: brown rice at Mae Asian Eatery, Boloco and Atlantic Poké; brown rice & red quinoa at WOW TIKKA; lemon-dill beans at GRECO, which drew the review that set this rule. **Where a menu lets a bean side replace a grain, take it**, even at a small upcharge.

## Preparation

`derived` from the 4s versus the 2s. This is the axis the macro arithmetic cannot see, and it is where their written reviews all land.

**Predicts a good rating:**

- Cooked to order and served hot.
- Raw fish. Sushi and poke cannot be dried out or held badly — every raw-fish order rated 3 or 4. **Five rated orders averaging 3.60** (Umai 4, Poke City 4, Atlantic Poké 4, Sake 3, Meet Us 3), the best record in the log behind Indian. **Poke is 2 for 2 at 4 and a `stated` standing yes.** On the 45g floor, see **Macros**.
- Aggressive seasoning: curry, jerk, gochujang, holy basil, harissa. **Now `stated` as well as derived** — see **Wants**.
- A cuisine-specific kitchen over a generic bowl-and-sandwich chain.
- **Grilled over fried** (`stated`) — against a breaded cutlet, specify grilled.

**Predicts a bad rating:**

- Cold assembly — deli sandwiches, wraps, pre-built boxes.
- Pita and shawarma held between cook and delivery.
- Chain burrito-bowl formats.
- Tofu in any load-bearing role.

**Freshness is worth a note line.** `derived`. Two of their six written reviews are about food held too long. At a pita, shawarma or wrap counter, a short "fresh off the grill if you can, please" is a reduction-tier ask that costs the kitchen nothing and targets the failure mode that produced both 2s.

## Portion and value

- **One real menu item**, plus at most one add-on or upgrade. `stated`. A salad with a meat add-on is fine; three à-la-carte sides stacked into a fake entree is not.

  **Take the add-on whenever the budget allows.** `stated` 2026-09-17, asked whether the one-item rule still holds given they broke it themselves at Santa Fe Burrito Grill: *"yes we can always do an addon if the budget allows."*

  **So the add-on is the default, not the exception**: a build that leaves room under the ceiling and takes no upgrade is under-built, and that room should have gone to protein. What they did *not* license is stacking several entrees.

- **Buy the full plate and eat less of it.** `stated` 2026-09-17: *"i prefer to get the food and just eat less of it versus paying regular price for less food, or all lettuce."*

  **This is a purchasing rule and it cuts against the instinct to order light.** Given a full-size item and a smaller one at a similar price, take the full-size one; given a plate with a starch side and the same plate without, take the one with the side. Leftover food is a choice they make at the desk. Two things it rules out:

  - **Never downsize to hit the calorie ceiling.** Change what is on the plate, not how much of it there is.
  - **Never substitute greens for a component to save calories.** That is the "all lettuce" case by name — paying full price for filler.

  It also resolves the GRECO half-pita: they wanted the pita on the plate and ate half of it, so the correct build still includes the pita.

- **Value is part of the verdict.** `derived`. A $17.95 build assembled out of $1.00 add-ons earned "expensive" at a 3. Reaching the ceiling through many small upcharges reads as padding even when the arithmetic is right.

- **Price does not predict the verdict, and the correlation runs backwards.** `derived` 2026-09-17, recounted with the two side orders excluded — the $4.13 Levain chips-and-roll and the $4.00 Bom Dough bacon are not lunches and were inflating every price statistic here.

  | Rating | Rated lunches | Avg subtotal |
  |---|---|---|
  | **4** | 17 | **$17.55** |
  | 3 | 10 | $17.77 |
  | 2 | 6 | $17.87 |
  | 1 | 1 | $17.95 |

  **A clean inverse ladder, monotonic at every step**, and the GRECO recount tightened it. The 34 rated lunches average **3.26**. Orders at or above $18.00 average **3.29** across 14; orders under $17.00 average **3.33** across 6. The cheapest 4 is Perillas at **$14.59**; the most expensive order is Roots to Rise at $18.88, rated 2.

  The sample is small and the spread is narrow, so this is not *cheaper is better*; it is **spending more has never once bought better food.**

- **The spend floor is retired.** `stated` 2026-09-17, asked whether to keep $16.85 as a target:

  > "we just dont want to spend extra money. they're buying me lunch, not the other way around."

  **There is one money rule and it is $0.00 out of pocket.** The $18.68 ceiling stays exactly as it is, as a hard gate against a card prompt. **$16.85 is no longer a target, a floor, or a thing to report as missed.** A build that clears the dietary gates at $14.59 is a complete order. What this kills outright:

  - **Padding.** Never add anything to a build to lift a subtotal.
  - **"Unspent stipend" as a cost.** Do not report it as one. The stipend is the program's to offer, not the user's to maximise.
  - **Ranking on spend.** Two builds that both clear the gates are separated by their ratings and the rotation, never by which costs more.

  **What survives is the add-on rule above**, and the two are not in tension: the budget buys *protein* when protein is what the build needs, not dollars for their own sake.

## How this file evolves

**This file is meant to change.** After any run that reads new reviews, review it against what the log now says. Evidence that never updates the rules is evidence wasted.

- **A `derived` rule the log no longer supports** — revise or delete it, and say so in the writeup. The Mediterranean correction is the worked example: a belief held on stated components, overturned by nine ratings.
- **A pattern across three or more rated orders** — write it in as `derived`, naming the orders.
- **A pattern across one or two orders** — `provisional`, or leave it out. Do not harden a rule on a single meal.
- **A `stated` rule the log contradicts** — do not touch it. Raise it once, with the ratings that disagree. They decide.
- **Anything they say in conversation** — `stated`, dated, in their words. Stated beats derived on the same subject.
- **A reason given when cancelling an order** — `stated` 2026-09-17: *"if the user gives a reason why they want to cancel and re-order, that's a strong signal."* Their words about a specific build, given before anyone ate, which makes it cleaner evidence than a review tangled up with how one kitchen cooked that day. **Sort it first**: a reason about the day is transient and changes no rule, while a reason about the food is a preference and goes in here as `stated`. When unclear, treat it as about the day and say so. See **Cancel and swap** in `SKILL.md`.
- **A new allergy or medical limit** — straight into **Hard limits** as `stated`, outranking everything else in the file.

Keep the tags current. An untagged rule is one nobody can safely revise, which is how the Mediterranean claim survived nine contradicting orders. Update `Last reviewed` when the file is checked against a fresh scrape, even when nothing changes — a date that has not moved in months means the ratings are not being read.

**This file is personal data, and that is what it is for.** Write allergies and medical limits in full — how severe, what triggers it, what happens. A gate reading "shellfish, anaphylaxis, includes shared fryers" gets enforced more carefully than one reading "no shellfish." Never soften a safety line to make the file easier to share.

Whether this file ever leaves the machine is the user's decision, not the skill's. See **The data files are personal** in `SKILL.md`.

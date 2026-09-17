# Dietary Preferences

**This file is the source of truth for what the user will and will not eat.** `SKILL.md` holds the process; `order-history.md` holds the evidence; this file holds the rules those two are serving.

The skill reads this file on load. If it is missing, the skill does not order — it runs the onboarding interview in **First run** in `SKILL.md` and writes this file from the answers.

Last reviewed: 2026-09-17. Nine answers came back from the refine pass and every one is written in below as `stated`. The big ones: the protein floor is **45g**, not 40g; carbs are **wanted but light** rather than being reduced; the vegetable limit is about **cheapness, not vegetables**; full portions get bought and under-eaten rather than ordered small; spicy Indian is a standing yes; spice goes to the **top of any heat scale**; novelty is the default but **a 4 plus a re-order is an anchor that beats it**, which is what GRECO is; the GRECO plate itself settled at **3**, which reads as what to order there rather than whether. Cancelled days are usually him not being in the office and mean nothing about the food.

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
| **Nothing on the Never Again list** | `stated` | Lives in `order-history.md`. Currently empty. |
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

## Calories

**Ceiling: 800 calories**, estimated, for the build as ordered. `stated` 2026-09-16.

A gate, not a goal. An item over 800 does not go on the slate at rank 1 and does not get placed, however well it scores on protein, price or format.

It routinely binds before the budget does, so expect builds to land under the spend ceiling and leave stipend unspent. That is the gate working. **Never add a side, a drink or a dessert to close a price gap if it pushes the build over 800.**

No floor. There is no minimum calorie count — a build that clears the protein floor at 600 calories is a better order than one that clears it at 790.

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
| East / Southeast Asian | 12 | **3.33** | Seven of the sixteen 4s. The deepest reliable bench. |
| Latin | 3 | 3.00 | Wide spread — a 4, a 3 and a 2. |
| Mediterranean / Middle Eastern | 10 | **2.80** | The weakest repeated cuisine. **2.63 with GRECO set aside.** |

**Spicy Indian is a standing yes.** `stated` 2026-09-17: *"spicy indian is always a good choice, yes."* This promotes a two-order pattern into a rule the log could not have justified on its own — Tabla and Nirvana both rated 4, WOW TIKKA went unrated, and Halal Indian was cancelled on a day he was not in the office. **When an Indian kitchen is on a day's list, it is the default pick for that day**, subject only to the hard gates and the format rotation. Order it at the highest heat offered.

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

**East/SE Asian's 3.33 includes the Life Alive tofu bowl**, the only 1 in the log. The eleven cuisine-native Asian kitchens average **3.55**. The row stays as counted, but the bench is stronger than the number reads.

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

  **This is a rationing rule, not a dislike, and the difference matters.** `carb-base` is **27 of 41 orders** and carries **12 of the 16 fours** — 11 if the Levain side order is set aside. Corrected 2026-09-17: this file previously said nine, which understated the format's record by a third.

  **It is also the only format that has ever failed.** All six 2s and the single 1 are `carb-base`. No `salad`, `protein-plate` or `soup-forward` order has ever rated below 3.

| Format | Rated | Avg | Worst | Best |
|---|---|---|---|---|
| `soup-forward` | 1 | **4.00** | 4 | 4 |
| `protein-plate` | 7 | **3.43** | 3 | 4 |
| `carb-base` | 23 | 3.17 | **1** | 4 |
| `salad` | 3 | 3.00 | 3 | 3 |
| `bowl-no-grain` | **0** | — | — | — |

  **So `carb-base` is the high-variance bet, not the bad one.** It holds the best food in the log and every disappointment in it. That sharpens the cooldown into a real rule: when a `carb-base` slot is spent, spend it on a kitchen with a rating behind it — never on an untested sandwich or wrap counter, which is where all seven failures came from.

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
- Raw fish. Sushi and poke cannot be dried out or held badly — every raw-fish order rated 3 or 4.
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

  Noted tension: the user broke this himself at Santa Fe Burrito Grill — three tacos plus a bowl — and rated it 4. The rule stands because he stated it; the exception is logged so it is not treated as a discovery.

- **Buy the full plate and eat less of it.** `stated` 2026-09-17: *"i prefer to get the food and just eat less of it versus paying regular price for less food, or all lettuce."*

  **This is a purchasing rule and it cuts against the instinct to order light.** Given a full-size item and a smaller one at a similar price, take the full-size one. Given a plate with a starch side and the same plate without, take the one with the side. Leftover food on the plate is a choice he makes at the desk; it is never a reason to buy less at the menu.

  Two things it rules out, both of which this skill would otherwise have drifted toward:

  - **Never downsize to hit the calorie ceiling.** Change what is on the plate, not how much of it there is. A smaller portion of the same thing is the worst available answer, because it costs the same and delivers less.
  - **Never substitute greens for a component to save calories.** That is the "all lettuce" case by name — paying full price for filler.

  It also resolves the GRECO half-pita. He wanted the pita on the plate and ate half of it. The correct build still includes the pita.

- **Padding to reach the spend floor is weak.** `stated`. Adding a $1.50 banana purely to clear the floor is acceptable but poor; prefer one item that lands in the window on its own.

- **Value is part of the verdict.** `derived`. A $17.95 build assembled out of $1.00 add-ons earned "expensive" at a 3. Reaching the ceiling through many small upcharges reads as padding even when the arithmetic is right.

- **Price does not predict the verdict.** `derived` 2026-09-17. Orders at or above $18.00 average **3.23** across 13 rated. Orders under $17.00 average **3.43** across 7. The whole log averages **3.24**. The cheapest 4 in it is Perillas at **$14.59**; the most expensive order in it is Roots to Rise at $18.88, rated 2.

  **So the spend floor is a stipend-usage goal and nothing else.** It has never once bought better food. Spending toward $18.68 is worth doing when the candidates are otherwise equal, and **never worth dropping a better-rated item to reach $16.85.**

## How this file evolves

**This file is meant to change.** Every scrape of their ratings is new evidence, and evidence that never updates the rules is evidence wasted. After any run that reads new reviews, review this file against what the log now says.

What to do with new evidence:

- **A `derived` rule the log no longer supports** — revise or delete it, and say so in the writeup. The Mediterranean correction is the worked example: a belief held on stated components, overturned by nine ratings.
- **A pattern across three or more rated orders** — write it in as `derived`. Name the orders it came from.
- **A pattern across one or two orders** — write it in as `provisional`, or leave it out. Do not harden a rule on a single meal.
- **A `stated` rule the log contradicts** — do not touch it. Raise it with them once, in the writeup, with the ratings that disagree. They decide.
- **Anything they say in conversation** — write it in as `stated`, dated, in their words where possible. Stated beats derived on the same subject.
- **A new allergy or medical limit** — goes straight into **Hard limits** as `stated`, and outranks everything else in the file.

Keep the tags current. An untagged rule is one nobody can safely revise, which is how the Mediterranean claim survived nine contradicting orders.

Update `Last reviewed` at the top when the file is checked against a fresh scrape, even when nothing changes. A date that has not moved in months means the ratings are not being read.

**This file is personal data, and that is what it is for.** Write allergies and medical limits in full — how severe, what triggers it, what happens. A gate that reads "shellfish, anaphylaxis, includes shared fryers" gets enforced more carefully than one reading "no shellfish," and enforcement is the whole reason the line exists. Never soften a safety line to make the file easier to share.

Whether this file ever leaves the machine is the user's decision, not the skill's. See **The data files are personal** in `SKILL.md`: the skill does not push, does not add remotes, and does not publish anything on its own initiative.

# Dietary Preferences

**This file is the source of truth for what the user will and will not eat.** `SKILL.md` holds the process; `order-history.md` holds the evidence; this file holds the rules those two are serving.

The skill reads this file on load. If it is missing, the skill does not order — it runs the onboarding interview in **First run** in `SKILL.md` and writes this file from the answers.

Last reviewed: 2026-09-17, against 40 rated orders.

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
| **Nothing vegetable-forward** | `stated` | No dish whose base is a vegetable pile with protein sprinkled on top. No broccoli-base bowls. |
| **Nothing on the Never Again list** | `stated` | Lives in `order-history.md`. Currently empty. |
| **Nothing they rated 1 or 0** | `derived` | A rating of 1 retires the dish. A 0 retires the dish and puts the kitchen under suspicion. |

## Calories

**Ceiling: 800 calories**, estimated, for the build as ordered. `stated` 2026-09-16.

A gate, not a goal. An item over 800 does not go on the slate at rank 1 and does not get placed, however well it scores on protein, price or format.

It routinely binds before the budget does, so expect builds to land under the spend ceiling and leave stipend unspent. That is the gate working. **Never add a side, a drink or a dessert to close a price gap if it pushes the build over 800.**

No floor. There is no minimum calorie count — a build that clears the protein floor at 600 calories is a better order than one that clears it at 790.

## Macros

**Protein floor: 40g**, estimated. `stated`.

The floor that never bends. 38g is not close enough. Among options that clear it, more protein is not better — once 40g is met, calories and their ratings decide.

**Protein is bought, not asked for.** `stated` 2026-09-16. It is the costed component, so it gets purchased in the option modal where the upcharge shows in the cart. A note asking for extra meat is ignored at most kitchens and reads badly at some. **A note's protein never counts toward the 40g floor; a note's calories always count against the 800 ceiling.**

**One protein bought well beats two bought badly.** `derived`. When a build stacks two paid proteins, expect the cheaper one to arrive in quantity and the premium one to be short.

No stated targets for carbs, fat, fiber or sodium. Do not invent them.

## Cuisines

Ranked by their own ratings across 40 orders. `derived` — revise this table whenever a scrape moves an average.

| Cuisine | Rated orders | Avg | Read |
|---|---|---|---|
| Indian | 2 | **4.00** | Never below 4. Small sample, strong signal. |
| East / Southeast Asian | 12 | **3.33** | Seven of the sixteen 4s. The deepest reliable bench. |
| Latin | 3 | 3.00 | Wide spread — a 4, a 3 and a 2. |
| Mediterranean / Middle Eastern | 9 | **2.78** | The weakest repeated cuisine. |

**The Mediterranean line is a correction, and it is worth understanding rather than just obeying.** This file used to claim Mediterranean builds land well. That came from components the user listed — tahini, hummus, feta, olives — and not from a single rating. Nine rated orders say otherwise: two 2s, both with freshness complaints attached, four 3s, and one 4.

The components are genuinely liked. The kitchens serving them on this program mostly hold the food too long. **Order the hummus; stop picking the restaurant because it has hummus.**

The one exception is **GRECO**, which cooks to order and rated 4.

Practical rule: when a day's menus offer an Indian or Asian kitchen alongside a shawarma counter, **the shawarma counter is the weaker bet even when its build looks better on paper.**

## Components

The part that generalizes across restaurants. Apply to any menu, not just the one a preference came from.

### Wants — include when the menu offers them

`stated`. Confirmed by the log: hummus twice at The Chicken & Rice Guys, tzatziki and pepper paprika dip at GRECO, fattoush and pickled turnips at Aceituna.

tahini · hummus · feta · olives · tomato · onion · cucumber

Read as components to add to a build, **not as a reason to choose a restaurant.** The cuisine table above is the restaurant-level signal; this list is the build-level one.

### Wants less of

- **Lettuce.** `stated`. The standing complaint — filler that displaces real food. Order salads light-lettuce or no-lettuce wherever mods are accepted, and never count lettuce volume as part of the meal.

### Rationed, not disliked

- **Starch bases** — wrap, sandwich, sub, burrito, pizza, rice bowl, grain bowl. `stated`: "not every week." Enforced by the `carb-base` cooldown in `SKILL.md`.

  **This is a rationing rule, not a dislike, and the difference matters.** `carb-base` is 26 of 40 orders and carries nine of the sixteen 4s. When the cooldown forces a non-`carb-base` pick, spend that slot on a kitchen they rated well rather than on a mediocre plate that merely fits the slot.

### Grain choice, when a grain is happening

`stated` 2026-09-09. **Quinoa first, brown rice second.** Both beat white rice and both beat leaving it to the default. Confirmed in the log — brown rice chosen at Mae Asian Eatery, Boloco and Atlantic Poké; brown rice & red quinoa at WOW TIKKA.

If neither is offered, white rice is acceptable rather than a reason to drop the item — a white-rice build rated 4 at Basil Rice — but say in the writeup that the grain was not their pick.

## Preparation

`derived` from the 4s versus the 2s. This is the axis the macro arithmetic cannot see, and it is where their written reviews all land.

**Predicts a good rating:**

- Cooked to order and served hot.
- Raw fish. Sushi and poke cannot be dried out or held badly — every raw-fish order rated 3 or 4.
- Aggressive seasoning: curry, jerk, gochujang, holy basil, harissa.
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

- **Padding to reach the spend floor is weak.** `stated`. Adding a $1.50 banana purely to clear the floor is acceptable but poor; prefer one item that lands in the window on its own.

- **Value is part of the verdict.** `derived`. A $17.95 build assembled out of $1.00 add-ons earned "expensive" at a 3. Reaching the ceiling through many small upcharges reads as padding even when the arithmetic is right.

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

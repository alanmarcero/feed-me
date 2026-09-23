# feed-me

```
> be me
> employer hands out $20 lunch stipend
> entire menu is bread, mayo, and rabbit food
> need maximal protons
> will not pay one (1) cent over. not ever.
> doing sales tax math in the checkout screen while starving
> mfw i realize i could just be super lazy
> make the ai order my protons
```

## three goals

everything else is plumbing.

| | | |
|---|---|---|
| **1** | **try new foods** | novelty is the point, not a tiebreaker. a new dish that clears the gates beats a known one that also clears them. a different restaurant serving the same build is not a new food |
| **2** | **hit macro goals** | the floors and ceilings in your preferences file. these are gates. goal 1 never breaks them |
| **3** | **be super lazy** | you do not want to be involved. every question is a cost. it acts, then reports |

goal 3 is why `/feed-me` orders instead of suggesting, never asks permission to look, syncs
itself, and answers in four lines of greentext instead of a report.

## what it does

| | |
|---|---|
| `/feed-me` | orders lunch for every open day, then syncs the log. the one you'll use |
| `/feed-me sync` | reconciles every order on the account. places nothing |
| `/feed-me refine` | mines your log against your preferences, asks better questions, sets cheat day |
| `/feed-me cancel <which>` | kills a pending order. add **and reorder** and it replaces it |

it opens the ordering site in a real browser, finds **every day** still open, reads every menu,
prices the upcharges hiding behind each option modal, plans the week so no two days land on the
same food, and places one order per day. not one lunch. all of them.

it still shows the ranked runners-up, because a nutritionist that won't show its work is a
vending machine with opinions. it'll also tell you about the perfect item sitting $0.04 over the
ceiling. so you can think about it. forever.

it answers in greentext, with the numbers exact. the costume comes off for the note it types
into the special-instructions box, and for anything it needs *you* to answer.

## the money

land on $20.01 and ezCater asks for your credit card like some kind of animal, over one (1)
cent. so: **a credit card is never entered.** ever. a card prompt is a bug report, so it backs
the order down until the total reads $0.00, and if nothing on that day's menu gets there it buys
nothing and says why.

the stipend doesn't pool or roll over — skip wednesday and wednesday's twenty dollars ceases to
exist — so it claims every open day off that day's own subsidy.

the catch: menu prices are pre-tax and the stipend is spent on the post-tax total. at 7% that
leaves about $18.68 of menu to play with. forty-one receipts agree.

## the rules

**the skill's, not up for discussion:** the ceiling is hard, a card is never entered, every open
day gets claimed, one (1) real item per order (three $5 sides in a trenchcoat is not an entree),
your reviews outrank the arithmetic, it never guesses your gender, and it never pushes to git on
its own.

**yours, and it just does what you said:**

| rule | ruling |
|---|---|
| allergies | HARD. the one gate a good-looking menu never argues with |
| a floor | a minimum to clear: protein, fiber, whatever. floors do not bend |
| a ceiling | a maximum to stay under: calories, carbs, sodium. bends only when nothing fits |
| cuisines, components, prep | what you want, what you'd rather skip, what to keep light |
| no rules | also fine. "just get me something good" is a complete answer |

## rotation

the failure mode of a robot with a calorie rule is that it finds the one lowest-calorie
qualifying salad and serves you that salad until you die. so every order is tagged with a
**format** — `salad`, `carb-base`, `protein-plate`, `bowl-no-grain`, `soup-forward` — and four
days ordered at once get four different ones.

## preferences

it looks for `dietary-preferences.md`. if it's not there, it does **not** hand you a form:

| what you've got | what it does |
|---|---|
| a preferences file | reads it, orders against it. best case |
| no file, but order history | infers preferences from what you rated, writes them down |
| neither | full sync first, then questions, then orders anyway |

**your order history is a preferences file nobody typed.** so the questions come off your log:
the one thing that doesn't add up, evidence attached, with the reading it would take so agreeing
is one word. five at most, allergies first. answer none and nothing breaks.

every rule is tagged:

| tag | means | can the skill change it? |
|---|---|---|
| `stated` | you said it | **no.** it can argue, it cannot overrule |
| `derived` | inferred from your ratings | yes, when the log stops backing it up |
| `provisional` | inferred from one or two orders | yes, and it hunts for more evidence |

## your reviews

you already rate orders on ezCater, so it reads the stars instead of asking twice. five stars,
stored 0-4:

| stars | verdict | what happens |
|---|---|---|
| 4 | **loved** | going back is allowed. go back twice and it's an anchor |
| 3 | liked | fine to repeat, not a priority |
| 2 | neutral | ranks *below* something untested |
| 1 | disliked | the dish is done |
| 0 | never again | dish done, kitchen suspect |

**a blank review is not missing data.** you write when something's wrong and say nothing when
it's right. and none of the complaints are about macros. "dry". "bland". "barely any steak".
execution failures no arithmetic sees coming, which is why ratings outrank numbers.

**you can also just tell it here.** that lands in the log tagged `chat:` and a sync **never**
overwrites it.

## two signals to go back

**the star is what you said. the re-order is what you did.**

| carries | tier | how it ranks |
|---|---|---|
| a 4 **and** a return | **anchor** | beats somewhere new on a tie. repeat it, same dish and all |
| a 4, never returned | loved | permission, not instruction. new still wins the tie |
| returned, never a 4 | habit | reliable. wins only when the day is thin |
| neither | untested | the default, if novelty is what you want |
| a 2 or worse | — | doesn't get a turn for being overdue |

## cheat day

your favourite cuisine and your macro floor are going to collide eventually. sushi day costs $18
and lands seven grams short. so it does **not** quietly rank your favourite last. it reads the
favourites off your ratings, and when one lands on a menu it can't build to your floor, it asks:

> sushi lands on thursday — your best-rated cuisine, five 4s. the nigiri gets to about 38g,
> seven under your floor. the curry clears it at 52g. cheat day, or the curry?

a cheat day bends a macro floor, a calorie ceiling, or that day's rotation. it never bends
allergies, $0.00 out of pocket, or anything you said was absolute. and if it fires most weeks,
it's a floor you don't actually want — it says so instead of waiving it forever.

## the other three

**`/feed-me sync`** — the books straightened with no lunch as a side effect. reads all three
tabs, opens every order, makes `order-history.md` match, places nothing. also the least
committal way to start: it builds your preferences out of the log, yours to overrule on sight.

**`/feed-me refine`** — refine goes looking for what an ordinary run never learns: a rule you
stated that your ratings argue with, a rule it inferred that stopped being true, and **the
gaps.** never ordering something and deliberately avoiding it look identical on paper.

**`/feed-me cancel <which>`** — kills it, no replacement, because most cancellations are you not
coming in. add **and reorder** and it puts something else on that day. `/feed-me cancel and
re-order tuesday's` needs no dish name. it proves the target against the live site before
touching anything, and builds the replacement before destroying the original.

## your data, your call

it will not type your password — it hits the login wall, stops, and waits.

`order-history.md` and `dietary-preferences.md` are personal records, which is the entire point
of them. both are in `.gitignore` here, so a clone gets the skill and none of the diary. it
writes your own on the first run.

## install

feed this to your LLM harness of choice:

```
install https://github.com/alanmarcero/feed-me as a skill
```

then `/feed-me`. if the skill doesn't show up, start a new session.

needs a browser for the ordering half: the playwright mcp server in the terminal, the built-in
browser pane in the claude desktop app, or claude in chrome. with no browser it goes back to
being a thing you paste menus into like it's 2024.

## on a schedule

want it on autopilot? it takes two apps:

- **claude desktop** runs a daily scheduled task. that's the scheduler, and it does the thinking
- **[claude in chrome](https://claude.com/chrome)** is the browser that task drives

it has to be chrome, not the desktop app's built-in pane. the pane forgets your ezCater login
whenever the app restarts, so every scheduled run wakes up at the sign-in page and orders
nothing. chrome keeps you signed in.

one-time setup:

1. install the claude in chrome extension and sign in with your claude account
2. in that chrome, sign in to the ezCater meal-program site once
3. in claude desktop, turn on the chrome connection and create a daily morning scheduled task
   that runs `/feed-me` through claude in chrome. or ask `/feed-me` to set it up for you
4. run the task once by hand to approve its tool permissions, including the command it uses to
   start chrome. an unapproved task stalls on a permission prompt with nobody there to click it

what has to be running when it fires:

| | needed? | |
|---|---|---|
| claude desktop | yes. minimized is fine, quit is not | it's the scheduler. quit it and the run silently doesn't happen |
| chrome | no | the task starts it in the background if it isn't running |
| your mac | awake. locked is fine, asleep is not | asleep means no run |

daily, because the open days move around week to week. a run with nothing to do orders nothing,
and a missed run gets caught the next day.

## disclaimer

protein and calorie numbers are estimates. nobody on ezCater publishes macros, so these are
educated guesses delivered with total confidence, the exact energy of every person who ever
eyeballed a chicken breast and declared it "like fifty grams, easy"

we go again

## license

[MIT](LICENSE)

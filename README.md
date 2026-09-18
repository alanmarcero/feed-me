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

## what it does

`/feed-me`. that's the whole interface.

| | |
|---|---|
| `/feed-me` | orders lunch for every open day, then syncs the log. the one you'll use |
| `/feed-me sync` | reconciles every order on the account. places nothing |
| `/feed-me refine` | mines your log against your preferences, asks better questions, sets cheat day |
| `/feed-me cancel <which>` | kills a pending order. add **and reorder** and it replaces it |

the last three are optional. a bare `/feed-me` keeps the books straight on its own.

it opens the ordering site in a real browser, finds **every day** still open, reads every menu,
prices the upcharges hiding behind each option modal, plans the week so no two days land on the
same food, and places one order per day. not one lunch. all of them.

it still shows the 5 ranked runners-up, because a nutritionist that won't show its work is a
vending machine with opinions. it'll also tell you about the perfect item sitting $0.04 over the
ceiling. so you can think about it. forever.

it answers in greentext, by requirement, with the numbers exact. the costume comes off twice:
the note it types into the special-instructions box, because a human reads that during a lunch
rush, and anything it needs *you* to answer, because a question buried in memes is a question
you skim past.

## the money

land on $20.01 and ezCater asks for your credit card like some kind of animal, over one (1)
cent. so: **a credit card is never entered.** ever. not for four cents. a card prompt is a bug
report, so it backs the order down and rebuilds until the total reads $0.00, and if nothing on
that day's menu gets there it buys nothing and says why.

the stipend doesn't pool or roll over — skip wednesday and wednesday's twenty dollars ceases to
exist — so it claims every open day off that day's own subsidy.

the catch: menu prices are pre-tax and the stipend is spent on the post-tax total. at 7% that
leaves about $18.68 of menu to play with. forty-one receipts agree: everything under it came
back covered, the one past it cost $0.20.

## the rules

the money rules are the skill's. the food rules are yours.

**the skill's, not up for discussion:**

| rule | ruling |
|---|---|
| the ceiling | HARD. derived from the stipend, verified against tax |
| credit card | never. a card prompt means the order is wrong, not that you owe money |
| every open day | an unclaimed day is a day of stipend deleted |
| one (1) real item | three $5 sides in a trenchcoat is not an entree |
| your reviews | outrank the arithmetic. a silent 4 beats a 3 with a paragraph |
| questions | plain english, outside the greentext |
| how it talks to you | second person, always. it never guesses your gender |
| pushing | never on its own. commits yes, publishes no |

**yours, and it just does what you said:**

| rule | ruling |
|---|---|
| allergies | HARD. the one gate a good-looking menu never argues with |
| a floor | a minimum to clear: protein, fiber, whatever. floors do not bend |
| a ceiling | a maximum to stay under: calories, carbs, sodium. bends only when nothing fits |
| cuisines | what you want most, what you'd rather skip |
| components | work these in, keep those out, keep that one light |
| prep | grilled not breaded, sauce on the side, whatever you're sick of |
| rationing | "fine, but not every week" |
| no rules | also fine. "just get me something good" is a complete answer |

the example file here says 45g protons, 800 calories, beans over rice, top of every heat scale,
and lettuce is packing peanuts — not because it's a vegetable, but because it's the cheapest
thing a kitchen can put under your protein. that's one person. the skill only cares that yours
is written down.

## rotation

the failure mode of a robot with a calorie rule is that it finds the one lowest-calorie
qualifying salad and serves you that salad until you die. so every order is tagged with a
**format** — `salad`, `carb-base`, `protein-plate`, `bowl-no-grain`, `soup-forward` — the slate
spans at least three, and four days ordered at once get four different ones. a format at zero
orders is **untested, not unwanted.**

## preferences

it looks for `dietary-preferences.md`. if it's not there, it does **not** hand you a form:

| what you've got | what it does |
|---|---|
| a preferences file | reads it, orders against it. best case |
| no file, but order history | infers preferences from what you rated, writes them down |
| neither | full sync first, then questions, then orders anyway |

**your order history is a preferences file nobody typed.** a rating is what you thought *after
eating*, which beats anything you'd say about your diet in the abstract. so the questions it
asks come off your log rather than a form — the one thing that doesn't add up, evidence
attached, with the reading it would take so agreeing is one word. five at most, allergies first
because it's the only one a log can't answer. answer none and nothing breaks.

every rule is tagged:

| tag | means | can the skill change it? |
|---|---|---|
| `stated` | you said it | **no.** it can argue, it cannot overrule |
| `derived` | inferred from your ratings | yes, when the log stops backing it up |
| `provisional` | inferred from one or two orders | yes, and it hunts for more evidence |

that's how you end up with good preferences having never written any. it's also how it catches
itself being wrong — it once insisted mediterranean food "landed well" through nine
contradicting ratings, because nothing marked the belief as revisable.

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

**a blank review is not missing data.** six of forty-one orders have text and all six are
complaints — you write when something's wrong and say nothing when it's right. and none of them
are about macros. "dry". "bland". "barely any steak". execution failures no arithmetic sees
coming, which is why ratings outrank numbers.

**you can also just tell it here.** that lands in the log tagged `chat:` and a sync **never**
overwrites it, though the star still comes off ezCater because that's the only place a number
lives. the log records what happened; the preferences file records what it *meant*. "i liked the
Scali salad" generalizes to one salad. "lose the lettuce, keep the tahini" generalizes to every
menu.

## two signals to go back

**the star is what you said. the re-order is what you did** — you had a whole slate that day and
picked the same place again, which is a choice made with a live budget rather than a click
afterward.

| carries | tier | how it ranks |
|---|---|---|
| a 4 **and** a return | **anchor** | beats somewhere new on a tie. repeat it, same dish and all |
| a 4, never returned | loved | permission, not instruction. new still wins the tie |
| returned, never a 4 | habit | reliable. wins only when the day is thin |
| neither | untested | the default, if novelty is what you want |
| a 2 or worse | — | doesn't get a turn for being overdue |

a repeat *it* placed for rotation isn't your preference reflected back, so it only counts when
another qualifying restaurant was open that day. neither signal clears a gate.

## cheat day

your favourite cuisine and your macro floor are going to collide eventually. sushi day costs $18
and lands seven grams short. so it does **not** quietly rank your favourite last and hope you
don't look.

it reads the favourites off your ratings rather than off anything you typed: several 4s, nothing
under a 3, more than two orders. five perfect sushi ratings means you love sushi whether or not
that's written down anywhere, and the same read works for pizza, thai, burgers, barbecue, tacos.
then when one of them lands on a menu it can't build to your floor, you get asked:

> sushi lands on thursday — your best-rated cuisine, five 4s. the nigiri gets to about 38g,
> seven under your floor. the curry clears it at 52g. cheat day, or the curry?

| bends on a cheat day | never bends |
|---|---|
| a macro floor | allergies. that's a wall, not a gate |
| a calorie, carb or sodium ceiling | $0.00 out of pocket, and the ceiling |
| that day's format rotation | anything you said was absolute |

one or two favourites, not six. and if the cheat day fires most weeks, it isn't a cheat day, it's
a floor you don't actually want — it says so instead of waiving it forever.

## the other three

**`/feed-me sync`** — the books straightened with no lunch as a side effect. reads all three
tabs, opens every order, makes `order-history.md` match, places nothing. "no drift" is a real
answer. it's also the least committal way to start: run it before answering anything and it
builds your preferences out of the log, yours to overrule on sight. every plain `/feed-me` ends
with a lazy version anyway, which turns "i placed four orders" into "i placed four orders and
all four are actually there."

**`/feed-me refine`** — the ordinary run only learns what a rating happens to teach it. refine
goes looking, and it's where **cheat day** gets settled rather than asked day by day: a rule you
stated that your ratings argue with, a rule it inferred that stopped being true, and the thing
nothing in a log points at — **the gaps.** never ordering something and deliberately avoiding it
look identical on paper and mean opposite things.

**`/feed-me cancel <which>`** — kills it, no replacement, because most cancellations are you not
coming in. add **and reorder** and it puts something else on that day. the delivery day is the
easy handle, so `/feed-me cancel and re-order tuesday's` needs no dish name; two pending orders
and it asks which. then it **proves the target** against the live site — right date, right id,
right dish — so a wrong tuesday is obvious at a glance instead of after lunch doesn't show up.
it builds the replacement before destroying the original.

**a plain cancel is not a complaint.** the *reorder* is what makes it a rejection — and that's
the strongest signal it gets, a preference stated about a build before anyone cooked it. it
sorts a *today* reason from a *food* reason and keeps only the second.

## your data, your call

it will not type your password — it hits the login wall, stops, and waits. it will not push on
its own either: it commits locally all day long, but `git push` is a publication and
publications are yours to order.

`order-history.md` and `dietary-preferences.md` are personal records, which is the entire point
of them. there's no version of this that's both useful and anonymous, so it doesn't try —
allergies in full, no safety line softened to make a file shareable. both are in
`.gitignore` here, so a clone gets the skill and none of the diary. it writes your own on the
first run.

## install

it's markdown in a folder, so git is optional:

```sh
mkdir -p ~/.claude/skills/feed-me
curl -fsSL https://github.com/alanmarcero/feed-me/tarball/main \
  | tar xz --strip-components=1 -C ~/.claude/skills/feed-me
```

no curl either? **Code → Download ZIP** on the repo page, then unzip it and move the *contents*
of `feed-me-main` into `~/.claude/skills/feed-me` — `SKILL.md` has to sit directly in that
folder, not one level down.

with git, if you'd rather updates be a `git pull`:

```sh
git clone https://github.com/alanmarcero/feed-me.git ~/.claude/skills/feed-me
```

then `/feed-me`

needs a browser for the ordering half: the playwright mcp server in the terminal, the built-in
browser pane in the claude desktop app. it drives either one. with no browser at all it still
works, it just goes back to being a thing you paste menus into like it's 2024.

## disclaimer

protein and calorie numbers are estimates. nobody on ezCater publishes macros, so these are
educated guesses delivered with total confidence, the exact energy of every person who ever
eyeballed a chicken breast and declared it "like fifty grams, easy"

we go again

## license

[MIT](LICENSE)

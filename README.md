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

## the problem

a stipend is use it or lose it. spend $16 like a coward and $4 evaporates into the void.
land on $20.01 and ezCater asks for your credit card like some kind of animal, over one (1)
cent. the good zone is narrow, it moves with sales tax, and you rediscover it daily while
hungry. unacceptable.

## what it does

`/feed-me`. that's the whole interface.

it opens the ordering site in a real browser, finds **every day** still open, reads every
menu, prices out the upcharges hiding behind each item's option modal, plans the week so no
two days land on the same food, and places one order per day.

not one lunch. all of them.

it still shows the 5 ranked runners-up, because a nutritionist that won't show its work is a
vending machine with opinions. it will also tell you about the perfect item sitting $0.04
over the ceiling. so you can think about it. forever.

it answers in greentext, by requirement. the numbers stay exact, the voice is insufferable on
purpose. the note it types into the special-instructions box is *not* in greentext, because a
human reads that box during a lunch rush. it says please.

## the money

the stipend doesn't pool and doesn't roll over. skip wednesday and wednesday's twenty dollars
ceases to exist. so it claims every open day, computing each day's ceiling off that day's own
subsidy and delivery fee. leaving one is a tuesday you paid for and did not eat.

the cart shows subsidy and total before you commit, so it builds to the largest order still
reading **total $0.00** and stops. at $20.00 and 7% tax the wall is $18.68, because $18.69
lands on exactly $20.00 with zero margin. that used to be a guess climbing a rung per
receipt. forty receipts settled it: everything at or under $18.50 came back covered, and the
one order that hit $18.88 cost $0.20.

## the one hard rule

**a credit card is never entered.** ever. not for four cents.

a card prompt is a **bug report**: the build went over the day's stipend, or the run wandered
onto the wrong path. so it stops, backs the order down, and rebuilds until the total reads
$0.00. if nothing on that day's menus gets there, it buys nothing and says why.

it will not spend your money. it only spends theirs.

## your data, your call

it will not type your password. it hits the login wall, stops, and waits for you to do the
one (1) thing that's yours to do.

it will not push, ever. it commits locally all day long, but `git push` is a publication and
publications are yours to order. no adding remotes, no flipping repos public.

`order-history.md` and `dietary-preferences.md` are personal records, which is the entire
point of them. there's no version of this that's both useful and anonymous, so it doesn't
try: allergies in full, order ids kept so a row traces back to its page, no safety line ever
softened to make a file easier to share. want the skill without the diary? `.gitignore` them
or use a private remote. the portable half, `SKILL.md` and `README.md`, has none of it
anyway.

## the rules

the money rules are the skill's. the food rules are yours.

**the skill's, not up for discussion:**

| rule | ruling |
|---|---|
| the ceiling | HARD. derived from the stipend, verified against tax |
| credit card | never. a card prompt means the order is wrong, not that you owe money |
| every open day | an unclaimed day is a day of stipend deleted |
| one (1) real item | three $5 sides in a trenchcoat is not an entree |
| rotation | no same format twice in a row |
| loved food | exempt from that. you rated it 4, so go back, same dish included |
| your reviews | outrank the arithmetic. a silent 4 beats a 3 with a paragraph |
| the numbers | live in `dietary-preferences.md`. never from memory |
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
| rationing | "fine, but not every week." carbs, fried stuff, whatever you're pacing |
| no rules | also fine. "just get me something good" is a complete answer |

the example file here says 40g protons, 800 calories, quinoa over brown rice over white, and
lettuce is packing peanuts. that's one guy. the skill only cares that yours is written down.

## the rotation thing

the failure mode of a robot with a calorie rule is that it finds the one lowest-calorie
qualifying salad and serves you that salad until you die. so every order gets tagged with a
**format** (`salad`, `carb-base`, `protein-plate`, `bowl-no-grain`, `soup-forward`) and the
five options span at least three of them.

this applies **inside** a single run: four days ordered in one go get four different formats.
two salad days in one week is worse than two salad weeks in a row, because it could see both
at once and did it anyway. variety can spend about 150 calories to break a repeat, and past
that it says so out loud.

## it works better with preferences. it works without them.

on first run it looks for `dietary-preferences.md`. if it's there it reads it and gets on
with lunch. if not, it does **not** make you fill in a form:

| what you've got | what it does |
|---|---|
| a preferences file | reads it, orders against it. best case |
| no file, but order history | infers preferences from what you rated, writes them down |
| neither | sane defaults, says it's guessing, starts the file from your first verdict |


**your order history is a preferences file nobody typed.** forty rated orders say more than
forty answers to a questionnaire, because a rating is what you thought *after eating*. it
tells you what it inferred and from which orders, easier to correct than an open question
about your diet.

the one thing the log can't tell it is allergies. never ordering shellfish is not evidence
either way, so it asks one question:

> anything you're allergic to or flat-out won't eat? i can work the rest out from your
> order history.

four words is a complete answer. ignore it and it orders anyway, but writes down *no limits
declared, never confirmed* so the gap stays visible, and asks again next time.

**if you'd rather just tell it**, it'll run the full interview: five skippable questions
covering hard limits, calories, macros, cuisines, components and prep.

### it's not a form you fill in once

every rule is tagged:

| tag | means | can the skill change it? |
|---|---|---|
| `stated` | you said it | **no.** it can argue, it cannot overrule |
| `derived` | inferred from your ratings | yes, when the log stops backing it up |
| `provisional` | inferred from one or two orders | yes, and it hunts for more evidence |

every time it reads new reviews it revises the `derived` rules that stopped being true. if
the log disagrees with something **you** said, it says so once, shows you the ratings, and
keeps doing what you told it.

this is also how you end up with good preferences having never written any: start cold, rate
your lunches, watch it fill itself in.

## it reads your reviews off the site

you already rate orders on ezCater, and clicking a star is less work than typing a sentence,
so it reads the stars instead of asking twice. five stars, stored 0-4:

| stars | verdict | what happens |
|---|---|---|
| 4 | **loved** | go back. same dish is fine |
| 3 | liked | fine to repeat, not a priority |
| 2 | neutral | ranks *below* something untested |
| 1 | disliked | the dish is done |
| 0 | never again | dish done, kitchen suspect |

ratings live in markup the accessibility tree doesn't expose, so it parses the order-detail
pages directly, re-scraping whenever something's been delivered since the last snapshot.

**a blank review is not missing data.** six of forty orders have text and all six are
complaints. you write when something's wrong and say nothing when it's right, so a silent 4
outranks a chatty 3. and the complaints are never about macros. "dry". "bland". "stale,
clearly cooked hours or even a day earlier". "barely any steak". execution failures no
arithmetic sees coming, which is why the ratings outrank the numbers.

it all lands in `order-history.md`, where the gains get tracked. every order, every verdict.
a cancelled order doesn't get logged, because the ledger is what you ate, not what you almost
ate. preferences don't live there: the log records what happened, the preferences file
records what it *meant*. "i liked the Scali salad" generalizes to one salad. "lose the
lettuce, keep the tahini" generalizes to every menu.

## loved means go back

the failure mode of a rotation rule is treating good food as a thing to be rationed. you
rated it 4. that's a request. so a **loved** kitchen is exempt from "pick somewhere else
this time" and can repeat inside the four-order window, something new off that menu or the
same build again. it just has to say it's riding on a 4, so you can tell a deliberate repeat
from a robot out of ideas. inverse too: a **neutral** kitchen doesn't get a turn for being
overdue.

## a worked example of it changing its mind

one user's log, not a rule for yours. forty orders in, averaged by cuisine:

| cuisine | orders | avg |
|---|---|---|
| indian | 2 | **4.00** |
| east / southeast asian | 12 | **3.33** |
| latin | 3 | 3.00 |
| mediterranean / middle eastern | 9 | **2.78** |

the skill used to say mediterranean "lands well". it does not. that belief came from
components he'd said he liked (tahini, hummus, feta, olives) and not from a single rating.
both 2s came with a review saying the same thing: the pita was dry, the shawarma was cooked
a day ago. invisible from the menu text.

the components are real, the kitchens are the problem. so: still order the hummus, stop
picking the restaurant because it has hummus.

## install

```sh
git clone git@github.com:alanmarcero/feed-me.git ~/.claude/skills/feed-me
```

then `/feed-me`

needs the playwright mcp server for the ordering half. without it the skill still works, it
just goes back to being a thing you paste menus into like it's 2024.


## disclaimer

protein and calorie numbers are estimates. no restaurant on ezCater publishes macros, so
these are educated guesses delivered with total confidence, the exact energy of every person
who ever eyeballed a chicken breast and declared it "like fifty grams, easy"

we go again

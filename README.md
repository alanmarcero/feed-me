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

| | |
|---|---|
| `/feed-me` | orders lunch for every open day, then syncs. this is the one you'll use |
| `/feed-me sync` | reconciles the log against ezCater and orders nothing |

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
ceases to exist. so it claims every open day, computing each ceiling off that day's own
subsidy and delivery fee. leaving one is a tuesday you paid for and did not eat.

the cart shows subsidy and total before you commit, so it builds to the largest order still
reading **total $0.00** and stops. the catch: every menu price is pre-tax and the stipend is
spent on the post-tax total, so a $19.50 lunch is not a $19.50 lunch. it does that conversion
on every candidate before ranking. at 7% that leaves about $18.68 of menu to play with, and
forty receipts agree: everything under it came back covered, the one order past it cost $0.20.

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
publications are yours to order. no adding remotes, no flipping repos public either.

`order-history.md` and `dietary-preferences.md` are personal records, which is the entire
point of them. there's no version of this that's both useful and anonymous, so it doesn't
try: allergies in full, order ids kept so a row traces back to its page, no safety line ever
softened to make a file shareable. want the skill without the diary? `.gitignore` them or use
a private remote. the portable half has none of it anyway.

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
at once and did it anyway.

## /feed-me sync

the books straightened, with no lunch as a side effect. it reads all three tabs, opens every
order, and makes `order-history.md` match: statuses advanced, ratings and reviews filled in,
orders you placed without the skill added, cancellations moved across, anything it can't find
flagged instead of deleted. it places nothing and cancels nothing. "no drift" is a real
answer and you'll get it plainly.

**it's also the least committal way to start.** run `sync` before answering anything and it
builds your preferences out of the log: median protein across the orders you rated 3 or
better becomes the floor, the 75th percentile of calories becomes the ceiling, cuisines rank
by average rating, components come from what keeps showing up in your 4s. all of it tagged
`provisional`, each shown with the number it came from, and yours to overrule on sight.

**and every plain `/feed-me` ends with one anyway.** you don't have to remember to run it. the
books staying straight is the skill's problem, not yours.

it's also the only thing that turns "i placed four orders" into "i placed four orders and
all four are actually there." a confirmation page is a claim; the upcoming tab is the fact.
if an order it just placed isn't on the site, that's the run failing, not finishing, and it
says so loudly and re-places it if the cutoff hasn't passed.

run plain `/feed-me` with no history and no preferences and it syncs *first* too, says so,
then carries on into the order. your account has your lunches on it even when the skill
doesn't.

## it works better with preferences. it works without them.

it looks for `dietary-preferences.md`. if it's there it reads it and gets on with lunch. if
not, it does **not** make you fill in a form:

| what you've got | what it does |
|---|---|
| a preferences file | reads it, orders against it. best case |
| no file, but order history | infers preferences from what you rated, writes them down |
| neither | syncs first, and if that's empty too, sane defaults and says it's guessing |

**your order history is a preferences file nobody typed.** forty rated orders say more than
forty answers to a questionnaire, because a rating is what you thought *after eating*.

**and if you'd rather tell it, the questions come out of your log, not off a form.**
describing your diet in the abstract is hard and the answers turn out wrong. being shown your
orders and asked about the one thing that doesn't add up is easy. so it asks about the
ambiguity and shuts up about what the log already settles:

> 1. anything you're allergic to or won't eat? only thing your orders can't tell me.
> 2. 26 of 40 orders are carbs and they hold 9 of your 16 favourites, but the rule says one in
>    three. drop the rule?
> 3. you like tahini/hummus/feta but mediterranean kitchens are your lowest-rated cuisine
>    (2.78, against 4.00 for indian). components, or the kitchens?
> 4. one tofu bowl, rated 1. tofu out entirely or was that just a bad bowl?
> 5. four days you cancelled outright and never reordered. bad menus or bad days?

five at most, allergies always first because it's the only one a log provably can't answer.
every other one carries its evidence and offers the reading it would take, so agreeing is one
word. answer none of them and nothing breaks: it writes *no limits declared, never confirmed*
so the gap stays visible, and won't shut up about it on the day it actually orders.

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

this is how you end up with good preferences having never written any: start cold, rate your
lunches, watch it fill itself in. it's also how the skill catches itself being wrong. it used
to insist mediterranean food "landed well" on the strength of components he'd listed, and
nine contradicting ratings never budged it, because nothing marked the belief as revisable.

## it reads your reviews off the site

you already rate orders on ezCater, and a star is less work than a sentence, so it reads the
stars instead of asking twice. five stars, stored 0-4:

| stars | verdict | what happens |
|---|---|---|
| 4 | **loved** | go back. same dish is fine |
| 3 | liked | fine to repeat, not a priority |
| 2 | neutral | ranks *below* something untested |
| 1 | disliked | the dish is done |
| 0 | never again | dish done, kitchen suspect |

ratings live in markup the accessibility tree doesn't expose, so it parses the order-detail
pages directly.

**a blank review is not missing data.** six of forty orders have text and all six are
complaints. you write when something's wrong and say nothing when it's right, so a silent 4
outranks a chatty 3. and the complaints are never about macros. "dry". "bland". "stale,
clearly cooked hours or even a day earlier". "barely any steak". execution failures no
arithmetic sees coming, which is why ratings outrank numbers.

**you can also just tell it here.** say the pita was dry and that lands in the log same as a
site review, tagged `chat:` so the two stay tellable apart. the star still comes off ezCater,
because that's the only place a number lives, but the text is the union of both and a sync
**never** overwrites what you said in conversation. an empty review box on the site is not
evidence you said nothing. if the two ever point opposite ways, a 4 against "eh, wouldn't
rush back", it keeps both and asks once rather than quietly picking.

it all lands in `order-history.md`. every order, every verdict, every cancellation, each
carrying a status from placed to rated. preferences don't live there: the log records what
happened, the preferences file records what it *meant*. "i liked the Scali salad" generalizes
to one salad. "lose the lettuce, keep the tahini" generalizes to every menu.

## loved means go back

the failure mode of a rotation rule is treating good food as a thing to be rationed. you
rated it 4. that's a request. so a **loved** kitchen is exempt from "pick somewhere else this
time" and can repeat inside the four-order window. it just has to say it's riding on a 4, so
you can tell a deliberate repeat from a robot out of ideas. a **neutral** kitchen doesn't get
a turn for being overdue.

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

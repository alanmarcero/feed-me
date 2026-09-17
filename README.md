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
| `/feed-me` | orders lunch for every open day, then runs a **lazy sync**. the one you'll use |
| `/feed-me sync` | a **full sync**. opens every order on the account. places nothing |
| `/feed-me refine` | mines your log against your preferences and asks better questions |
| `/feed-me cancel <which>` | kills a pending order. add **and reorder** and it picks you something else |

the other three are **optional but useful**. you never have to type them — a bare `/feed-me`
still keeps the books straight on its own. reach for them when you want the ledger audited,
the rules sharpened, or a plate you've changed your mind about taken off the books.

two syncs, and they're called exactly that. **lazy sync** runs itself at the end of every
order. **full sync** is what you get when you ask for one. the only difference is whether a
settled row gets its order page re-opened.

it opens the ordering site in a real browser, finds **every day** still open, reads every
menu, prices out the upcharges hiding behind each item's option modal, plans the week so no
two days land on the same food, and places one order per day.

not one lunch. all of them.

it still shows the 5 ranked runners-up, because a nutritionist that won't show its work is a
vending machine with opinions. it will also tell you about the perfect item sitting $0.04
over the ceiling. so you can think about it. forever.


it answers in greentext, by requirement. the numbers stay exact, the voice is insufferable on
purpose. two places the costume comes off. the note it types into the special-instructions box,
because a human reads that box during a lunch rush — it says please. and anything it needs *you*
to answer, which comes back in plain english under the block, because a question buried in memes
is a question you skim past.

## the money

the stipend doesn't pool and doesn't roll over. skip wednesday and wednesday's twenty dollars
ceases to exist. so it claims every open day, computing each ceiling off that day's own
subsidy and delivery fee. leaving one is a tuesday you paid for and did not eat.

the cart shows subsidy and total before you commit, so it builds to the largest order still
reading **total $0.00** and stops. the catch: every menu price is pre-tax and the stipend is
spent on the post-tax total, so a $19.50 lunch is not a $19.50 lunch. it does that conversion
on every candidate before ranking. at 7% that leaves about $18.68 of menu to play with, and
forty-one receipts agree: everything under it came back covered, the one past it cost $0.20.

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
| loved food | a 4 buys permission to repeat. a 4 *and* a return beats trying somewhere new |
| your reviews | outrank the arithmetic. a silent 4 beats a 3 with a paragraph |
| cancelling | the target gets proven against the site first. a plain cancel is never read as a complaint |
| questions | asked in plain english, outside the greentext. you should never have to hunt for one |
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

the example file here says 45g protons, 800 calories, beans over quinoa over brown rice over
white, a light carb on every plate, top of every heat scale, and lettuce is packing peanuts —
not because it's a vegetable, but because it's the cheapest thing a kitchen can put under your
protein. that's one guy. the skill only cares that yours is written down.

## the rotation thing

the failure mode of a robot with a calorie rule is that it finds the one lowest-calorie
qualifying salad and serves you that salad until you die. so every order gets tagged with a
**format** (`salad`, `carb-base`, `protein-plate`, `bowl-no-grain`, `soup-forward`) and the
five options span at least three of them.

this applies **inside** a single run: four days ordered in one go get four different formats.
two salad days in one week is worse than two salad weeks in a row, because it could see both
at once and did it anyway.

a format at zero orders is **untested, not unwanted** — it'll reach for one rather than treat
the gap as a decision you made.

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

**and every plain `/feed-me` ends with a lazy one anyway.** you don't have to remember. the
books staying straight is the skill's problem, not yours.

lazy means it skips detail pages it doesn't need, not that it skips checking. it always reads
all three tab listings, because that's where changes announce themselves: the card tells it
*whether* you rated something, the order page tells it *what you rated*. so it only opens an
order it's never seen, one that just picked up a review, or one whose row is missing
something. a settled log goes from ~45 fetches to about 5.

`/feed-me sync` is the thorough one. every order, every time, no skipping. that's what
catches the things a card can't show: a rating you changed from 3 to 4, review text you
edited later, a price the restaurant corrected.

the lazy pass is also the only thing that turns "i placed four orders" into "i placed four
orders and all four are actually there." a confirmation page is a claim; the upcoming tab is
the fact. orders it just placed are new, so laziness never applies to them. if one isn't on
the site, that's the run failing rather than finishing, and it says so loudly and re-places it
if the cutoff hasn't passed.

run plain `/feed-me` with no history and no preferences and it does the **full** one first,
says so, builds your log and your preferences out of it, asks the questions that log raised,
and then orders anyway without waiting for you to answer. nothing to be lazy against on a
first run, and the ratings are exactly what the first order needs.

it won't hold lunch hostage to a questionnaire. you typed `/feed-me` because you wanted food.
answer whenever, and since orders stay editable until each day's cutoff it can still apply
your answer to today's lunch rather than next week's.

## it works better with preferences. it works without them.

it looks for `dietary-preferences.md`. if it's there it reads it and gets on with lunch. if
not, it does **not** make you fill in a form:

| what you've got | what it does |
|---|---|
| a preferences file | reads it, orders against it. best case |
| no file, but order history | infers preferences from what you rated, writes them down |
| neither | full sync first, then questions, then orders. if that's empty too, sane defaults |

**your order history is a preferences file nobody typed.** thirty-four rated orders say more
than thirty-four answers to a questionnaire, because a rating is what you thought *after
eating*.

**and if you'd rather tell it, the questions come out of your log, not off a form.**
describing your diet in the abstract is hard and the answers turn out wrong. being shown your
orders and asked about the one thing that doesn't add up is easy. so it asks about the
ambiguity and shuts up about what the log already settles:

1. Anything you're allergic to or won't eat? The only thing your orders can't tell me.
2. 27 of 41 orders are carbs and they hold 12 of your 16 favourites, but the rule rations them
   to one in three. Drop the rule?
3. You like tahini, hummus and feta, but Mediterranean kitchens are your lowest-rated cuisine —
   2.80, against 4.00 for Indian. Components, or the kitchens?
4. One tofu bowl, rated 1. Tofu out entirely, or was that just a bad bowl?
5. Four days you cancelled outright and never reordered. Bad menus or bad days?

**note the voice.** the greentext is for things you read and put down. anything it needs *you*
to answer comes back in plain english, outside the block — a numbered list in meme cadence
reads as more findings and gets skimmed past, which is exactly how a refine pass with eight
questions in it once came back looking like a report with none.

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

## /feed-me refine

the ordinary run only learns what a rating happens to teach it, which is not much per meal.
refine goes looking on purpose.

it's the same questions the cold start asks, pointed at a preferences file that already
exists, which makes them better questions: now there are rules to test the log *against*. a
rule you stated that your own ratings argue with. a rule it inferred that stopped being true.
a guess made off one order that nothing has tested since. a pattern that's held long enough
to stop being a guess.

and the thing no ordinary run can ever surface, because nothing in a log points at what isn't
in it: **the gaps.** a format you've never once ordered. a cuisine carrying a 4.00 average on
two orders. a restaurant you tried once and never again. meals you never rated. whole axes the
file has no opinion on, like whether you want spice as a scale or a yes, or how much of a plate
you actually finish.

**an absence in a log is not a preference**, and that's the whole reason the mode exists. never
ordering a thing and deliberately avoiding it look identical on paper and mean opposite things.
one run here found a format sitting at zero orders out of forty-one that turned out to be the
exact shape of three rules the file already carried — nobody had ever said no to it, it just
never came up.

it leads with the finding and puts the question after it, reports the good patterns even when
there's nothing to decide, and writes only to `dietary-preferences.md`. answer nothing and it
still fixes what it inferred wrong.

it never opens the browser. both local files are the whole input, and if one is missing it
stops and asks whether you want a sync rather than quietly running one — comparing two things
is the entire point, and a mode that places nothing has no business spending a login.

## it reads your reviews off the site

you already rate orders on ezCater, and a star is less work than a sentence, so it reads the
stars instead of asking twice. five stars, stored 0-4:

| stars | verdict | what happens |
|---|---|---|
| 4 | **loved** | going back is allowed. go back twice and it's an anchor |
| 3 | liked | fine to repeat, not a priority |
| 2 | neutral | ranks *below* something untested |
| 1 | disliked | the dish is done |
| 0 | never again | dish done, kitchen suspect |

ratings live in markup the accessibility tree doesn't expose, so it parses the order-detail
pages directly.

**a blank review is not missing data.** six of forty-one orders have text on the site and all
six are complaints. you write when something's wrong and say nothing when it's right, so a silent 4
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

## two signals to go back

the failure mode of a rotation rule is treating good food as a thing to be rationed. so it
reads two different things as reasons to return, and they're made of different evidence.

**the star is what you said.** you rated it 4, after eating, which is the only place the food
itself gets judged.

**the re-order is what you did.** you had a whole slate that day and picked the same place
again. that's a choice made with a live budget, not a click made afterward — and if you like
trying new things, going back cost you something. which is exactly what makes it worth reading.

| carries | tier | how it ranks |
|---|---|---|
| a 4 **and** a return | **anchor** | beats somewhere new on a tie. repeat it, same dish and all |
| a 4, never returned | loved | permission, not instruction. new still wins the tie |
| returned, never a 4 | habit | reliable. wins only when the day is thin |
| neither | untested | the default, if novelty is what you want |
| a 2 or worse | — | doesn't get a turn for being overdue |

four guards keep it honest. a repeat *it* placed for rotation isn't your preference reflected
back, so it only counts when another qualifying restaurant was open that day. a same-day
cancellation is one visit, not two. a return followed by a 3 doesn't retroactively become a 4
— the anchor says go back, the 3 says what to order when you do. and neither signal clears a
gate: an anchor that can't hit the protein floor under the calorie ceiling still doesn't get
placed.

it says which signals a repeat is riding on, so you can tell a deliberate return from a robot
out of ideas.

## changed your mind

`/feed-me cancel the curry plate` kills it. that's it, no replacement — most cancellations
are you not coming in, and inventing a lunch for a day you won't be there is worse than
nothing.

`/feed-me cancel the current plate and reorder` kills it **and** puts something else on that
day.

**the delivery day works as the handle, and it's usually the easy one.** each day is its own
order, so `/feed-me cancel and re-order tuesday's` is unambiguous without naming a dish. so is
"cancel tuesday's and wednesday's" — it runs them one day at a time, because each day has its
own cart and its own cutoff.

two pending orders and you didn't say which? it asks, in plain english, and lists them. it
won't guess at a destructive action. it reads the **upcoming** tab to find the target, never
the log — a cache is not a good enough reason to cancel someone's lunch.

**then it proves the target before it touches it.** "tuesday" gets resolved to a real date
against the live delivery dates, it confirms an order actually exists on that date, and it
opens that order's own page to check the id, the day, the restaurant and the dish all agree
with what you asked for. anything off and it stops and asks. it reads the target back to you
with its date and order id, so a wrong tuesday is obvious at a glance instead of after lunch
doesn't show up. no order on that date? it says so and cancels nothing — it never falls
through to "the closest one".

**a plain cancel is not a complaint.** cancelling tuesday means you won't be in on tuesday, and
that says nothing about tuesday's menu. it logs that as a dropped day and the restaurant keeps
its record clean. the *reorder* is what makes it a rejection: you still want lunch, just not
that lunch. and it won't ask you to justify not coming into the office.

**it builds the replacement before it destroys the original.** the day never sits empty while
a rebuild is still possible. it also strips the old line out first, because the stipend is per
*day* and shared across every order on it — two live orders on one day can blow the ceiling
between them and summon the card prompt. if nothing on that day's menu clears your gates, it
leaves your original order alone and tells you why. food you didn't want beats no food.

cutoff already passed? cancel-only still works and it'll tell you the day is gone. asked for a
swap? it does **neither** and says so, because cancelling without being able to rebuild is
half of a thing you didn't ask for.

**tell it why and that's the strongest signal it gets.** "cancel the curry, too much rice
lately" is you stating a preference about a build before anyone cooked it — cleaner than a
review, which is always tangled up with how one kitchen did that day. it sorts a *today* reason
from a *food* reason and only the second one becomes a rule. the reason also shapes the
replacement: "too heavy" means lighter, not merely different. then it closes with a sync,
because "i cancelled it" and "the site agrees i cancelled it" are different claims.

## install

```sh
git clone git@github.com:alanmarcero/feed-me.git ~/.claude/skills/feed-me
```

then `/feed-me`

needs a browser for the ordering half. in the terminal that's the playwright mcp server; in
the claude desktop app it's the built-in browser pane, and the skill drives either one. both
paths are documented under **which browser drives this** and both have placed real orders.

with no browser at all the skill still works, it just goes back to being a thing you paste
menus into like it's 2024.


## disclaimer

protein and calorie numbers are estimates. no restaurant on ezCater publishes macros, so
these are educated guesses delivered with total confidence, the exact energy of every person
who ever eyeballed a chicken breast and declared it "like fifty grams, easy"

we go again

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

that was the origin story. the skill is not actually about protons — it's about whatever
*you* are trying to eat. tell it you're cutting, bulking, keto, kosher, allergic to
shellfish, or that you just want food that isn't sad, and it optimizes for that instead.
the protons are one guy's preferences file, not the law.

## the problem

a stipend is use it or lose it. spend $16 like a coward and $4 evaporates into the void.
spend $18.75 and ezCater asks for your credit card like some kind of animal. the good
zone is narrow, it moves with sales tax, and you have to rediscover it every single day
while hungry and staring at a wall of sandwiches. unacceptable.

## what it does

`/feed-me`. that's it. that's the whole interface.

it opens the ordering site in a real browser, finds **every day** that is still open,
reads every menu on every one of those days, prices out the upcharges hiding behind each
item's option modal, plans the whole week so no two days land on the same kind of food,
types a polite note to each kitchen, and places one order per day.

not one lunch. all of them.

no menus to paste. no options to weigh. no decision to make. be super lazy. make the ai
order my protons.

it still shows you the 5 ranked runners-up, because a nutritionist that won't show its
work is just a vending machine with opinions. every single one clears **your** gates —
whatever you told it those are — or it does not make the list. every single one lands
inside the window.

it will also tell you about the perfect item sitting $0.04 over the ceiling. so you can
think about it. forever.

it answers in greentext. this was not a stylistic accident, it was a requirement. short
lines, gym-bro register, no line whose job is to introduce the next line. the numbers are
still exact. the voice is just insufferable on purpose.

the note it types into the special-instructions box is not in greentext, because a human
being reads that box during a lunch rush. it says please. it asks for extra pickled
mustard greens *if you can*. it is the single most polite thing in this repo.

## every day is its own $20

the stipend does not pool and it does not roll over. skip wednesday and wednesday's
twenty dollars simply ceases to exist. so it claims every open day, computes each day's
ceiling off that day's own subsidy and delivery fee, and lands each one on its own.

four days open means four orders. leaving one on the table is not restraint, it is a
tuesday you paid for and did not eat.

## the money

the cart shows subsidy and total before you commit, so it builds to the largest order
that still reads **total $0.00** and stops there. at $20.00 and 7% tax the arithmetic
wall is $18.68 — because $18.69 lands on exactly $20.00 with zero margin.

this used to be a guess that climbed one rung per receipt. it isn't any more. forty
receipts settled it: every subtotal at or under $18.50 came back fully covered, and the
one order that went to $18.88 cost exactly $0.20. the wall is where the math said it was.
no more ladder.

## the one hard rule

**a credit card is never entered.** ever. not for four cents.

a card prompt is not a bill, it is a **bug report**. it means the build went over the
day's stipend or the whole run wandered onto the wrong path. so it stops, backs the order
down, and rebuilds until the total reads $0.00 again. if no order on that day's menus can
get there, it buys nothing that day and tells you why.

it will not spend your money. it only spends theirs.

## what it will not do

it will not type your password. if it hits the login wall it stops, tells you the browser
window is sitting right there, and waits for you to do the one (1) thing that is actually
yours to do.

it will not push. not to back up the log, not because there's an unpushed commit sitting
there, not as a tidy-up at the end of a run. it commits locally all day long — that's
useful — but `git push` is a publication and publications are yours to order. it also
won't add a remote, change one, or flip a repo public. no remote configured is a decision,
not an oversight.

## about the data files

`order-history.md` and `dietary-preferences.md` are personal records. that is the entire
point of them. they know what you eat, what you thought of it, and what you cannot safely
be served. there is no version of this that is both useful and anonymous, so the skill does
not try — it writes allergies in full, keeps your order ids so a row traces back to its
page, and does not soften a safety line to make a file easier to share.

what happens to those files after that is your call. this particular repo is public because
its owner decided it should be. a fresh clone has decided nothing, and the default is local
until you say otherwise. want the skill without the diary? `.gitignore` the two data files,
or point it at a private remote. it'll keep working.

the portable half — `SKILL.md` and `README.md` — has none of that in it. process, selectors,
rules, voice. clone it and it's yours.

## a worked example of it changing its mind

this is one user's log, not a rule for yours. it's here because it's the clearest thing
the skill does that a static preferences file cannot.

forty orders in, averaged by cuisine:

| cuisine | orders | avg |
|---|---|---|
| indian | 2 | **4.00** |
| east / southeast asian | 12 | **3.33** |
| latin | 3 | 3.00 |
| mediterranean / middle eastern | 9 | **2.78** |

which is funny, because the skill used to say mediterranean "lands well". it does not.
that belief came from a list of components he'd said he liked — tahini, hummus, feta,
olives — and not from a single rating. nine orders later: two 2s, four 3s, one 4.

the components are real. the kitchens are the problem. both of the 2s came with a review
attached and both say the same thing — the pita was dry, the shawarma was cooked a day
ago. that's a food-holding problem and it is completely invisible from the menu text.

so: still order the hummus. stop picking the restaurant because it has hummus.

your log will say something entirely different. the point is that it gets read.

## the rules

the money rules are the skill's. the food rules are yours.

**the skill's, and not up for discussion:**

| rule | ruling |
|---|---|
| the ceiling | HARD. derived from the stipend, verified against tax. we do not go over. |
| credit card | never. a card prompt means the order is wrong, not that you owe money |
| every open day | all of them get ordered. an unclaimed day is a day of stipend deleted |
| one (1) real item | three $5 sides in a trenchcoat is not an entree |
| rotation | no same format twice in a row. it is a nutritionist, not a vending machine |
| loved food | exempt from that. you rated it 4, that means go back, same dish included |
| your reviews | outrank the arithmetic. a 4 you never wrote a word about beats a 3 with a paragraph |
| the numbers | live in `dietary-preferences.md`. read from there, never from memory |
| pushing | never on its own. commits yes, publishes no. your repo, your call |

**yours, and it just does what you said:**

| rule | ruling |
|---|---|
| allergies | HARD. the one gate a good-looking menu never gets to argue with |
| a floor | a minimum to clear — protein, fiber, whatever. floors do not bend |
| a ceiling | a maximum to stay under — calories, carbs, sodium. bends only when nothing on the menu fits under it |
| cuisines | what you want most, what you'd rather skip |
| components | work these in, keep those out, keep that one light |
| prep | grilled not breaded, sauce on the side, whatever you're sick of |
| rationing | "fine, but not every week." carbs, fried stuff, whatever you're pacing |
| no rules at all | also fine. "just get me something good" is a complete answer |

the example file in this repo says 40g protons, 800 calories, quinoa over brown rice over
white, and lettuce is packing peanuts. that is one guy. yours will say something else
entirely and the skill does not care which — it cares that it's written down.

## the rotation thing

the failure mode of a robot with a calorie rule is that it finds the one lowest-calorie
qualifying salad and then serves you that salad until you die. so every order gets tagged
with a **format** — `salad`, `carb-base`, `protein-plate`, `bowl-no-grain`, `soup-forward` —
and the same format cannot go twice in a row. the five options it hands back have to span
at least three of them.

this applies **inside** a single run, too. when it orders monday through thursday in one
go, those four days are four consecutive orders — so they get four different formats. two
salad days in one week is a worse failure than two salad weeks in a row, because it could
see both at the same time and did it anyway.

variety is allowed to spend about 150 calories to break a repeat. past that, calories win,
but it has to say out loud that it is repeating itself and why.

## it works better with preferences. it works without them.

on first run it looks for `dietary-preferences.md`. if it's there it reads it and gets on
with lunch. if it isn't, it does **not** stop and make you fill in a form. it works down
this ladder instead:

| what you've got | what it does |
|---|---|
| a preferences file | reads it, orders against it. best case |
| no file, but order history | infers your preferences from what you rated, orders against that, writes it down |
| neither | orders on sane defaults, says out loud that it's guessing, starts the file from your first verdict |

**your order history is a preferences file nobody typed.** forty rated orders say more
about what you want for lunch than forty answers to a questionnaire, because a rating is
what you thought *after eating* and a questionnaire is what you claim in the abstract. so
it reads the log, works out which cuisines you rate highest and what your 2-stars have in
common, and tells you what it inferred and from which orders — which is a much easier
thing to correct than an open question about your own diet.

the one thing the log cannot tell it is allergies. never ordering shellfish is not evidence
of an allergy and not evidence against one. so it asks exactly one question:

> anything you're allergic to or flat-out won't eat? i can work the rest out from your
> order history.

four words is a complete answer. ignore it and it orders anyway, but it writes down *no
limits declared — never confirmed* so the gap is visible instead of assumed away, and it
asks again next time.

**if you'd rather just tell it**, it'll take the full interview — order history first so it
arrives with proposed answers instead of a blank form, then five questions in one message:
hard limits, calories, macros, cuisines, components and prep. every one is skippable.
"don't care" gets recorded as *no preference stated*, so next time it knows it asked.

either way it writes the file, shows you what it wrote, and gets on with the order you
actually asked for. no second invocation.

## the preferences file learns

`dietary-preferences.md` is not a form you fill in once. every rule in it is tagged:

| tag | means | can the skill change it? |
|---|---|---|
| `stated` | you said it | **no.** yours. it can argue, it cannot overrule |
| `derived` | inferred from your ratings | yes, whenever the log stops backing it up |
| `provisional` | inferred from one or two orders | yes, and it goes looking for more evidence |

so every time it reads new reviews it re-derives the `derived` rules and revises the ones
that stopped being true. if the log starts disagreeing with something **you** said, it says
so once, shows you the ratings, and then keeps doing what you told it.

this is also how you end up with good preferences having never written any. start it cold,
rate your lunches, and the file fills itself in. a file that started empty and is *still*
empty after five rated orders means the loop isn't running and something is broken.

this is not decoration. the mediterranean thing below survived nine contradicting orders
purely because nothing marked it as revisable.

## brotein log

`order-history.md` is where the gains get tracked. every order logged. every verdict
recorded. a cancelled order is not an order and does not get logged — the ledger is what
you actually ate, not what you almost ate.

preferences don't live here — they live in `dietary-preferences.md`. that's the split. the
log records what happened; the preferences file records what it *meant*. "i liked the Scali
salad" generalizes to exactly one salad. "lose the lettuce, keep the tahini" generalizes to
every menu on earth.

## it reads your reviews off the site

you already rate your orders on ezCater. clicking a star is less work than typing a
sentence, so the skill goes and reads the stars instead of asking you to say it twice.

ezCater shows five stars and stores a 0-4:

| stars | verdict | what happens |
|---|---|---|
| 4 | **loved** | go back. same dish is fine. see below |
| 3 | liked | fine to repeat, not a priority |
| 2 | neutral | ranks *below* something untested |
| 1 | disliked | the dish is done |
| 0 | never again | the dish is done and the kitchen is suspect |

the ratings and the reviews live in markup the accessibility tree does not expose, so it
parses the order-detail pages directly. the selectors are written down in the skill. it
re-scrapes at the start of any run where something has been delivered since the last
snapshot.

**a blank review is not missing data.** six of forty orders have text on them and all six
are complaints. you write when something is wrong and say nothing when it's right, so a
silent 4 outranks a chatty 3.

and the complaints are never about macros. they are "dry", "bland", "stale, clearly
cooked hours or even a day earlier", "barely any steak". those are execution failures
that no amount of arithmetic can see coming, which is the entire reason the ratings
outrank the numbers when it builds a slate.

## loved means go back

the failure mode of a rotation rule is that it treats good food as a thing to be rationed.
you rated it 4. that is not a data point, it's a request.

so a **loved** kitchen is exempt from "pick somewhere else this time", and it can repeat
inside the four-order window — something new off that menu if there is one, or the exact
same build again if there isn't. no apology. it just has to say out loud that it's riding
on a 4, so you can tell a deliberate repeat from a robot running out of ideas.

the inverse also holds. a **neutral** kitchen does not get a turn just because it hasn't
had one lately. rotation happens among food you like.

## install

```sh
git clone git@github.com:alanmarcero/feed-me.git ~/.claude/skills/feed-me
```

then `/feed-me`

needs the playwright mcp server for the ordering half. without it the skill still works,
it just goes back to being a thing you paste menus into like it's 2024.

## disclaimer

protein and calorie numbers are estimates. no restaurant on ezCater publishes macros,
so these are educated guesses delivered with total confidence — the exact energy of every
person who has ever eyeballed a chicken breast and declared it "like fifty grams, easy"

we go again

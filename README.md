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
spend $18.75 and ezCater asks for your credit card like some kind of animal. the good
zone is narrow, it moves with sales tax, and you have to rediscover it every single day
while hungry and staring at a wall of sandwiches. unacceptable.

## what it does

`/feed-me`. that's it. that's the whole interface.

it opens the ordering site in a real browser, finds **every day** that is still open,
reads every menu on every one of those days, prices out the protein upcharges hiding
behind each item's option modal, plans the whole week so no two days land on the same
kind of food, types a polite note to each kitchen, and places one order per day.

not one lunch. all of them.

no menus to paste. no options to weigh. no decision to make. be super lazy. make the ai
order my protons.

it still shows you the 5 ranked runners-up, because a nutritionist that won't show its
work is just a vending machine with opinions. every single one clears **40g protons** or
it does not make the list. every single one lands inside the window.

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

it will not write your name, your delivery address, your email or your order ids into any
file in this repo. this thing is public. the order-detail page it scrapes prints your name
and the street address right next to the food, so the skill is told, explicitly, to parse
the food fields and throw the rest away. restaurants, dishes, prices and star ratings are
food data. everything else stays out.

## the thing the log actually told us

forty orders in, averaged by cuisine:

| cuisine | orders | avg |
|---|---|---|
| indian | 2 | **4.00** |
| east / southeast asian | 12 | **3.33** |
| latin | 3 | 3.00 |
| mediterranean / middle eastern | 9 | **2.78** |

which is funny, because the skill used to say mediterranean "lands well". it does not.
that belief came from a list of components you said you liked — tahini, hummus, feta,
olives — and not from a single rating. nine orders later: two 2s, four 3s, one 4.

the components are real. the kitchens are the problem. both of the 2s came with a review
attached and both reviews say the same thing — the pita was dry, the shawarma was cooked
a day ago. that is a food-holding problem, and it is invisible from the menu text.

so: still order the hummus. stop picking the restaurant because it has hummus.

## the rules

| rule | ruling |
|---|---|
| the ceiling | HARD. derived from the stipend, verified against tax. we do not go over. |
| 40g protons | floor, not target. 38g is not "close enough" |
| calories | strongest soft goal. never enough to drop under 40g. we are not cutting that hard. |
| one (1) real item | three $5 sides in a trenchcoat is not an entree |
| no broccoli base | do not ask |
| grilled not breaded | every time the menu offers the choice |
| lettuce | filler. order it light. it is not food, it is packing peanuts |
| your reviews | outrank the calorie count. a 4 you never wrote a word about beats a 3 with a paragraph |
| tofu | never the protein. the only 1-star in the log and we all know why |
| rotation | no same format twice in a row. it is a nutritionist, not a vending machine |
| loved food | exempt. you rated it 4, that means go back, same dish included |
| carbs | wrap/sandwich/rice base gets ONE slot every three orders. sometimes. not weekly. |
| grains | quinoa first, brown rice second. white rice is a last resort, not a default |
| every open day | all of them get ordered. an unclaimed day is a day of stipend deleted |
| credit card | never. a card prompt means the order is wrong, not that you owe money |

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

## brotein log

`order-history.md` is where the gains get tracked. every order logged. every verdict
recorded. a cancelled order is not an order and does not get logged — the ledger is what
you actually ate, not what you almost ate.

it currently holds **40 orders**, dec 2025 through sep 2026, with what each one cost and
what you thought of it.

it also tracks **components**, not just dishes. "i like the tahini and the feta, lose the
lettuce" generalizes to every menu on earth. "i liked the Scali salad" generalizes to
exactly one salad.

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
that no amount of protein arithmetic can see coming, which is the entire reason the
ratings outrank the calorie count when it builds a slate.

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

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

so i made the machine do it

## the problem

a stipend is use it or lose it. spend $16 like a coward and $4 evaporates into the void.
spend $18.75 and ezCater asks for your credit card like some kind of animal. the good
zone is narrow, it moves with sales tax, and you have to rediscover it every single day
while hungry and staring at a wall of sandwiches. unacceptable.

## what it does

`/feed-me`. that's it. that's the whole interface.

it opens the ordering site in a real browser, reads every menu on offer that day, prices
out the protein upcharges hiding behind each item's option modal, builds the order, types
a polite note to the kitchen, checks the utensils box, and places it. then it tells you
what it got you and what day it lands.

no menus to paste. no options to weigh. no decision to make. be super lazy. make the ai
order my protons.

it still shows you the 5 ranked runners-up, because a nutritionist that won't show its
work is just a vending machine with opinions. every single one clears **40g protons** or
it does not make the list. every single one lands inside the window.

it will also tell you about the perfect item sitting $0.04 over the ceiling. so you can
think about it. forever.

it answers in greentext. this was not a stylistic accident, it was a requirement. the
numbers are still exact. the voice is just insufferable on purpose.

the note it types into the special-instructions box is not in greentext, because a human
being reads that box during a lunch rush. it says please. it asks for extra pickled
mustard greens *if you can*. it is the single most polite thing in this repo.

## the money

the stipend covers subtotal + tax and the cart shows you subsidy and total before you
commit, so it builds to the largest order that still reads **total $0.00** and stops
there. at $20.00 and 7% tax the arithmetic wall is $18.68 — because $18.69 lands on
exactly $20.00 with zero margin, and zero margin is how you end up holding a credit card.

if checkout ever asks for a card it backs the order down instead of paying. it will not
spend your money. it only spends theirs.

## what it will not do

it will not type your password. if it hits the login wall it stops, tells you the browser
window is sitting right there, and waits for you to do the one (1) thing that is actually
yours to do.

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
| rotation | no same format twice in a row. it is a nutritionist, not a vending machine |
| carbs | wrap/sandwich/rice base gets ONE slot every three orders. sometimes. not weekly. |

## the rotation thing

the failure mode of a robot with a calorie rule is that it finds the one lowest-calorie
qualifying salad and then serves you that salad until you die. so every order gets tagged
with a **format** — `salad`, `carb-base`, `protein-plate`, `bowl-no-grain`, `soup-forward` —
and the same format cannot go twice in a row. the five options it hands back have to span
at least three of them.

variety is allowed to spend about 150 calories to break a repeat. past that, calories win,
but it has to say out loud that it is repeating itself and why.

## brotein log

`order-history.md` is where the gains get tracked. every order logged. every verdict
recorded. a cancelled order is not an order and does not get logged — the ledger is what
you actually ate, not what you almost ate.

- **liked it** → starts showing up more
- **ok with mods** → right shape, wrong build. comes back with the fix stapled on
- **hated it** → goes on the **Never Again** list, never to be spoken of again
- **said nothing** → sits at `pending` forever, quietly judging you

it also tracks **components**, not just dishes. "i like the tahini and the feta, lose the
lettuce" generalizes to every menu on earth. "i liked the Scali salad" generalizes to
exactly one salad.

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

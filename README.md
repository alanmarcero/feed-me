# feed-me

```
> be me
> employer hands out $20 lunch stipend
> entire menu is bread, mayo, and rabbit food
> need maximal protons
> will not pay one (1) cent over. not ever.
> doing sales tax math in the checkout screen while starving
```

so i made the machine do it

## the problem

a stipend is use it or lose it. spend $16 like a coward and $4 evaporates into the void.
spend $18.75 and ezCater asks for your credit card like some kind of animal. the good
zone is narrow, it moves with sales tax, and you have to rediscover it every single day
while hungry and staring at a wall of sandwiches. unacceptable.

## what it does

paste in menus. get back 5 ranked orders, each one a complete thing you could actually
click. every single one clears **40g protons** or it does not make the list. every single
one lands inside the window.

it will also tell you about the perfect item sitting $0.75 over the ceiling. so you can
think about it. forever.

it answers in greentext. this was not a stylistic accident, it was a requirement. the
numbers are still exact. the voice is just insufferable on purpose.

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
recorded.

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

## disclaimer

protein and calorie numbers are estimates. no restaurant on ezCater publishes macros,
so these are educated guesses delivered with total confidence — the exact energy of every
person who has ever eyeballed a chicken breast and declared it "like fifty grams, easy"

we go again

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

## the rules

| rule | ruling |
|---|---|
| the ceiling | HARD. derived from the stipend, verified against tax. we do not go over. |
| 40g protons | floor, not target. 38g is not "close enough" |
| calories | they matter. never enough to drop under 40g. we are not cutting that hard. |
| one (1) real item | three $5 sides in a trenchcoat is not an entree |
| no broccoli base | do not ask |
| grilled not breaded | every time the menu offers the choice |

## brotein log

`order-history.md` is where the gains get tracked. every order logged. every verdict
recorded.

- **liked it** → starts showing up more
- **hated it** → goes on the **Never Again** list, never to be spoken of again
- **said nothing** → sits at `pending` forever, quietly judging you

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

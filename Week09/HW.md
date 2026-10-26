# HOMEWORK — Week 09: make the generator yours

**Due before Tuesday's class.** You need a working page on Tuesday — Week 10
bolts buttons onto whatever you hand in, so an unfinished one costs you that
class too.

Take Thursday's page and make it about something you actually care about. Not a
gig unless you want one: a random recipe card, a random outfit, a random
workout, a daily horoscope, a shop order, a random album from your collection.
**Same code, different nouns.**

## What it has to have

- [ ] **At least three arrays** of your own, used to build the page. Different
      lengths — that is what `.length` is protecting you from.
- [ ] **`pickRandom()` and `randomBetween()`**, copied in and used. If you
      wrote your own versions, better.
- [ ] **Real arithmetic.** Something is calculated from something else: a
      total, a per-person share, a percentage, a duration. Not a number you
      typed and then printed.
- [ ] **Money or measurements displayed with `.toFixed(2)`** — and not used
      halfway through the maths, because it hands back a string.
- [ ] **Two booleans you work out rather than type**, each from a comparison.
      `isSoldOut`, `isLongRecipe`, `needsAJacket`, `isOverBudget`.
- [ ] **An `if / else if / else`** choosing between three messages, where all
      three are reachable. Reload until you have seen each one.
- [ ] **A ternary**, used for a value rather than a block of work.
- [ ] **A template literal** building a sentence out of at least three
      variables.
- [ ] **A `for` loop** producing a list on the page, with `innerHTML`.
- [ ] **`%`** doing something — striped rows, every third item, odd or even.
- [ ] **Everything on the page via `textContent` or `innerHTML`.** Nothing
      typed into the HTML that the JavaScript should be producing.

## Two things worth the extra ten minutes

- [ ] **A random image.** Three or four of your own files in an `img/` folder
      and the same pattern as a random name:

      ```js
      const images = ['img/one.jpg', 'img/two.jpg', 'img/three.jpg']
      document.querySelector('#art').src = pickRandom(images)
      ```

      This is the bit that makes the page look like a real thing rather than an
      exercise.

- [ ] **A random accent colour** with
      `document.body.style.setProperty('--accent', pickRandom(colours))`, and a
      stylesheet built on `var(--accent)` so one line re-themes the page.

## Check it before you push

Reload it **ten times in a row** and read what comes out each time.

- Did anything say `undefined`, `NaN` or `[object Object]`?
- Did a number come out with eleven decimal places?
- Did a sentence read strangely with a particular random combination — "1
  tickets left", "a 0 minute workout"?
- Did a price ever come out negative?

**Edge cases are not bad luck, they are the job.** Fix the ones you can and
write a comment about any you cannot — bring those to Tuesday, they make good
examples for the room.

## If you got lost

- Tuesday's finished code is in
  `Week9_2/Math_Random_Content/StarterFiles/index.html` — it is the starter for
  Thursday precisely so nobody is stuck.
- Both lessons' notes show every step with the code at that point.
- Read the error in the console **first**, then
  [TROUBLESHOOTING.md](../TROUBLESHOOTING.md). The most common three this week:
  `Cannot set properties of null` (script in the head, or a misspelled id),
  `x is not defined` (capital letter in the wrong place), and
  `Assignment to constant variable` (it needs to be `let`).

## Submitting

```bash
git add .
git commit -m "week 9 homework"
git push
```

Check on github.com that your `img/` folder actually went up if you used one —
a missing image is the most common thing to discover in front of the class.

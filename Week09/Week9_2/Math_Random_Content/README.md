# Week 09 — Math, `Math.random()` & Random Content

*Arrays, the Math object, and a page that invents a different show every time it loads · ~100 min*

> **Following along at home?** Bring your file from Tuesday. If it is in a
> strange state, copy `StarterFiles/` instead — it is exactly where Tuesday
> ended, plus one new empty element for the running order.

On Tuesday you wrote the **rules** that produce a show page. The facts at the
top were still typed in by hand.

Today the facts go too. Reload the page and it is a different band, a different
venue, a different price, a different colour. **The rules never change — only
what you feed them.** That is most of what programming is.

Three new ideas:

- **arrays** — one variable holding a list of values
- **`Math.random()`** — and the small amount of arithmetic that turns it into
  something useful
- **the `for` loop** — doing the same thing once per item in a list

Stuck? Read the error first, then [TROUBLESHOOTING.md](../../../TROUBLESHOOTING.md).

---

## Steps

1. [Arrays — a list in one variable](#step-1)
2. [`Math.random()`, and what it actually gives you](#step-2)
3. [From a random decimal to a random item](#step-3)
4. [Two functions you will use forever](#step-4)
5. [A different show every reload](#step-5)
6. [The rest of the Math object](#step-6)
7. [A `for` loop over the lineup](#step-7)
8. [Modulo, and every other row](#step-8)
9. [A random colour scheme](#step-9)
10. [In-class exercise](#step-10)

---

<a id="step-1"></a>

## Step 1 — Arrays — a list in one variable

Everything on Tuesday held exactly one value. An **array** holds a list:

```js
const venues = ['The Fillmore East', 'Rough Trade', 'Brooklyn Steel', 'Market Hotel']
```

Square brackets around the whole thing, commas between items. The items can be
any type — strings here, but numbers, booleans or a mixture all work.

```js
console.log(venues.length)   // 4 -- how many items
console.log(venues[0])       // 'The Fillmore East'
console.log(venues[3])       // 'Market Hotel'
console.log(venues[4])       // undefined
```

**Two things, and the second one causes most array bugs:**

- **You get an item by its index, in square brackets.**
- **Indexes start at 0.** A four-item array has indexes 0, 1, 2 and 3. So
  **the last index is always `length - 1`**, never `length`. Asking for
  `venues[4]` is not an error — JavaScript hands you `undefined`, which goes on
  to break something further down where the real cause is no longer visible.

Write out the lists our show will be built from:

```js
const headliners = ['Bad Moon Rising', 'Velvet Static', 'Dial Tone', 'Mountain Time', 'Kerosene Club', 'The Paper Cuts']
const supports = ['Low Ceiling', 'The Ferry Lights', 'Tiny Mirrors', 'Garden State Radio']
const djs = ['DJ Shortwave', 'DJ Halfmoon', 'DJ Ferry']
const venues = ['The Fillmore East', 'Rough Trade', 'Brooklyn Steel', 'Market Hotel']
const nights = ['Thu 12 Nov', 'Fri 13 Nov', 'Sat 14 Nov']
```

Use your own bands. It is more fun and it is the same code.

> **`const` on an array still works.** You cannot replace the whole array with
> a different one, but you can change what is inside it. `const` locks the
> name to the list, not the list to its contents.

---

<a id="step-2"></a>

## Step 2 — `Math.random()`, and what it actually gives you

```js
console.log(Math.random())
```

Reload a few times. You get things like `0.8347261...`, `0.1120049...`,
`0.9981...`

**`Math.random()` returns a decimal between 0 and 1.** Specifically: 0 is
possible, and 1 is not — it is always *less than* 1. Remember that; it is the
reason the next step works.

That is the only random thing JavaScript gives you. Everything else — a random
number between 3 and 10, a random item from a list, a random colour — is that
one decimal plus arithmetic you do yourself.

`Math` is an **object**: a bundle of related tools under one name, reached with
a dot. You have met this shape already with `.toFixed()` and `.toUpperCase()`.
Proper objects are coming in a couple of weeks.

```js
console.log(Math.PI)   // 3.141592653589793 -- not a method, just a value
```

---

<a id="step-3"></a>

## Step 3 — From a random decimal to a random item

We want an index: for a four-item array, a whole number that is 0, 1, 2 or 3.

Build it one piece at a time and log each one:

```js
console.log(Math.random())                  // 0.8347...  a decimal under 1
console.log(Math.random() * venues.length)  // 3.3389...  a decimal under 4
console.log(Math.floor(Math.random() * venues.length))   // 3
```

**`Math.floor()` always rounds down**, so a decimal under 4 becomes 0, 1, 2 or
3 — exactly the valid indexes, and never 4.

```js
const index = Math.floor(Math.random() * venues.length)
const venue = venues[index]
console.log(venue)
```

### Why `floor` and not `round`

`Math.round()` rounds to the *nearest* whole number. Feed it `3.7` and you get
`4` — an index that does not exist, so you get `undefined` and a page with a
hole in it.

It is also unfair: with `round`, indexes 0 and 3 only come up from half as wide
a range as 1 and 2, so the middle of your list gets picked twice as often.

**`Math.floor()` for indexes. Every time.**

---

<a id="step-4"></a>

## Step 4 — Two functions you will use forever

We are about to do that six times. Writing it out six times is how typos get
in, so wrap it up once:

```js
function pickRandom(list) {
  const index = Math.floor(Math.random() * list.length)
  return list[index]
}
```

- **`function`** declares it, then the name, then the **parameter** in
  brackets — `list` is a placeholder for whatever array you hand in.
- **`return`** sends a value back to wherever the function was called. Without
  it the function does its work and gives you nothing.
- Calling it is the name plus brackets, with the real value inside:

```js
console.log(pickRandom(venues))
console.log(pickRandom(headliners))
```

One function, any list. **That is the whole point of a parameter.**

The second one gets you a whole number in a range:

```js
function randomBetween(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min
}

console.log(randomBetween(1, 6))    // a dice roll
console.log(randomBetween(12, 45))  // a ticket price
```

Read it from the inside out: `max - min + 1` is **how many numbers are in the
range** (1 to 6 is six numbers, not five — that is what the `+ 1` is for).
Multiply the random decimal by that, floor it to get 0 up to 5, then `+ min`
slides the whole range up to start at 1.

Copy these two into every project. Everybody has a version of them.

---

<a id="step-5"></a>

## Step 5 — A different show every reload

Now go back to Tuesday's facts and replace the right-hand side of each line.
**The variable names do not change, so nothing below them has to change
either:**

```js
const headliner = pickRandom(headliners)
const support = pickRandom(supports)
const venue = pickRandom(venues)
const doors = `${pickRandom(nights)}, doors 8pm`

const ticketPrice = randomBetween(12, 45) + 0.5
const capacity = randomBetween(120, 400)
const ticketsSold = randomBetween(capacity - 25, capacity)

let quantity = randomBetween(1, 4)
```

Save and reload. Reload again. **Every one of Tuesday's rules still works** —
the subtotal, the service fee, the student discount, the status message — on
facts that did not exist until the page loaded.

Note `ticketsSold` is deliberately close to `capacity`, so reloading a few
times gets you all three status messages. Watch for the sold-out one; it comes
up roughly once in twenty-five.

**This is the idea worth taking away from today.** You did not write a page
about one gig. You wrote something that can describe any gig, and then handed
it a gig.

---

<a id="step-6"></a>

## Step 6 — The rest of the Math object

Four more, each with an actual job on this page:

```js
// round: to the NEAREST whole number -- "tickets from $36"
const posterPrice = Math.round(ticketPrice)

// floor: always DOWN -- how many tickets $100 really buys
const ticketsForAHundred = Math.floor(100 / ticketPrice)

// ceil: always UP -- 50 people and 14 seats a van is four vans, not three
const vansNeeded = Math.ceil(ticketsSold / 14)

// max: hands back the largest of its arguments -- never show a negative
const safeTicketsLeft = Math.max(0, ticketsLeft)
```

**The difference between them is only ever about what you are counting.**
Money you round. Things you can only have whole ones of, where running out is
not an option — vans, pizzas, pages of results — you `ceil`. Things where a
part does not count — how many whole tickets you can afford — you `floor`.

`Math.min()` is the mirror of `Math.max()`. Together they are how you keep a
number inside a range:

```js
const volume = Math.min(100, Math.max(0, someNumber))   // clamped to 0–100
```

Put the new values on the page:

```js
const finePrint = `Tickets from $${posterPrice} · ${safeTicketsLeft} of ${capacity} remaining · ${vansNeeded} shuttle vans booked`
document.querySelector('#fine-print').textContent = finePrint
```

---

<a id="step-7"></a>

## Step 7 — A `for` loop over the lineup

Three acts, one list, and the same markup for each:

```js
const lineup = [pickRandom(djs), support, headliner]
```

A **`for` loop** runs a block once per item. Its header has three parts,
separated by semicolons:

```js
for (let i = 0; i < lineup.length; i++) {
  console.log(lineup[i])
}
```

| Part | Means |
|---|---|
| `let i = 0` | start counting at 0 — the first index |
| `i < lineup.length` | **keep going while this is true.** Less than, never less-than-or-equal |
| `i++` | add 1 to `i` after each pass |

`i` is just a variable, and inside the loop `lineup[i]` is the current item.

**`<` and not `<=` is the thing to get right.** A three-item array has indexes
0, 1, 2. `i <= lineup.length` would run a fourth time with `i` as 3, and
`lineup[3]` is `undefined`.

Now build the markup instead of logging it:

```js
let lineupHTML = ''

for (let i = 0; i < lineup.length; i++) {
  const setTime = 8 + i        // 8pm, 9pm, 10pm
  lineupHTML += `<li><span class="time">${setTime}pm</span>${lineup[i]}</li>`
}

document.querySelector('#lineup').innerHTML = lineupHTML
```

Three new things in those six lines:

- **`lineupHTML` starts as an empty string and is a `let`**, because it changes
  on every pass.
- **`+=` means "add this onto the end".** `a += b` is shorthand for
  `a = a + b`. Each pass glues another `<li>` onto the string.
- **`.innerHTML`, not `.textContent`.** `textContent` would put the literal
  characters `<li>` on the page. `innerHTML` reads the string as markup and
  builds real elements from it.

> **A warning that matters later:** only ever hand `innerHTML` a string you
> built yourself. Putting text that came from a user into `innerHTML` is how
> websites get attacked, because any `<script>` tag in it will run. Your own
> template literal is fine.

---

<a id="step-8"></a>

## Step 8 — Modulo, and every other row

`%` is the **modulo** operator: the remainder after division.

```js
console.log(7 % 2)   // 1
console.log(8 % 2)   // 0
console.log(9 % 3)   // 0
```

So `i % 2` is `0, 1, 0, 1, 0...` as `i` counts up — which makes
**`i % 2 === 0`** true on every other pass. That is how striped rows are done,
everywhere, forever:

```js
const stripe = i % 2 === 0 ? 'stripe' : ''
lineupHTML += `<li class="${stripe}"><span class="time">${setTime}pm</span>${lineup[i]}</li>`
```

The CSS for `.stripe` is already in your stylesheet. A ternary writing a class
name into a template literal — **your JavaScript choosing which CSS applies**
— is a pattern you will use for the rest of the course.

Modulo is also how you ask "is this even?" (`n % 2 === 0`), "every third
item?" (`i % 3 === 0`), or "what is left over?" when dividing anything up.

One more, to make the headliner bold:

```js
const isHeadliner = i === lineup.length - 1
const act = isHeadliner ? `<span class="headline-act">${lineup[i]}</span>` : lineup[i]
```

`lineup.length - 1` is the last index — the thing from step 1.

---

<a id="step-9"></a>

## Step 9 — A random colour scheme

Remember `--accent` from the CSS variables lesson? JavaScript can set one:

```js
const accents = ['#e0504f', '#2f6f4e', '#3b4c9c', '#b4622a', '#6b3f7a']

document.body.style.setProperty('--accent', pickRandom(accents))
```

Reload. The poster changes colour, **and so does everything else that uses
`var(--accent)`** — because you changed the variable, not the rule.

One line, and it is the clearest demonstration you will get of why variables
were worth learning in the first place.

Finish with a random tagline, using everything from today:

```js
const taglines = [
  'Strictly 18+. No re-entry.',
  'Late set. Bring earplugs.',
  'All ages until 10pm.',
  'Sold out three times running.',
]

document.querySelector('#tagline').textContent = pickRandom(taglines)
```

---

<a id="step-10"></a>

## Step 10 — In-class exercise

1. **Your own arrays.** Replace my bands and venues with yours. Make the lists
   different lengths and check nothing breaks — that is what `.length` is
   protecting you from.
2. **A fourth act.** Add another name to `lineup` and confirm the set times,
   the stripes and the bold headliner all still come out right. If any of them
   is wrong, something is hard-coded that should be worked out.
3. **A random image.** The same pattern with a `src`:

   ```js
   const posters = ['img/poster-1.jpg', 'img/poster-2.jpg', 'img/poster-3.jpg']
   document.querySelector('#poster').src = pickRandom(posters)
   ```

   Add an `<img id="poster">` to the markup, drop three images in an `img/`
   folder, and you have random artwork. **This is the one to take into your
   own site.**
4. **Harder:** make the service fee depend on the price — 15% on tickets under
   $20, 10% on anything more. You have `if / else` and you have a number that
   is different on every load.

---

## What you can do now

- make an **array**, read items by index, and use `.length`
- get a random item from any list with
  `Math.floor(Math.random() * list.length)`
- write a **function** with a parameter and a `return`, and reuse it
- choose between `Math.round`, `Math.floor` and `Math.ceil` based on what you
  are counting
- loop over a list with **`for`**, building a string of markup as you go
- use **`%`** for every-other-row, every-third-item and is-it-even
- set a **CSS custom property** from JavaScript and re-theme a whole page

**Next week: the DOM properly.** Everything so far happens once, when the page
loads. Next we make it happen when somebody *clicks* — showing and hiding
content, and toggling classes on and off.

**Homework:** [HW.md](../../HW.md).

# Week 09 — JavaScript Variables With a Purpose

*Numbers, strings and booleans building a show page that works out its own prices · ~100 min*

> **Following along at home?** Copy `StarterFiles/` into your own repo to
> start — the HTML and the CSS are already written, and the `<script>` tag at
> the bottom is empty. Everything you do today happens inside that tag.

Today is your first real JavaScript. Not "here are the data types," but: here
is a ticket page where **nothing is typed in by hand**. Every price, every
message, every line of the order is worked out by the code.

Open `StarterFiles/index.html` in your browser now. It is a beautifully styled
page with **nothing in it** — eight empty elements waiting for values. By the
end of class they will all be full, and changing one number at the top will
change the whole page.

Three types do all the work:

| Type | Looks like | Does |
|---|---|---|
| **number** | `18.5`, `250` | arithmetic |
| **string** | `'Bad Moon Rising'` | text, and building sentences |
| **boolean** | `true`, `false` | deciding what happens |

Stuck? Read the error first, then [TROUBLESHOOTING.md](../../../TROUBLESHOOTING.md).

---

## Steps

1. [Where JavaScript goes, and the console](#step-1)
2. [Three types, and `const`](#step-2)
3. [`let`, and the one value that changes](#step-3)
4. [Numbers doing real work](#step-4)
5. [Strings, and the backtick](#step-5)
6. [Getting it onto the page](#step-6)
7. [Booleans you work out rather than type](#step-7)
8. [Choosing a message](#step-8)
9. [In-class exercise](#step-9)

---

<a id="step-1"></a>

## Step 1 — Where JavaScript goes, and the console

Copy `StarterFiles/` into your own homework repo:

```
show-page/
├── index.html        ← markup and the empty <script>
└── css/
    └── style.css     ← done, do not touch it today
```

Look at the bottom of `index.html`:

```html
    <script>
      // We write all of this together in class.
    </script>
  </body>
</html>
```

**The script is at the end of the `<body>`, not in the `<head>`, and that
matters.** The browser reads your file top to bottom. A script in the `<head>`
runs *before* the elements below it exist — so when it goes looking for the
total, there is nothing there yet and it fails. Put the script last and
everything it needs is already on the page.

Write one line inside the script tag and save:

```js
console.log('hello from the show page')
```

Now open the page and press **Cmd + Option + J** (Mac) or **F12** (Windows) to
open devtools, and click the **Console** tab.

`console.log()` prints to that panel. It does not appear on the page, and your
visitors never see it. **It is how you check your work for the rest of your
life as a developer** — leave the console open from here on.

> **Two slashes make a comment.** `// like this` — JavaScript ignores it.
> Comments are for the human reading the code later, who is almost always you.

---

<a id="step-2"></a>

## Step 2 — Three types, and `const`

A **variable** is a name for a value. You make one with a keyword, a name, an
`=`, and the value:

```js
const headliner = 'Bad Moon Rising'
```

Our show has facts. Type them all in, and notice you can tell the type just by
looking:

```js
const headliner = 'Bad Moon Rising'   // string -- it has quotes
const support = 'The Paper Cuts'      // string
const venue = 'The Fillmore East'     // string
const doors = 'Fri 13 Nov, doors 8pm' // string

const ticketPrice = 18.5              // number -- no quotes
const capacity = 250                  // number
const ticketsSold = 243               // number
const serviceFeeRate = 0.12           // number -- 12%, as a decimal
const studentPrice = 5                // number

const isStudent = true                // boolean -- true or false, nothing else
```

Check a few:

```js
console.log(typeof headliner)    // 'string'
console.log(typeof ticketPrice)  // 'number'
console.log(typeof isStudent)    // 'boolean'
```

`typeof` tells you what something is. You will use it the first time a value
behaves strangely.

**Things worth knowing now:**

- **Numbers have no quotes.** `18.5` is a number; `'18.5'` is a string that
  looks like one, and it will not do arithmetic. This causes more beginner bugs
  than anything else.
- **JavaScript has one number type.** `250` and `18.5` are both just numbers —
  the whole one is called an **integer**, the decimal one a **float**, but you
  do not have to declare which.
- **Booleans are `true` and `false`** — no quotes, lowercase. `'true'` with
  quotes is a string and is not the same thing at all.
- **Naming:** start with a letter, no spaces, and use `camelCase` —
  `ticketPrice`, not `ticket price` or `ticketprice`. Booleans conventionally
  start with `is` or `has`: `isStudent`, `isSoldOut`.

---

<a id="step-3"></a>

## Step 3 — `let`, and the one value that changes

Try reassigning one of the values you just made:

```js
ticketPrice = 20
```

Save, reload, read the console:

```
TypeError: Assignment to constant variable.
```

**`const` means the value never changes after you set it.** That is a feature.
Most of your values never should change, and this makes the computer enforce
it instead of you remembering.

When something *does* need to change, use **`let`**:

```js
let quantity = 3
```

The number of tickets in the basket is the one thing on this page that is
genuinely going to vary, so it is the one `let`.

**The rule, and you can follow it for years:** use `const` by default. Switch
to `let` only when you get the error above and the reassignment was
deliberate.

> **You will see `var` everywhere.** Every tutorial written before about 2017
> uses it, and it still works. It has some genuinely confusing scoping
> behaviour that `let` and `const` fixed. Read it, do not write it.

---

<a id="step-4"></a>

## Step 4 — Numbers doing real work

The operators are the ones from algebra:

```js
const subtotal = ticketPrice * quantity
console.log(subtotal)   // 55.5
```

| | |
|---|---|
| `+` `-` `*` `/` | add, subtract, multiply, divide |
| `%` | **modulo** — the remainder after division. `7 % 2` is `1` |
| `( )` | grouping, and it beats everything else |

Build the order up one line at a time, each line using the one above it:

```js
const subtotal = ticketPrice * quantity        // 55.5
const serviceFee = subtotal * serviceFeeRate   // 6.66
const total = subtotal + serviceFee            // 62.16
```

`console.log` each one as you go. **Never write four lines of maths and then
look.**

### Two traps, both today

**1. Decimals are not exact.** Try this in the console:

```js
console.log(0.1 + 0.2)   // 0.30000000000000004
```

That is not a bug in your code. Computers store decimals in binary, and some
fractions do not fit — the same way `1/3` does not fit in decimal. It barely
matters until you are showing money, which is why:

```js
console.log(total.toFixed(2))   // '62.16'
```

**`.toFixed(2)`** rounds to two decimal places. Note the quotes in that
output — **it hands you back a string, not a number.** So use it when you are
about to *display* the value, never halfway through your maths.

**2. A plus sign between a string and a number glues them together.**

```js
console.log('3' + 4)     // '34'   <- not 7
console.log(3 + 4)       // 7
```

If a total ever comes out as `'5550'` instead of `55.5`, one of your numbers
is secretly a string. Check it with `typeof`.

---

<a id="step-5"></a>

## Step 5 — Strings, and the backtick

You can join strings with `+`:

```js
const billing = headliner + ' + ' + support
```

That works, and it gets unreadable fast — count the quotes and spaces in a
line with four values in it. So:

```js
const billing = `${headliner} + ${support}`
```

**Backticks** — the key above Tab, not a quote mark — let you write the
sentence normally and drop values in with **`${ }`**. This is a *template
literal*, and it is how you will write nearly every string from now on.

```js
const whereWhen = `${venue} · ${doors}`
const priceLine = `${quantity} × $${ticketPrice.toFixed(2)}  —  $${subtotal.toFixed(2)}`
```

Look closely at `$${ticketPrice.toFixed(2)}`: the first `$` is a literal
dollar sign being printed, the `${` is the start of a slot. They are different
things that happen to look alike.

### String methods

A **method** is a function attached to a value, called with a dot:

```js
console.log(headliner.toUpperCase())  // 'BAD MOON RISING'
console.log(headliner.length)         // 16 -- no brackets, it is not a method
console.log(venue.trim())             // removes spaces at either end
```

`.length` counts **characters, including spaces**. Use it to check a headline
is not too long for its box.

### When a value is missing

Suppose the support act has not been announced:

```js
const support = ''                              // an empty string
const supportName = support || 'support TBA'
```

**`||` means "or".** It hands back the first value that is *truthy* — that is,
not empty, not zero, not `false`. An empty string is "falsy", so `supportName`
becomes `'support TBA'`.

That one-liner is how you stop a page showing a blank space where a name
should be, and you will use it constantly.

---

<a id="step-6"></a>

## Step 6 — Getting it onto the page

Everything so far has gone to the console. Time to put it where people can see
it.

```js
document.querySelector('#billing').textContent = billing
```

**Read it left to right:** in the document, find the element whose id is
`billing`, and set the text inside it to our `billing` variable.

That is the whole explanation for today. *Why* it works, what else you can do
with it, and how to make it happen on a click rather than on load — that is
next week, and it is the rest of the course.

Three things to get right:

- **`#` means "the id".** Same as in CSS. `querySelector('#total')` finds
  `<strong id="total">`.
- **`.textContent` is the text inside the element.** Setting it replaces
  whatever was there.
- **The element must exist before this line runs**, which is why the script is
  at the bottom of the body. If you get `Cannot set properties of null`, your
  script is in the wrong place or the id is misspelled.

Fill in the top of the page:

```js
document.querySelector('#billing').textContent = billing
document.querySelector('#where-when').textContent = whereWhen
document.querySelector('#price-line').textContent = priceLine
document.querySelector('#fee-line').textContent = feeLine
document.querySelector('#total').textContent = `$${total.toFixed(2)}`
```

Save, reload. **The page is real now.** Go back up to the top, change
`quantity` to `5`, and reload — every line updates, because every line is
worked out rather than typed.

---

<a id="step-7"></a>

## Step 7 — Booleans you work out rather than type

`isStudent` we typed by hand. Most booleans you should not:

```js
const ticketsLeft = capacity - ticketsSold   // 7
const isSoldOut = ticketsLeft === 0          // false
const isAlmostSoldOut = ticketsLeft < 10     // true
```

**A comparison is a boolean.** `ticketsLeft < 10` does not "check" anything —
it *is* `true` or `false`, and you can store it in a variable like any other
value. Log them and look.

| | |
|---|---|
| `===` | exactly equal — same value **and** same type |
| `!==` | not equal |
| `>` `<` `>=` `<=` | greater, less, and or-equal versions |

### Always `===`, never `==`

```js
console.log(5 === '5')   // false -- a number is not a string
console.log(5 == '5')    // true  -- == quietly converts one first
```

`==` converts types behind your back to make things match, which sounds
helpful and causes bugs you cannot see. **Use `===` every time.** If you
genuinely need to compare a number to a string, convert it yourself with
`Number()` so it is visible in the code.

Why bother working booleans out instead of typing `true`? Because
`ticketsSold` will change, and when it does, `isSoldOut` is already correct.
Nobody has to remember to update it. **A value you calculate cannot go stale.**

---

<a id="step-8"></a>

## Step 8 — Choosing a message

Three possible statuses, and exactly one should print:

```js
let status

if (isSoldOut) {
  status = 'Sold out — join the waiting list'
} else if (isAlmostSoldOut) {
  status = `Hurry — only ${ticketsLeft} tickets left`
} else {
  status = 'Tickets available'
}

document.querySelector('#status').textContent = status
```

**The first true branch wins and the rest are skipped entirely.** Order
matters: if `else if (isAlmostSoldOut)` came first, a sold-out show would say
"only 0 tickets left" and never reach the sold-out message.

Note `status` is declared with `let` and no value, then assigned inside the
branches. That is normal and it is exactly what `let` is for.

Change `ticketsSold` to `250` and reload. Then `100`. Three different pages,
one set of rules.

### The ternary — an if/else that fits on one line

```js
const discount = isStudent ? studentPrice : 0
```

Read it as a question: **is `isStudent` true? If so `studentPrice`, otherwise
`0`.** The `?` and `:` are the whole syntax.

It is the right tool when you are choosing between two *values*. It is the
wrong tool when each branch does several things — use a real `if` then.

```js
const discountLine = isStudent
  ? `Student discount  —  −$${discount.toFixed(2)}`
  : ''
document.querySelector('#discount-line').textContent = discountLine
```

When `isStudent` is `false` that line becomes an empty string, and the CSS
rule `.line:empty { display: none }` makes the whole row disappear. **Your
JavaScript and your CSS working together** — that is the real shape of a front
end.

Now finish the total properly, with the discount in it:

```js
const total = subtotal + serviceFee - discount   // 57.16
```

---

<a id="step-9"></a>

## Step 9 — In-class exercise

Your page works. Now prove you can change it. In order of difficulty:

1. **Add a booking fee** of `$2.50` per ticket — a new `const`, into the total,
   and its own line on the page.
2. **A second boolean** of your choosing: `isMember`, `hasEarlyEntry`,
   `isRefundable`. Compute something from it and show a different message.
3. **A sold-out page.** Set `ticketsSold` to `capacity` and make sure
   everything reads sensibly — status, fine print, all of it. Did anything come
   out wrong? Fix it.
4. **Make the headline behave.** If `billing.length` is over 40 characters,
   show a shorter version instead. You now have everything you need for that
   sentence: `.length`, a comparison, and a ternary.

---

## What you can do now

- make variables with `const` and `let`, and know which to reach for
- tell a **number**, a **string** and a **boolean** apart, and check with
  `typeof`
- do arithmetic, and display money with `.toFixed(2)`
- build sentences with **template literals** and `${ }`
- work out booleans from comparisons instead of typing them
- choose between outcomes with **if / else if / else** and the **ternary**
- put any of it on the page with `textContent`

**Thursday:** the `Math` object, `Math.random()`, and arrays — so the page can
invent a different show every time it loads.

**Homework:** [HW.md](../../HW.md).

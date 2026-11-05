# Week 10 — The DOM & Event Listeners

*Finding elements, listening for clicks, and a page that changes while you are looking at it · ~100 min*

> **Following along at home?** Copy `StarterFiles/` — it is Week 09's finished
> page plus three controls that do not work yet: a quantity stepper, a promo
> code box, and a "Roll a different show" button. By the end of class all three
> work.

Everything you have written so far happens **once**, when the page loads. Today
it happens **when somebody does something**.

That is the whole difference between a document and an application, and it
comes down to three ideas:

1. the **DOM** — the page, as objects your code can read and change
2. **`addEventListener`** — "when this happens, run this function"
3. **one function that draws the page**, which you can run again

Stuck? Read the error first, then [TROUBLESHOOTING.md](../../../TROUBLESHOOTING.md).

---

## Steps

1. [What the DOM actually is](#step-1)
2. [Finding elements](#step-2)
3. [Reading and changing what you found](#step-3)
4. [Your first listener](#step-4)
5. [The event object](#step-5)
6. [One function that draws the page](#step-6)
7. [The stepper](#step-7)
8. [Listening to typing](#step-8)
9. [Roll a different show](#step-9)
10. [In-class exercise](#step-10)

---

<a id="step-1"></a>

## Step 1 — What the DOM actually is

You already used it in Week 09, without being told what it was:

```js
document.querySelector('#total').textContent = totalLine
```

When the browser loads your HTML, it does not keep the text of your file. It
builds a **tree of objects in memory** — one for every element, each with
properties you can read and change. That tree is the **Document Object Model**,
and `document` is the way in.

Put an element in the console and look at it:

```js
console.log(document.querySelector('.poster'))
console.dir(document.querySelector('.poster'))
```

`console.log` shows it as markup, which is comforting and slightly misleading.
**`console.dir` shows it as what it really is** — an object with dozens of
properties: `textContent`, `id`, `className`, `style`, `children`, and on and
on. Scroll that list once. Everything you will ever do to a page is in there.

**The important consequence:** changing the DOM does not change your HTML file.
Edit the page in devtools, reload, and it is back. The file is the starting
instructions; the DOM is the live thing.

---

<a id="step-2"></a>

## Step 2 — Finding elements

Three ways, and you will mostly use the first:

```js
document.querySelector('#total')      // the FIRST match. Takes any CSS selector
document.querySelectorAll('.faq-q')   // ALL matches, as a list
document.getElementById('total')      // older, id only, no # needed
```

**`querySelector` takes any CSS selector you already know.** That is the whole
reason it is the one to use:

```js
document.querySelector('#count')          // by id
document.querySelector('.qty button')     // first button inside .qty
document.querySelector('input[type="text"]')
```

### When it does not find anything

```js
const nothing = document.querySelector('#does-not-exist')
console.log(nothing)   // null
```

**`null` means "I looked and there was nothing there."** It is not an error —
the error comes one line later, when you try to use it:

```
Cannot set properties of null (setting 'textContent')
```

That message means one of three things, every time: the id is misspelled, the
selector is missing its `#` or `.`, or **your script ran before the element
existed**. Which is why the script is at the bottom of the body.

---

<a id="step-3"></a>

## Step 3 — Reading and changing what you found

Once you have an element, its properties are ordinary variables — you can read
them and you can assign to them.

```js
const status = document.querySelector('#status')

console.log(status.textContent)      // read it
status.textContent = 'Doors at 8pm'  // change it
```

The ones worth knowing now:

| Property | Does |
|---|---|
| `.textContent` | the text inside. Safe, and tags arrive as literal characters |
| `.innerHTML` | the markup inside. Reads tags as tags — only give it strings you built |
| `.value` | what is typed in an input. **Inputs use `.value`, not `.textContent`** |
| `.src` / `.href` | the file an image loads, the address a link goes to |
| `.disabled` | `true` or `false` on a button or input |
| `.classList` | the element's classes — Thursday's entire lesson |
| `.style.something` | one inline CSS property. Useful, and usually the wrong tool |

Try a few in the console on the live page:

```js
document.querySelector('#count').textContent = '99'
document.querySelector('#plus').disabled = true
document.querySelector('#promo').value = 'HELLO'
```

**`.disabled = true` is worth a second look.** The property takes a boolean, and
you spent last week learning that a comparison *is* a boolean — so a line like
`button.disabled = quantity === 1` is coming in step 7.

> **About `.style`:** `element.style.color = 'red'` works, and it writes an
> inline style that beats your stylesheet. Fine for one-off values worked out
> at runtime — you already did it with `--accent`. For anything with a *name* —
> hidden, open, active, selected — use a class instead. That is Thursday.

---

<a id="step-4"></a>

## Step 4 — Your first listener

```js
function handlePlus() {
  console.log('somebody clicked plus')
}

document.querySelector('#plus').addEventListener('click', handlePlus)
```

Save, open the console, click the + button. Click it again.

**Three parts, always in this order:**

1. **the element** doing the listening
2. **the event name**, as a string — `'click'`, `'input'`, `'submit'`
3. **the function to run** when it happens

### The one mistake everybody makes

```js
addEventListener('click', handlePlus)     // correct
addEventListener('click', handlePlus())   // wrong
```

**No brackets.** Brackets mean *run this now and use the answer*. So the wrong
version runs `handlePlus` immediately, once, as the page loads — and then hands
`addEventListener` whatever it returned, which is `undefined`. The symptom is
unmistakable once you have seen it: **it fires once on load and never again.**

Without brackets you are handing over the function itself, to be kept and
called later. A function is a value, like a number or a string, and that is a
genuinely strange idea the first time. It is also the idea the whole language
is built on.

> **You will see this written other ways.** `element.onclick = handlePlus`
> works but allows only one listener. `addEventListener` lets you attach
> several, remove them later, and listen to anything. Use it.

### Events worth knowing

| Event | Fires when |
|---|---|
| `click` | clicked — also keyboard-activated, if it is a real `<button>` |
| `input` | an input's value changes, **on every keystroke** |
| `change` | an input's value changed *and* it lost focus |
| `submit` | a form is submitted. Goes on the `<form>`, not the button |
| `mouseover` / `mouseout` | the pointer enters or leaves |
| `keydown` | a key goes down, anywhere |

---

<a id="step-5"></a>

## Step 5 — The event object

When your handler runs, the browser hands it an object describing what
happened. Take it as a parameter — the name `event` is a convention, not a rule:

```js
function handlePlus(event) {
  console.log(event)
}
```

Click and expand it in the console. It is enormous. Three parts matter:

| | |
|---|---|
| `event.target` | the element the event actually happened **on** |
| `event.currentTarget` | the element the listener is **attached to** |
| `event.preventDefault()` | stop the browser's built-in reaction |

**`target` and `currentTarget` are not always the same**, and the day they
differ is the day you lose an hour. If your button contains a `<span>` and
somebody clicks exactly on the span, `target` is the span and `currentTarget`
is still the button. Thursday's FAQ buttons have an icon inside them, so this
comes up for real.

For an input, `event.target.value` is what is currently typed — which is step 8.

---

<a id="step-6"></a>

## Step 6 — One function that draws the page

Here is the problem. Your Week 09 code works out the subtotal, the fee and the
total, writes them onto the page, and **then is finished forever**. Change
`quantity` afterwards and nothing moves, because nothing is watching it.

So: put all of the drawing into a function you can run as many times as you
like.

**Three groups, and knowing which is which is the skill:**

```js
// 1. STATE -- things that can change. `let`, up at the top.
let quantity = 2
let promoCode = ''

// 2. things worked out FROM state -- these go INSIDE render()
// 3. putting things on the page     -- these go INSIDE render() too
function render() {
  const subtotal = ticketPrice * quantity
  const serviceFee = subtotal * serviceFeeRate
  const total = subtotal + serviceFee - discount

  document.querySelector('#count').textContent = quantity
  document.querySelector('#total').textContent = `$${total.toFixed(2)}`
  // ...and the rest of Week 09's bottom block
}

render()   // run it once to draw the page the first time
```

**The rule:** anything calculated from something that can change has to live
inside `render()`. Leave `const subtotal = ticketPrice * quantity` outside and
it is worked out once, from the quantity at load, and never again — a bug that
looks like the button is broken when the maths is.

Nothing should look different yet. Reload and check the page is exactly as it
was. **A refactor you cannot see is a refactor that worked.**

> This is a bigger idea than it looks. "Keep the state in one place, and have
> one function that turns state into what you see" is how every interface
> framework works, React included. You are doing by hand what you will later
> have done for you.

---

<a id="step-7"></a>

## Step 7 — The stepper

Now the payoff. Two handlers, and they have the same two lines in them:

```js
const MIN_TICKETS = 1
const MAX_TICKETS = 6

function handlePlus() {
  if (quantity < MAX_TICKETS) {
    quantity = quantity + 1   // 1. change the state
    render()                  // 2. draw the page again
  }
}

function handleMinus() {
  if (quantity > MIN_TICKETS) {
    quantity = quantity - 1
    render()
  }
}

document.querySelector('#plus').addEventListener('click', handlePlus)
document.querySelector('#minus').addEventListener('click', handleMinus)
```

Click +. **The count, the price line, the fee and the total all move together**,
because they are all worked out inside `render()` from the same `quantity`.

**Change the state, then render. That is the pattern**, and it does not get more
complicated than that for the rest of the course.

The `if` is a **guard** — without it you can order zero tickets, or minus three.
Never trust a button to be clicked a sensible number of times.

Better still, say so on the page. Add these two lines at the end of `render()`:

```js
document.querySelector('#minus').disabled = quantity === MIN_TICKETS
document.querySelector('#plus').disabled = quantity === MAX_TICKETS
```

A comparison is a boolean, and `.disabled` takes a boolean, so they fit
together with nothing in between. The CSS for a disabled button is already in
your stylesheet. **Click down to 1 and watch the minus button fade out.**

---

<a id="step-8"></a>

## Step 8 — Listening to typing

```js
function handlePromo(event) {
  promoCode = event.target.value.trim().toUpperCase()
  render()
}

document.querySelector('#promo').addEventListener('input', handlePromo)
```

Same two lines as before: change the state, then render.

- **`input` fires on every keystroke**, which is what makes it feel live.
  `change` would wait until you clicked away.
- **`.value`**, because it is an input. `.textContent` on an input is empty, and
  it is a confusing twenty minutes.
- **`.trim().toUpperCase()`** — methods chain left to right, each one handing
  its answer to the next. Trim the spaces somebody pasted, then stop caring
  about capitals.

Now use it, inside `render()`:

```js
const promoIsValid = promoCode === 'FRIEND'
const promoDiscount = promoIsValid ? subtotal * 0.1 : 0
const total = subtotal + serviceFee - discount - promoDiscount
```

Type `friend` slowly and watch the total drop the moment the last letter lands.
Delete a letter and watch it come back.

---

<a id="step-9"></a>

## Step 9 — Roll a different show

Last one, and it is nearly free. Week 09's random picks go into their own
function — **facts only, no drawing**:

```js
function rollNewShow() {
  headliner = pickRandom(headliners)
  support = pickRandom(supports)
  venue = pickRandom(venues)
  ticketPrice = randomBetween(12, 45) + 0.5
  // ...and the rest
}

function handleRoll() {
  rollNewShow()   // new facts
  render()        // draw them
}

document.querySelector('#roll').addEventListener('click', handleRoll)

rollNewShow()
render()
```

Note those last two lines, right at the bottom of the file. Everything above
them only *describes* what should happen. **Nothing has actually run yet.**
Those two lines start the page.

Click the button instead of reloading. Same result, no reload — which is, more
or less, what a single-page app is.

---

<a id="step-10"></a>

## Step 10 — In-class exercise

1. **A reset button.** Add one to the markup, and make it put `quantity` back
   to 1 and empty the promo code. Remember the input's `.value` has to be
   cleared too — the state and the box both.
2. **A second promo code** worth a flat $10 off, and a different message.
3. **Guard the stock.** You cannot buy more tickets than are left. Use
   `ticketsLeft` in your `MAX_TICKETS` logic and check what happens on a nearly
   sold-out show.
4. **Harder — `mouseover`.** Make the poster show the venue's address while the
   pointer is over it, and go back when it leaves. Two listeners, one element.

---

## What you can do now

- say what the DOM is, and why editing it does not change your file
- find elements with `querySelector`, and know what `null` is telling you
- read and set `.textContent`, `.innerHTML`, `.value`, `.src`, `.disabled`
- attach a listener with `addEventListener`, **without brackets on the function
  name**
- use `event.target`, and know why `currentTarget` exists
- keep **state** in one place and have **one `render()`** that draws from it
- change state, call render — for a click or a keystroke, the same two lines

**Thursday:** `classList`. Showing and hiding, a dark mode, an accordion — all
by adding and removing class names and letting the stylesheet do the work.

**Homework:** [HW.md](../../HW.md).

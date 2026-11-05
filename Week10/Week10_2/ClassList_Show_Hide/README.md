# Week 10 — `classList`: Showing, Hiding & Toggling

*Changing how a page looks by changing class names, and letting the stylesheet do the work · ~100 min*

> **Following along at home?** Bring your file from Tuesday. If it is in a
> strange state, copy `StarterFiles/` — it is Tuesday's finished page plus the
> new markup for today: a theme button, a collapsible running order, and three
> FAQ questions. All of it is styled and none of it works yet.

On Tuesday you changed what the page **said**. Today you change how it
**looks** — and almost none of that happens in JavaScript.

The rule for the whole class:

> **JavaScript adds and removes class names. CSS decides what they mean.**

Keeping it that way is the difference between a stylesheet you can read in
November and one you are afraid of. By the end you will have a collapsible
section, a working dark mode and an accordion, and the JavaScript for all three
is about fifteen lines.

Stuck? Read the error first, then [TROUBLESHOOTING.md](../../../TROUBLESHOOTING.md).

---

## Steps

1. [`classList`](#step-1)
2. [Hiding something, properly](#step-2)
3. [The toggle](#step-3)
4. [A collapsible section, and a free chevron](#step-4)
5. [One class on the body changes everything](#step-5)
6. [Many buttons at once](#step-6)
7. [`data-` attributes](#step-7)
8. [Toggles that work for everybody](#step-8)
9. [In-class exercise](#step-9)

---

<a id="step-1"></a>

## Step 1 — `classList`

Every element has a `classList`, with four methods on it:

```js
const poster = document.querySelector('.poster')

poster.classList.add('featured')       // put it on
poster.classList.remove('featured')    // take it off
poster.classList.toggle('featured')    // on if off, off if on
poster.classList.contains('featured')  // true or false
```

Try all four in the console on the live page and watch the Elements panel.

**`add` and `remove` do not complain.** Adding a class that is already there
does nothing. Removing one that was never there does nothing. No errors, no
duplicates — which makes them safe to call whenever.

### Why not `className`

On Tuesday we wrote this, and it worked:

```js
promoMsgEl.className = promoIsValid ? 'promo-msg ok' : 'promo-msg'
```

`className` is the whole class attribute as one string, so setting it
**replaces every class on the element**. You have to remember and re-list every
other class on that element, forever. Miss one and the layout quietly falls
apart.

```js
promoMsgEl.classList.toggle('ok', promoIsValid)
```

Same effect, says what it means, and leaves everything else alone. **Go and
change that line in your file now.**

> **`toggle` takes a second argument**, and it is worth knowing early.
> `toggle('ok', true)` means *add it*; `toggle('ok', false)` means *remove it*.
> So "make this class match this boolean" is one line. With a boolean you have
> worked out, that is almost always what you want.

---

<a id="step-2"></a>

## Step 2 — Hiding something, properly

There are two ways to hide an element, and only one of them scales.

```js
element.style.display = 'none'           // the tempting one
element.classList.add('is-hidden')       // the one to use
```

With a class, your stylesheet holds one rule:

```css
.is-hidden {
  display: none;
}
```

**Four reasons the class wins**, and they are all the same reason:

- **To show it again you need to know what it was.** `display = 'block'`? It
  might have been `flex`, or `grid`, or `inline-block`. With a class you just
  remove the class, and the element goes back to whatever the stylesheet
  always said.
- **Inline styles beat your stylesheet.** Once JavaScript has written
  `style="display: block"`, your CSS cannot win without `!important`, and now
  you have two files arguing.
- **One place to change it.** Decide later that hidden should mean faded
  instead of gone, and you edit one CSS rule rather than hunting through your
  JavaScript.
- **You can read it.** `classList.add('is-hidden')` says what you meant.
  `style.display = 'none'` says how you did it.

Name classes for the **state**, not the look: `is-hidden`, `is-open`,
`is-active`, `has-error`. The `is-` prefix is a widespread convention and it
tells the next person this class is switched on and off by code, not something
to style around.

---

<a id="step-3"></a>

## Step 3 — The toggle

Showing and hiding the same thing with one button is the whole pattern, and it
is one line:

```js
const answer = document.querySelector('#faq-1')
answer.classList.toggle('is-hidden')
```

`toggle` also **hands back whether the class is now on**, which saves you
asking:

```js
const nowHidden = answer.classList.toggle('is-hidden')
console.log(nowHidden)   // true if it is now hidden
```

Wire up the first question by hand:

```js
const firstQuestion = document.querySelector('.faq-q')

function handleFirstQuestion() {
  const answer = document.querySelector('#faq-1')
  answer.classList.toggle('is-hidden')
}

firstQuestion.addEventListener('click', handleFirstQuestion)
```

Click it. Click it again. **One line of JavaScript, one line of CSS, and you
have an accordion** — the component people reach for a library to get.

Now notice the problem: there are three questions, and this works for one.

---

<a id="step-4"></a>

## Step 4 — A collapsible section, and a free chevron

The running order should fold away too — but its header has a little ▾ in it
that ought to flip over, and the chevron is not inside the thing being hidden.

**So put the class on the parent and let CSS reach down into it.** The markup
is already arranged for this:

```html
<section class="lineup-wrap is-open" id="lineup-section">
  <button class="section-head" id="lineup-toggle">
    <h2>Running order</h2>
    <span class="chevron">▾</span>
  </button>

  <div class="collapsible-body" id="lineup-body">
    ...
  </div>
</section>
```

```css
.collapsible-body    { display: none; }
.is-open > .collapsible-body { display: block; }

.chevron             { transition: transform 200ms ease; }
.is-open .chevron    { transform: rotate(180deg); }
```

And the JavaScript, in full:

```js
const lineupSection = document.querySelector('#lineup-section')

function handleLineupToggle() {
  lineupSection.classList.toggle('is-open')
}

document.querySelector('#lineup-toggle').addEventListener('click', handleLineupToggle)
```

**One class, two effects.** The panel appears and the chevron turns over,
because the CSS already knows what `is-open` means for everything inside.

### The chevron animates, and you did not ask it to

That `transition` is from Week 07, and it is doing real work here. The class
flips `transform` from nothing to `rotate(180deg)`, and because the property is
in a transition the browser animates between the two. **You never animate
anything in JavaScript. You change a class and let the transition happen.**

That is also why `display` cannot be animated — it has no in-between. If you
want a panel to fade or slide, transition `opacity` or `max-height` instead,
and use a class for that too.

---

<a id="step-5"></a>

## Step 5 — One class on the body changes everything

Dark mode. All of it:

```js
const themeButton = document.querySelector('#theme')

function handleTheme() {
  const isDark = document.body.classList.toggle('dark')
  themeButton.textContent = isDark ? 'Light mode' : 'Dark mode'
}

themeButton.addEventListener('click', handleTheme)
```

Four lines, and the entire page changes colour. Look at what makes that
possible, in your stylesheet:

```css
:root {
  --paper: #f7f4ed;
  --ink: #16151a;
  --muted: #6c6a75;
  --line: #d8d3c7;
}

body.dark {
  --paper: #1c1b21;
  --ink: #f2efe8;
  --muted: #8f8c9b;
  --line: #34323d;
  background: #09090c;
}
```

**Not one rule mentions a component.** No `.ticket`, no `.lineup`, no `.total`.
Every one of those was built on `var(--paper)` and `var(--ink)` back in Week
07, so redefining four variables re-themes forty rules at once.

That is the whole argument for CSS custom properties, and it took three
classes to arrive at: **variables in Week 07, a class on the body today.**

Note `document.body` — the body element is used so often it has its own
shortcut. `document.querySelector('body')` is the same thing.

---

<a id="step-6"></a>

## Step 6 — Many buttons at once

Three FAQ questions. You are not writing three handlers.

```js
const questions = document.querySelectorAll('.faq-q')
console.log(questions)        // a list of all three
console.log(questions.length) // 3
```

**`querySelectorAll` gives you a list of every match**, which you index and
loop over exactly like the arrays from Week 09:

```js
function handleQuestion(event) {
  const button = event.currentTarget
  const answer = document.querySelector(button.dataset.target)
  answer.classList.toggle('is-hidden')
}

for (let i = 0; i < questions.length; i++) {
  questions[i].addEventListener('click', handleQuestion)
}
```

**The same function is attached to all three buttons.** It has to work out
which one was clicked, every time it runs — and that is what `event` is for:

```js
const button = event.currentTarget
```

**`currentTarget`, not `target`.** Each of these buttons has a `<span>` chevron
inside it. Click exactly on the chevron and `event.target` is the span, which
has no `data-target` on it and breaks everything. `currentTarget` is always the
element the listener was attached to — the button. This is the bug from
Tuesday's step 5, arriving on schedule.

> **A note for later:** `querySelectorAll` gives you a *NodeList*, not quite an
> array. A `for` loop works on both, which is why we are using one. Arrays have
> extra methods a NodeList lacks, and you will meet the difference when you
> meet `forEach`.

---

<a id="step-7"></a>

## Step 7 — `data-` attributes

How does the function know which answer belongs to which button? The markup
tells it:

```html
<button class="faq-q" data-target="#faq-2">Can I get a refund?</button>
<p class="faq-a is-hidden" id="faq-2">Tickets are non-refundable, but...</p>
```

**Any attribute beginning with `data-` is yours.** HTML ignores it, CSS can see
it, and JavaScript reads it through `dataset`:

```js
button.dataset.target   // '#faq-2'
```

`data-target` becomes `dataset.target`; `data-max-guests` becomes
`dataset.maxGuests`. Dashes become camelCase, which catches everybody once.

This is how you connect one element to another **without hard-coding anything
in your JavaScript**. Add a fourth question to the HTML with its own
`data-target` and it works immediately — no new code at all. Delete one, same.
**The markup describes the relationship; the JavaScript just follows it.**

The finished handler, with the chevron:

```js
function handleQuestion(event) {
  const button = event.currentTarget
  const answer = document.querySelector(button.dataset.target)

  const nowHidden = answer.classList.toggle('is-hidden')

  // the chevron lives in the button, so the class goes on the button
  button.classList.toggle('is-open', !nowHidden)
}
```

There is `toggle`'s second argument from step 1, keeping the chevron and the
panel honest about each other.

---

<a id="step-8"></a>

## Step 8 — Toggles that work for everybody

Two things make a toggle usable by people who are not looking at it, and both
are nearly free.

**1. It has to be a `<button>`.** Not a `<div>` with a click listener. A real
button is reachable by Tab, fires on Enter and Space, and is announced as
something you can press. A div with a click listener is invisible to anyone
using a keyboard, and you would have to rebuild all three behaviours by hand to
catch up.

Every toggle in today's markup is already a `<button type="button">`. The
`type` matters: inside a form, a button with no type submits it.

**2. Say whether it is open.**

```js
function handleLineupToggle() {
  const isOpen = lineupSection.classList.toggle('is-open')
  lineupToggle.setAttribute('aria-expanded', isOpen)
}
```

`aria-expanded` is read aloud as "expanded" or "collapsed". Without it, a
screen reader announces a button that does something invisible. With it, the
control explains itself.

`setAttribute` sets any attribute by name; the value is converted to a string,
so a boolean is fine. Pair it with `aria-controls="lineup-body"` on the button,
pointing at the id of the thing it opens, and the two elements are formally
connected.

For the theme button, the equivalent is `aria-pressed`, because it is an
on/off switch rather than a disclosure.

**This is four extra lines across the whole page.** Toggles are exactly where
accessibility is cheapest, which is why it is worth being in the habit before
anything is at stake.

---

<a id="step-9"></a>

## Step 9 — In-class exercise

1. **Close the others.** Opening one FAQ question currently leaves the rest
   open. Make only one be open at a time — loop over all the answers, add
   `is-hidden` to every one, then remove it from the one just clicked.
2. **A details toggle on the order.** Hide the fee and discount lines behind a
   "show breakdown" button, with the total always visible.
3. **Make the panel fade instead of jumping.** `display` cannot be animated, so
   use a class that transitions `opacity` and `max-height`. Find out why
   `max-height` needs a number rather than `auto`.
4. **Harder — remember the theme.** Look up `localStorage.setItem` and
   `localStorage.getItem`, store whether dark mode is on, and put the class
   back when the page loads. Twenty minutes, and it is the first thing you will
   have built that survives a reload.

---

## What you can do now

- `classList.add`, `.remove`, `.toggle`, `.contains` — and why not `className`
- hide and show with a **class**, never `style.display`
- use `toggle`'s **return value** and its **second argument**
- put a class on a **parent** and let CSS style everything inside it
- re-theme a whole page with **one class on the body** and CSS variables
- give **many elements the same listener** with `querySelectorAll` and a loop
- connect elements with **`data-` attributes** and read them with `dataset`
- use **`currentTarget`** so a click on an icon inside a button still works
- make a toggle usable with **`<button>`** and **`aria-expanded`**

**Where this goes next:** everything on this page is invented by your own code.
Next we go and get real data from somewhere else — which needs **objects**, the
shape nearly all real data arrives in.

**Homework:** [HW.md](../../HW.md).

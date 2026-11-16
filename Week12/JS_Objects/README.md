# Self-guided lesson: JavaScript objects

**Tuesday 17 November, in place of class. Due before class on Thursday 19
November.**

There is no class on Tuesday. This is what you do instead, on your own, at
whatever hour suits you. Budget **about two hours**. It is longer than the
homework usually is, because it is replacing a class, and because objects are
the single most important thing left in this course.

Everything you have written so far has used loose variables. One variable, one
value. That has worked because our pages have been small. It stops working very
soon, and Thursday's class assumes you have done this.

**Read it in order and do the Try It boxes as you reach them.** Skipping to the
assignment at the bottom will not work. The assignment is a refactor of your own
show page, and it uses every section above it.

---

## Before you start

Open `StarterFiles/index.html` in VS Code. It is a near empty page with a
`<script>` tag, and it is where you will do the Try It exercises.

Open it in your browser, then open the **Console**:

- Mac: `Cmd + Option + J`
- Windows: `F12`, then click the **Console** tab

Every time you save in VS Code, refresh the browser to re-run the script.

You will also want **your own show page from Week 10 and Week 11** open in a
second tab. The lesson keeps pointing at it.

---

## What you will be able to do afterwards

- Store many related values in one variable, and explain why that is better
- Read and change values inside an object, with both kinds of syntax
- Build an array of objects, which is the shape nearly all real data arrives in
- Write a function that lives inside an object, and use `this`
- Loop over an object's keys
- Save an object to local storage and get it back, which does not work the way
  you would guess

---

## Section 1: ten variables, one show

Open your show page and find this part. You wrote it in Week 9 and have been
adding to it since:

```js
let headliner
let support
let venue
let doors
let ticketPrice
let capacity
let ticketsSold
let lineup
let tagline
let accent
```

Ten variables. Now say out loud what they describe.

They describe **one show**. Not ten things. One thing with ten facts about it.

Nothing is broken, and nothing is going to break tomorrow. But notice what you
cannot currently do:

- You cannot pass the show to a function. You would have to pass ten arguments,
  in the right order, every time.
- You cannot have **two** shows on the page. You would need twenty variables,
  named `headliner2`, `venue2`, and so on, and the day you add an eleventh fact
  you have to add it twice.
- You cannot save the show anywhere, or send it anywhere, because it is not a
  thing. It is ten things that happen to be near each other in your file.

An object fixes all three. That is the whole lesson.

> One sentence to keep: **an array is a list of things, an object is one thing
> with named parts.**

---

## Section 2: one object

An object literal is curly braces, with `key: value` pairs inside, separated by
commas.

```js
const show = {
  headliner: 'Bad Moon Rising',
  venue: 'Brooklyn Steel',
  ticketPrice: 28.5,
  capacity: 400,
  isSoldOut: false,
}

console.log(show)
```

Things to notice:

- The names on the left are **keys** (also called properties). They are not
  strings, and they do not get quotes. They are not variables either.
- The values on the right can be any type you have met: a string, a number, a
  boolean, an array, even another object.
- The comma after the last pair is optional and harmless. Leave it in. It makes
  adding the next line a one line change instead of a two line change.
- `console.log(show)` prints a thing you can click open in the console. Click
  the triangle. That is your object.

### Try it

In `StarterFiles/index.html`:

1. Make an object called `me` with four keys: your name, the borough or town
   you live in, the number of classes you are taking this semester, and whether
   you have eaten today (a boolean).
2. `console.log(me)` and open it up in the console.
3. Add a fifth key whose value is an **array** of three things you would eat
   right now.

---

## Section 3: reading values, two ways

### Dot notation

```js
console.log(show.headliner)   // Bad Moon Rising
console.log(show.capacity)    // 400
```

This is what you will use nine times out of ten. Read it as "the headliner of
show".

If you ask for a key that does not exist, you do not get an error. You get
`undefined`:

```js
console.log(show.doors)       // undefined
```

**Remember that one.** `undefined` appearing on your page almost always means a
misspelled key, not a broken object. `show.Headliner` with a capital H is a
different key from `show.headliner`, and JavaScript will not warn you.

### Bracket notation

```js
console.log(show['headliner'])   // Bad Moon Rising
```

Same answer, more typing. So why does it exist?

Because the thing in the brackets is **an expression**, which means it can be a
variable:

```js
const whichKey = 'venue'
console.log(show[whichKey])      // Brooklyn Steel
```

Read those two lines again. With dot notation you have to know the key when you
write the code. With bracket notation you can decide **while the code is
running**. That is not a style preference, it is a different capability, and
Section 8 depends on it.

Bracket notation is also the only option when a key has a space or a dash in
it, or starts with a number:

```js
const prices = {
  'student rate': 5,
  'door price': 35,
}

console.log(prices['student rate'])   // 5
console.log(prices.student rate)      // SyntaxError
```

You will meet keys like that when data comes from somewhere other than you.

### Try it

Using your `me` object:

1. Log your name with dot notation.
2. Log your borough with bracket notation.
3. Make a variable `key` set to `'name'`, then log `me[key]`. Change `key` to
   another key of your object, save, refresh. Same line of code, different
   answer.
4. Log a key that does not exist. Confirm you get `undefined` and not an error.

---

## Section 4: changing an object

### Update

```js
show.ticketPrice = 32
show['capacity'] = 500
```

### Add

Exactly the same syntax. If the key is not there, assigning to it creates it:

```js
show.doors = 'Fri 13 Nov, 8pm'
```

There is no separate "add" command. That is convenient and it is also why a
typo silently creates a new key instead of updating the one you meant.

### Delete

```js
delete show.isSoldOut
```

You will use this rarely. It is here so you recognise it.

### The thing that confuses everyone

```js
const show = {headliner: 'Bad Moon Rising'}

show.headliner = 'Velvet Static'   // fine
show = {headliner: 'Velvet Static'} // TypeError: Assignment to constant variable
```

`const` means **the variable cannot be pointed at a different object**. It does
not mean the object cannot change. The box is nailed to the floor; the contents
are not.

This is why almost every object you write will be `const`, including ones you
change constantly. If that feels wrong, it is because `const` is badly named.

### Try it

1. Change the number of classes in your `me` object.
2. Add a key that was not there before.
3. Push a fourth item onto your array of foods. (`me.foods.push('...')`. Yes,
   array methods work on an array that lives inside an object.)
4. Try to reassign the whole `me` object while it is `const`. Read the error.
   Then change nothing and move on. That error is a friend.

---

## Section 5: objects inside objects

A value can be another object. This is how real data is shaped.

```js
const show = {
  headliner: 'Bad Moon Rising',

  venue: {
    name: 'Brooklyn Steel',
    neighbourhood: 'East Williamsburg',
    capacity: 1800,
  },

  support: ['Low Ceiling', 'Tiny Mirrors'],
}

console.log(show.venue.name)        // Brooklyn Steel
console.log(show.venue.capacity)    // 1800
console.log(show.support[0])        // Low Ceiling
console.log(show.support.length)    // 2
```

Read `show.venue.name` left to right: take `show`, go into `venue`, take
`name`. You can chain as deep as the data goes.

**The error you will hit:**

```js
console.log(show.vnue.name)
// TypeError: Cannot read properties of undefined (reading 'name')
```

Decode it. `show.vnue` is `undefined` because of the typo. Then JavaScript
tries to read `.name` of `undefined`, and that is the error. **The thing that
is wrong is the word before the one the error names.** That rule will save you
twenty minutes at some point this semester.

### Try it

1. Rebuild `me` so that the place you live is an object with `city` and
   `borough` or `state` inside it.
2. Log the city with one chained expression.
3. Deliberately misspell the middle word and read the error carefully. Confirm
   it names the key *after* the broken one.

---

## Section 6: an array of objects

This is the one to slow down on. **Almost all data you ever receive is an array
of objects.** Every API, every spreadsheet export, every database query.

```js
const shows = [
  {headliner: 'Bad Moon Rising', venue: 'Brooklyn Steel', price: 28.5, soldOut: false},
  {headliner: 'Velvet Static', venue: 'Market Hotel', price: 15, soldOut: true},
  {headliner: 'Dial Tone', venue: 'Rough Trade', price: 22, soldOut: false},
]

console.log(shows.length)             // 3
console.log(shows[0])                 // the first show object
console.log(shows[0].headliner)       // Bad Moon Rising
console.log(shows[2].venue)           // Rough Trade
```

An array of objects is just an array. Everything you learned about arrays in
Week 9 still works, and now each item has named parts instead of being a bare
string.

### Looping over it

```js
for (let i = 0; i < shows.length; i++) {
  console.log(`${shows[i].headliner} at ${shows[i].venue}, $${shows[i].price}`)
}
```

### Picking one at random

You already wrote this function. It did not care what was in the array, so it
works unchanged:

```js
function pickRandom(list) {
  return list[Math.floor(Math.random() * list.length)]
}

const tonight = pickRandom(shows)
console.log(tonight.headliner)
```

Stop and notice what just happened. In your current show page, "roll a new
show" picks a random headliner, **and** a random venue, **and** a random price,
with no connection between them, so you can get a $45 ticket to a band playing
a basement. With an array of objects you pick **one real show**, and everything
about it is already consistent.

That is not a tidiness improvement. That is a bug class that stops existing.

### Try it

1. Make an array of four objects. Anything you like: four albums, four
   sandwiches, four classes. Each object needs at least three keys, one of
   which is a number and one of which is a boolean.
2. Log the third item's name with one expression.
3. Loop over the array and log one sentence per item using a template literal.
4. Use `pickRandom` to grab one and log two of its keys.

---

## Section 7: functions inside objects

A value can be a function. A function stored in an object is called a
**method**, and you call it with brackets like any other function:

```js
const show = {
  headliner: 'Bad Moon Rising',
  capacity: 400,
  ticketsSold: 388,

  ticketsLeft: function () {
    return 400 - 388
  },
}

console.log(show.ticketsLeft())   // 12
```

That works and it is terrible, because the numbers are typed in twice. Change
`capacity` to 500 and `ticketsLeft` quietly lies to you.

### `this`

Inside a method, `this` means **the object the method was called on**:

```js
const show = {
  headliner: 'Bad Moon Rising',
  capacity: 400,
  ticketsSold: 388,

  ticketsLeft() {
    return this.capacity - this.ticketsSold
  },

  isSoldOut() {
    return this.ticketsLeft() === 0
  },

  summary() {
    return `${this.headliner}: ${this.ticketsLeft()} tickets left`
  },
}

console.log(show.ticketsLeft())   // 12
console.log(show.isSoldOut())     // false
console.log(show.summary())       // Bad Moon Rising: 12 tickets left
```

Now change `ticketsSold` to `400` and all three answers update, because none of
them store anything. They work it out from the object they are sitting in.

Notice `ticketsLeft() { ... }` with no `function` keyword and no colon. That is
shorthand and it is what you will see in real code. `isSoldOut` calls
`this.ticketsLeft()`, so a method can use another method of the same object.

**The rule for `this`:** it refers to whatever is to the left of the dot at the
moment you call it. `show.ticketsLeft()` has `show` to the left of the dot, so
`this` is `show`. There is more to it than that, and you will meet the
complications later. For now, left of the dot.

### Try it

1. Add a method to one of your objects that works something out from two of its
   own keys using `this`. A total, a difference, a percentage.
2. Add a second method that returns a full sentence with a template literal,
   and that calls the first method.
3. Change one of the keys it depends on, refresh, and confirm both answers
   moved.

---

## Section 8: looping over an object's keys

Arrays have `.length` and numbered positions. Objects do not. To loop over an
object you first turn it into an array:

```js
const show = {
  headliner: 'Bad Moon Rising',
  venue: 'Brooklyn Steel',
  price: 28.5,
}

console.log(Object.keys(show))     // ['headliner', 'venue', 'price']
console.log(Object.values(show))   // ['Bad Moon Rising', 'Brooklyn Steel', 28.5]
console.log(Object.entries(show))  // [['headliner', 'Bad Moon Rising'], ...]
```

All three give you an array, so all three can be looped.

```js
const keys = Object.keys(show)

for (let i = 0; i < keys.length; i++) {
  const key = keys[i]
  console.log(`${key}: ${show[key]}`)
}
```

Look at that last line. `show[key]` is **bracket notation from Section 3**,
earning its keep. You could not write `show.key`, because that would look for a
property literally called `key`. This loop is the reason bracket notation
exists.

What it gets you: a details table that writes itself. Add a key to the object
and the table grows. You never touch the loop.

### Try it

1. Log `Object.keys`, `Object.values` and `Object.entries` of one of your
   objects and look at the shape of each.
2. Write the loop above for your own object so it prints every key and value.
3. Add a new key to the object, refresh, and confirm the loop picked it up
   without you changing the loop.

---

## Section 9: putting an object on the page

Everything so far has gone to the console. Here is the same thing in the DOM,
using exactly the `querySelector` and `textContent` you have been using since
Week 10.

```html
<h1 id="headliner"></h1>
<p id="where"></p>
<p id="price"></p>
```

```js
const show = {
  headliner: 'Bad Moon Rising',
  venue: 'Brooklyn Steel',
  price: 28.5,
}

function render() {
  document.querySelector('#headliner').textContent = show.headliner
  document.querySelector('#where').textContent = show.venue
  document.querySelector('#price').textContent = `$${show.price.toFixed(2)}`
}

render()
```

This is your `render()` function from Week 10, unchanged in shape. The only
difference is where the values come from: one object instead of loose
variables.

And now the payoff. Change the object, call `render()`, and the page catches
up:

```js
show.price = 35
render()
```

**One source of truth, one function that draws it.** That idea is the whole of
the rest of this course, and most of the next one.

### Try it

1. Add three elements with ids to the starter file's HTML.
2. Write a `render()` that fills them from one of your objects.
3. Below it, change one key and call `render()` again. Confirm the page updates.

---

## Section 10: objects and local storage

You learned local storage on Thursday. Try the obvious thing:

```js
const show = {headliner: 'Bad Moon Rising', price: 28.5}

localStorage.setItem('show', show)
console.log(localStorage.getItem('show'))   // "[object Object]"
```

`[object Object]`. Not an error, which is worse, because you will not notice
until later.

**Local storage can only hold strings.** Given an object, it calls the object's
default string conversion, which is the useless text above. Your data is gone.

The fix is two functions:

```js
localStorage.setItem('show', JSON.stringify(show))

const saved = JSON.parse(localStorage.getItem('show'))
console.log(saved.headliner)   // Bad Moon Rising
console.log(saved.price + 1)   // 29.5, so it is a real number again
```

- `JSON.stringify(thing)` turns an object or array into a string.
- `JSON.parse(string)` turns that string back into an object or array.

JSON is a text format for exactly this. It looks almost identical to an object
literal, with two differences: **every key is in double quotes**, and there are
**no trailing commas and no functions**.

```js
console.log(JSON.stringify(show))
// {"headliner":"Bad Moon Rising","price":28.5}
```

Methods do not survive the round trip. `JSON.stringify` silently drops them. An
object with a `ticketsLeft()` method comes back out of storage without it. That
is not a bug you can fix, it is what the format is.

**One trap.** If nothing has been saved yet, `getItem` returns `null`, and
`JSON.parse(null)` gives you `null` rather than an error. So always check:

```js
const raw = localStorage.getItem('show')

if (raw) {
  const saved = JSON.parse(raw)
  // use it
} else {
  // first visit, use a default
}
```

### Try it

1. Save one of your objects to local storage **without** `JSON.stringify`.
   Look at it in the console and in the Application tab of devtools. See
   `[object Object]` with your own eyes.
2. Do it properly with `stringify`, then read it back with `parse` and log one
   of its keys.
3. Save an **array of objects** the same way. It works identically.
4. Add a method to the object, save it, read it back, and try to call the
   method. Read the error. Now you know why.

---

# The assignment

Two parts. The second one is short and matters more than it looks.

## Part A: refactor your show page

Take your own show page, the one you have been building since Week 9, and
replace the loose variables with objects.

- [ ] **Make an array of at least five show objects.** Real-ish ones, written
      out by hand. Each needs at least six keys, including a number, a boolean,
      and an array (the lineup is the obvious array).
- [ ] **"Roll a different show" picks one object from that array**, instead of
      picking each fact separately. One line, using the `pickRandom` you
      already have.
- [ ] **`render()` reads from the chosen object**, not from ten loose
      variables. Every `textContent` line should have a dot in it.
- [ ] **At least one method on your show objects**, using `this`. Tickets left,
      or the total with the service fee, or a status sentence. Something
      `render()` actually calls.
- [ ] **One part of the page is built with a loop over `Object.keys`.** The
      running order, a details table, the FAQ, your choice. Adding a key to the
      object must make that part of the page grow with no change to the loop.

The quantity picker, the promo code and the theme toggle should all still work
when you are done. If one of them breaks, that is information: it probably
depended on a variable you deleted. Fix it rather than deleting the feature.

> **If your show page is in a state you cannot work with**, say so in your
> commit message and build Part A from scratch as a smaller page. A page that
> does the five bullets above on three objects is worth full marks. A broken
> page you were afraid to touch is worth none.

## Part B: make it survive a reload

- [ ] When the page loads, if there is a saved show in local storage, use it.
- [ ] When you roll a new show, save it.
- [ ] Add a button that clears the saved show and starts fresh.

Then answer this, in a comment at the top of your script:

- [ ] **When you reload and the saved show comes back, what happened to the
      method you wrote in Part A?** Test it before you answer. Say what you saw
      and why. Then say, in one sentence, what you would have to do about it.

That question is the point of Part B. Everything else is twenty minutes.

---

## Figure it out

Not shown above. One of them, not all three. Say which you tried in your commit
message.

- **`Object.freeze()`.** Freeze one of your show objects, then try to change a
  key. What happens, and does it stop you changing something *inside* a nested
  object? Try it and report.
- **Spread.** `const copy = {...show}`. Then change a key on `copy` and check
  whether `show` moved. Then do the same with a **nested** object inside it and
  check again. The second answer surprises people.
- **Sort an array of objects by one of their keys.** `shows.sort(...)` needs
  something from you that sorting an array of numbers did not. Work out what,
  and say what `sort` does to the original array.

---

## Checklist before you submit

- [ ] No `var` anywhere. `const` by default, `let` when the value is
      reassigned.
- [ ] The console is clean. No red, and no stray `console.log` left in from
      debugging, except any the assignment asked for.
- [ ] `undefined` does not appear anywhere on the rendered page.
- [ ] You have read your own Part B comment back and it is a real answer.

## Submitting

```bash
git add .
git commit -m "week 12 objects deep dive"
git push
```

Check on github.com that it actually went up.

**Bring questions to Thursday.** Objects are the thing everything else is built
on, and the half hour of Thursday I spend on what went wrong here is better
spent than almost anything else in the remaining weeks.

---

New to the repo? Start with [SETUP.md](../../SETUP.md). Stuck on something?
[TROUBLESHOOTING.md](../../TROUBLESHOOTING.md).

# JSON, and your first API calls

**Tuesday 24 November.** The last JavaScript class of the semester.

Last week you built objects by hand and typed the data in yourself. Today the
data arrives from somewhere else, over the internet, while the page is already
open. Everything you learned about objects is what makes that possible: what
comes back **is** an array of objects, and you already know how to read one.

Start from `StarterFiles/`. The HTML and the CSS are written. Open the console
and keep it open.

## The sentence this class is about

**The data is not there when the page loads. It arrives.**

Every strange thing about today comes from that one fact. Code that waits is
written differently from code that does not.

---

## 1. Look at it before you fetch it

Put this in your browser's address bar and press return:

```
https://rickandmortyapi.com/api/character/1
```

That is the whole API. No key, no sign-up, no password. A URL, and some text
comes back.

Look at what came back. Curly braces, keys in **double quotes**, commas. It
looks almost exactly like the object literals you wrote last week, and that is
not a coincidence: **JSON is a text format designed to look like a JavaScript
object.**

Two differences, both of which you saw in Week 12:

- every key is in double quotes, always
- there are no functions and no trailing commas

Now try the list:

```
https://rickandmortyapi.com/api/character
```

Scroll to the top of that one. The characters are **not** the top level. There
are two keys, `info` and `results`, and the characters are the array inside
`results`. Reading the shape before you write code against it is a habit worth
building today.

## 2. `fetch`, and the thing that makes it different

```js
const BASE = 'https://rickandmortyapi.com/api/character'

const response = fetch(`${BASE}/1`)
console.log(response)
```

Run that. You do not get a character. You get a **Promise**.

That is not a mistake, it is the whole idea. Going to another computer and
back takes time, maybe 50 milliseconds, maybe four seconds, maybe forever.
JavaScript will not stand still and wait, because the page would freeze. So
`fetch` hands you back a receipt that says "an answer is coming" and carries
on.

## 3. `async` and `await`

`await` is how you say: stop here, on this line, until the promise is
finished, then give me what it produced.

You can only use `await` inside a function marked `async`. That is the rule,
and the error when you forget is `await is only valid in async functions`,
which for once says exactly what is wrong.

```js
async function getCharacter(id) {
  const response = await fetch(`${BASE}/${id}`)
  const character = await response.json()

  console.log(character)
  return character
}

getCharacter(1)
```

**Two `await`s, and people ask about this every time.** The first one waits for
the response to arrive. The second one waits for the body of that response to
be read and parsed from JSON text into a real JavaScript object.

`response.json()` is `JSON.parse` from last week, with the reading of the text
built in. The text comes over the wire; the object is what you get back.

Open it up in the console. `id`, `name`, `status`, `species`, `gender`,
`image`, and an `origin` that is an object with its own `name` inside it.
Twelve keys. This is Week 12, delivered.

### Try it

- Change the `1` to `2`, then to `183`.
- `console.log(character.origin.name)`. Chained, exactly like last week.
- `console.log(character.episode.length)`. It is an array of URLs.

## 4. Put it on the page

Nothing new in this part. `querySelector` and `textContent` from Week 10, and
`classList` from Week 11.

```js
function renderCharacter(character) {
  document.querySelector('#avatar').src = character.image
  document.querySelector('#name').textContent = character.name
  document.querySelector('#meta').textContent = `${character.species} · ${character.gender}`
  document.querySelector('#origin').textContent = `From ${character.origin.name}`

  const statusEl = document.querySelector('#status')
  statusEl.textContent = character.status
  statusEl.className = `status ${character.status.toLowerCase()}`
}
```

That last pair of lines is the Week 11 idea doing real work. `status` comes
back as `Alive`, `Dead` or `unknown`. Lower-case it, put it on the element as
a class, and the stylesheet decides what green and red mean. **JavaScript adds
class names, CSS decides what they mean**, and the data is now choosing the
class.

## 5. The mistake everybody makes

Try this:

```js
getCharacter(9999)
```

There is no character 9999. Look at what happens: no red error, no `catch`
fires, and the page tries to render something that is not a character.

**`fetch` does not throw when the server says no.** It throws when it could
not reach the server **at all**. A reply saying "no such character" is still a
reply, and as far as `fetch` is concerned the trip was a complete success.

So you check yourself:

```js
async function getCharacter(id) {
  const response = await fetch(`${BASE}/${id}`)

  if (!response.ok) {
    throw new Error(`No character with id ${id} (status ${response.status})`)
  }

  return await response.json()
}
```

`response.ok` is `true` for any status in the 200s and `false` otherwise.
`response.status` is the number: 200 worked, 404 not found, 500 the server
broke.

**This is the single most common bug in code that talks to an API**, and now
you know about it before you write it.

## 6. The two ways it can go wrong

```js
async function showCharacter(id) {
  showLoading()

  try {
    const character = await getCharacter(id)
    renderCharacter(character)
  } catch (err) {
    console.error(err)
    showError(friendly(err))
  }
}
```

`try` and `catch` around an `await` catches both kinds of failure: the one you
threw yourself in step 5, and the one `fetch` throws when it could not get
there.

They deserve different messages, though, and telling them apart is worth
knowing:

```js
function friendly(err) {
  if (err.name === 'TypeError') {
    return 'Could not reach the server. Check your connection and try again.'
  }

  return err.message
}
```

A `TypeError` saying `Failed to fetch` means **no connection**. Anything else
here is an `Error` you threw yourself, so its message already says something
worth reading.

**Test it properly.** Turn your wifi off, reload, and read what your page
says. A page that says nothing when the internet is gone is a page that looks
broken.

## 7. Loading, and why it needs its own function

A request has exactly three states: it is happening, it failed, or it worked.
Exactly one of them should be on screen.

The starter file has an element for each, all with `is-hidden` on them. One
function per state, each one turning the others off, is the whole trick:

```js
function showLoading() {
  loadingEl.classList.remove('is-hidden')
  errorEl.classList.add('is-hidden')
  cardEl.classList.add('is-hidden')
}
```

If you set these flags inline at six different points in your code, you will
eventually show a spinner and an error at the same time. Writing it once means
you cannot.

---

# Part two: many of them

## 8. The list, and where the array actually is

```js
const response = await fetch(`${BASE}/?name=${name}`)
const data = await response.json()

console.log(data.info.count)   // how many matched in total
console.log(data.results)      // the array of characters
```

Remember step 1: the characters are in `results`. `data.results` is an array
of objects, which is exactly the shape you practised with last week, and
everything you know about arrays still applies.

**One detail that matters.** A name could contain a space or an `&`, and those
have meanings inside a URL. `encodeURIComponent` makes any text safe to paste
into one:

```js
const response = await fetch(`${BASE}/?name=${encodeURIComponent(name)}`)
```

## 9. A card for each

```js
let html = ''

for (let i = 0; i < characters.length; i++) {
  const character = characters[i]
  const status = character.status.toLowerCase()

  html += `
    <article class="card">
      <img class="avatar" src="${character.image}" alt="${character.name}" />
      <div class="card-body">
        <h2>${character.name}</h2>
        <p class="status ${status}">${character.status}</p>
        <p class="meta">${character.species}</p>
        <p class="meta">${character.origin.name}</p>
      </div>
    </article>`
}

resultsEl.innerHTML = html
```

This is the Week 9 running-order loop with better markup. Build one string,
set `innerHTML` **once** at the end. Setting it inside the loop makes the
browser re-parse everything on every pass.

## 10. Wire up the search box

```js
async function handleSearch(event) {
  event.preventDefault()

  const name = document.querySelector('#query').value.trim()

  if (name === '') {
    return
  }

  showLoading()

  try {
    const characters = await searchCharacters(name)
    renderResults(characters, name)
  } catch (err) {
    console.error(err)
    showError(friendly(err))
  }
}

document.querySelector('#search-form').addEventListener('submit', handleSearch)
```

`event.preventDefault()` on a form submit, same as the contact form in Week 08.
Without it the page reloads and everything you just fetched is gone.

**And one thing to decide for yourself.** Search for something nobody is
called, like `zzzz`. The API answers **404**. Is that an error, or is it an
empty result?

It is not an error. Nothing went wrong, the answer is just "none". So that one
status gets handled on its own, before the `response.ok` check:

```js
if (response.status === 404) {
  return []
}
```

Then the empty list becomes a sentence a person can read, rather than a red
box that makes it look like your page is broken.

---

## In-class exercise

**Fifteen minutes.** Make the cards do something.

- [ ] Clicking a card shows that character on their own, in the big card at
      the top. You have the id: it is on the character object, and
      `data-` attributes from Week 10 are how you get it onto the element and
      back off it.
- [ ] A "back to results" button.

If you finish, two harder ones:

- [ ] **Show which episodes they are in.** `character.episode` is an array of
      URLs, not names. Getting the names means more requests. How many?
- [ ] **Stop the double search.** Type a name, hit search twice quickly. Both
      requests are now in flight and they can come back in either order. What
      would you do about it?

Neither has a tidy answer at this level. The second one is a real problem that
real applications have.

## On your own

Point this at a different API. There are hundreds that need no key: a weather
API, a dictionary, the NASA photo of the day, a transit API, anything you are
actually curious about.

Keep the shape you built today:

1. a function that fetches and checks `response.ok`
2. a function that renders
3. `try`/`catch` around the call
4. a loading state and an error state that are never on screen together

**Final project reminder:** [HW.md](../HW.md) has not changed. Presentations
and documentation are next week, and that deadline is the one that matters
most right now.

---

New to the repo? Start with [SETUP.md](../../SETUP.md). Stuck on something? [TROUBLESHOOTING.md](../../TROUBLESHOOTING.md).

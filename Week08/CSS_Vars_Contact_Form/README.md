# Week 07 — Contact Form, CSS Variables & GitHub Pages

*A real form that sends you real email, themed with variables, live on the internet · ~100 min*

> **Following along at home?** Copy `StarterFiles/` into your own repo to
> start — the form markup is written, both stylesheets are empty. By the end of
> this lesson your page is on the internet at an address you can send to
> someone.

Three things today, and the third one is the one that matters most for your
midterm:

1. a styled **contact form**
2. **CSS custom properties** — variables — so the whole colour scheme changes
   from one line
3. **FormSpree** so the form actually sends, and **GitHub Pages** so the page
   is actually online

Stuck? Read the error first, then [TROUBLESHOOTING.md](../../../TROUBLESHOOTING.md).

---

## Steps

1. [Files, and reading the form](#step-1)
2. [Base styles](#step-2)
3. [The problem with hard-coded colours](#step-3)
4. [Custom properties, and `:root`](#step-4)
5. [A whole theme from one attribute](#step-5)
6. [Fonts are variables too](#step-6)
7. [Making it send: FormSpree](#step-7)
8. [Putting it on the internet: GitHub Pages](#step-8)
9. [In-class exercise](#step-9)

---

<a id="step-1"></a>

## Step 1 — Files, and reading the form

Copy `StarterFiles/` into your own homework repo:

```
contact/
├── index.html        ← the form, already written
└── css/
    ├── contact.css   ← empty
    └── vars.css      ← empty
```

Open `index.html` and read the form before styling it. Five things in there are
doing real work:

```html
<form action="#" method="POST">
```

- **`method="POST"`** sends the data in the body of the request. The
  alternative, `GET`, puts every field in the URL — fine for a search box,
  wrong for a message.
- **`action`** is *where it goes*. Right now it is `#`, a placeholder. Step 7
  replaces it with a real endpoint.

```html
<label class="label-lg" for="name">Name:</label>
<input name="name" type="text" class="input-lg" required />
```

- **`for="name"` on the label, matching the input** — clicking the label
  focuses the input, and a screen reader reads them as a pair. Free
  accessibility; do it every time.
- **`name="name"`** is the key the data arrives under. No `name`, no data —
  the field is simply not submitted. This is the single most common broken-form
  bug.
- **`required`** makes the browser refuse to submit an empty field, with no
  JavaScript at all.

And at the bottom:

```html
<input type="hidden" name="subject" value="New Submission From: {{name}}" />
```

A hidden field the user never sees, carrying extra information along with the
submission. FormSpree uses this one to set the email subject line.

Also note `<body id="blue">` — that `id` is doing nothing yet. By step 5 it
will control the entire colour scheme.

---

<a id="step-2"></a>

## Step 2 — Base styles

Straightforward layout, mostly flexbox. Type it, do not paste — there is
nothing new here and that makes it good practice.

**`css/contact.css`**

```css
body {
  font-family: sans-serif;
}

.container {
  width: 50%;
  margin: 0 auto;
  border-radius: 1rem;
  padding: 2rem;
  background-color: #e2e8f0;
  color: #475569;
}

.row {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
}

.label-lg {
  font-size: 2rem;
}

.input-lg {
  padding: 0.5rem;
  border-radius: 0.25rem;
  border: 1px solid #475569;
  margin-right: 2rem;
}

.message-area {
  display: flex;
  flex-direction: column;
  margin-top: 2rem;
  font-size: 2rem;
}

.message {
  height: 16rem;
  border-radius: 0.25rem;
  padding: 1rem;
}

.button-bar {
  display: flex;
  flex-direction: row-reverse;
  margin-top: 2rem;
}

.btn-dark {
  width: 100%;
  border-radius: 0.5rem;
  padding: 1rem;
  background-color: #1e293b;
  color: #f8fafc;
  border: 1px solid #475569;
}

.btn-dark:hover {
  background-color: #64748b;
}
```

`width: 50%` with `margin: 0 auto` is the oldest centring trick in CSS: give a
block a width, then let equal automatic margins split the leftover space.

`flex-direction: row-reverse` on the button bar pushes the button to the right
without any alignment properties — the row just runs the other way.

---

<a id="step-3"></a>

## Step 3 — The problem with hard-coded colours

Count how many times `#475569` appears in what you just wrote. Three. And
`#1e293b` and `#f8fafc` once each, with a hover shade that has to *relate* to
the button colour or it looks wrong.

Now imagine the client says "make it green."

You are hunting through a stylesheet with your eyes, changing some of them,
missing one, and finding it a week later. On a real site that is not three
values, it is sixty, spread over four files.

The colour is not really "`#475569`". It is **"the text colour"** — and
`#475569` is just what that currently happens to be. CSS lets you say that.

---

<a id="step-4"></a>

## Step 4 — Custom properties, and `:root`

A **custom property** is a variable you define yourself. Two rules:

- the name **must start with two dashes**: `--c-text`
- you read it with **`var()`**: `color: var(--c-text)`

They are declared inside a selector, and they are **inherited** — every element
inside that selector can see them. So the usual place to put the global set is
**`:root`**, which is the `<html>` element, the ancestor of everything.

**`css/vars.css`** — new file

```css
/* Imports on top, usually fonts and other utility stylesheets */

/* CSS Variables -- usually in global.css or variables.css */
:root {
  --c-bg: #e2e8f0;
  --c-text: #475569;
  --c-bg-btn: #1e293b;
  --c-bg-btn-hover: #64748b;
  --c-btn-text: #f8fafc;
}
```

Then pull it into the main stylesheet, **at the very top** — `@import` has to
come before any rules or the browser ignores it:

**`css/contact.css`**

```css
@import url('vars.css');
```

And swap every hard-coded colour for the name of what it *is*:

```css
.container {
  background-color: var(--c-bg);
  color: var(--c-text);
}

.input-lg {
  border: 1px solid var(--c-text);
}

.btn-dark {
  background-color: var(--c-bg-btn);
  color: var(--c-btn-text);
  border: 1px solid var(--c-text);
}

.btn-dark:hover {
  background-color: var(--c-bg-btn-hover);
}
```

The page looks identical. Nothing has changed except that the colours now have
names — and one place to change them.

Change `--c-bg` in `vars.css` and watch the whole page follow.

---

<a id="step-5"></a>

## Step 5 — A whole theme from one attribute

Here is the part that makes variables genuinely different from
find-and-replace.

Because custom properties are **inherited**, you can redefine them further down
the tree and everything inside picks up the new value. Define them again under
a different selector, and you have a theme.

**`css/vars.css`**

```css
/* Different colour themes by body id */

#green {
  --c-bg: #a7f3d0;
  --c-text: #065f46;
  --c-bg-btn: #059669;
  --c-bg-btn-hover: #065f46;
  --c-btn-text: #ecfdf5;
}

#blue {
  --c-bg: #1e3a8a;
  --c-text: #dbeafe;
  --c-bg-btn: #3b82f6;
  --c-bg-btn-hover: #1d4ed8;
  --c-btn-text: #eff6ff;
}
```

Now, in `index.html`, change one attribute:

```html
<body id="blue">     →     <body id="green">
```

**The entire page re-themes.** Not one rule in `contact.css` changed. The rules
ask for `var(--c-bg)`, and what that means now depends on which id is on the
body.

Delete the id entirely and the page falls back to the `:root` values — which is
why `:root` should always hold a complete, sensible default set.

This is how dark mode works on real sites, and now you know the whole trick.

---

<a id="step-6"></a>

## Step 6 — Fonts are variables too

Custom properties are not only for colour. Any value at all.

**`css/vars.css`** — the `@import` for the fonts goes at the very top of the
file, above `:root`:

```css
@import url('https://fonts.googleapis.com/css2?family=Press+Start+2P&family=Share+Tech+Mono&display=swap');

:root {
  /* ...the colours... */

  --ff-heading: 'Press Start 2P', system-ui;
  --ff-body: 'Share Tech Mono', monospace;
}
```

**`css/contact.css`**

```css
body {
  font-family: var(--ff-body);
}

h1 {
  font-family: var(--ff-heading);
}
```

Two font families, named for their job rather than their name. Swap the
`@import` and the two variables and the whole site's typography changes.

Note the fallbacks after each — `system-ui`, `monospace`. If Google Fonts is
slow or blocked, the page still has a sensible font. Always leave a fallback.

---

<a id="step-7"></a>

## Step 7 — Making it send: FormSpree

A form with `action="#"` does nothing. A real form needs somewhere to send the
data — normally a server you wrote, which we do not have.

**FormSpree** is a free service that gives you an endpoint, takes the
submission and emails it to you. There are several of these; this one is quick
and free.

1. Sign up at [formspree.io](https://formspree.io/) and verify your email.
2. Create a new form. Give it a name you will recognise in six months.
3. Copy the endpoint it gives you — it looks like
   `https://formspree.io/f/xxxxxxxx`.
4. Paste it into your form's `action`:

```html
<form action="https://formspree.io/f/xxxxxxxx" method="POST">
```

5. **Keep that tab open** — the first submission has to be confirmed there.

Now fill in your own form and submit it. You should get an email. If you do
not, check in this order:

- does every input have a **`name`**? No name, no data
- is the `action` the full URL including `https://`?
- is `method="POST"` still there?
- is the submission sitting in the FormSpree dashboard waiting for you to
  confirm it?

**The free tier has a monthly limit**, so do not sit there submitting fifty
test messages.

---

<a id="step-8"></a>

## Step 8 — Putting it on the internet: GitHub Pages

Everything you have built this semester has lived at `file:///Users/you/...`,
which nobody else can open. GitHub will host a static site for you, free, at a
real URL.

**1. A repository for the site.** Either a brand-new repo with this project at
its top level, or a folder in a repo you already have. The important thing is
that **`index.html` is where GitHub expects it** — at the root of whatever you
publish.

**2. Push it.**

```bash
git add .
git commit -m "add contact form"
git push
```

**3. Turn Pages on.** In the repo on github.com: **Settings → Pages**. Under
*Build and deployment*, set **Source** to *Deploy from a branch*, pick your
branch (`main`), folder `/ (root)`, and Save.

**4. Wait.** First deploy takes a couple of minutes. Refresh the Settings →
Pages panel and the URL appears at the top:

```
https://your-username.github.io/your-repo-name/
```

**5. Open it on your phone.** It is on the internet. Send it to someone.

### When it does not work

- **404.** Nine times out of ten there is no `index.html` at the root of what
  you published, or the filename is `Index.html` — GitHub's servers are
  case-sensitive even though your Mac is not.
- **The page loads but has no styles.** A path problem. `href="/css/style.css"`
  with a leading slash means *the root of the domain*, which on GitHub Pages is
  not your project folder. Use relative paths: `./css/style.css`.
- **Images missing.** Same cause, plus case — `Photo.JPG` and `photo.jpg` are
  different files to the server.
- **You pushed and nothing changed.** Give it a minute, then hard-refresh
  (Cmd+Shift+R). Pages caches aggressively.

Put the live link in your repo's `README.md`. **A site nobody can visit is not
finished**, and your midterm is graded with that in mind.

---

<a id="step-9"></a>

## Step 9 — In-class exercise

**1. A third theme.** Add your own — `#sunset`, `#mono`, whatever — as a new
block of variables in `vars.css`, and switch the body id to try it. Five
variables, no other changes.

**2. Put the form on your midterm.** A contact page, or a section on the home
page. It is your own site, your own colours — which means redefining the
variables, not rewriting the rules.

**3. Pull your midterm's colours into variables.** Open your midterm's
stylesheet and find the colours you have repeated. Name them at `:root` and
swap them for `var()`. This is the single cheapest thing you can do to make a
stylesheet maintainable, and you will feel the difference the first time you
change your mind about a colour.

**Bonus:** get your midterm itself onto GitHub Pages before you present. You
have just done it once; the second time takes five minutes.

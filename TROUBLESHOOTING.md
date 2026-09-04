# Troubleshooting

Check here **before** you email me. Almost every problem in this class is one of
the things below.

## The three questions, in order

1. **Read the error out loud.** Actually read it. The answer is in there
   surprisingly often.
2. **Are you in the right folder?** Run `ls` (Mac / Git Bash) or `dir` (Windows
   CMD). Do you see the files you expect? If not, every command will fail in
   confusing ways.
3. **Did you save the file?** `Cmd/Ctrl + S`. The browser cannot see unsaved
   changes.

---

## My page looks wrong / nothing is happening

### I changed the HTML and the browser looks the same

- Did you **save**?
- Did you **refresh** the browser? Try a hard refresh: `Cmd + Shift + R` (Mac) or
  `Ctrl + Shift + R` (Windows).
- Are you looking at the file you're editing? Check the browser's address bar
  against the file path in VS Code.
- Easiest fix for all of the above: use the **Live Server** extension. Right-click
  your HTML file → "Open with Live Server". It reloads automatically on save.

### My CSS isn't applying

In order, check:

1. **Is the stylesheet linked?** In `<head>`:
   `<link rel="stylesheet" href="style.css">`
2. **Is the path right?** If your CSS is in a `css/` folder it's
   `href="css/style.css"`. Open the browser dev tools → Network tab → a 404 on
   your css file means the path is wrong.
3. **Is the selector right?** `.card` targets `class="card"`. `#card` targets
   `id="card"`. `card` targets a `<card>` element, which doesn't exist.
4. **Is something more specific overriding it?** Right-click the element →
   Inspect. Anything crossed out in the Styles panel is being overridden by a
   rule with higher specificity. An id beats a class beats a tag.
5. **Typo in the property or a missing semicolon?** One bad line can break the
   rules after it.

### My image isn't showing

- Check the path. `img/photo.jpg` means there's a folder called `img` **next to
  your HTML file**.
- Check the file extension, exactly. `.jpg` and `.jpeg` and `.JPG` are different
  as far as a web server is concerned, even though your Mac may not care.
- **Capitalization matters.** `Photo.jpg` and `photo.jpg` are different files
  once your site is online, even if it works locally on a Mac.
- No spaces in filenames. Ever. `my photo.jpg` will cause problems — use
  `my-photo.jpg`.

### My layout is broken and I can't tell why

Add this temporarily to see every box on the page:

```css
* { outline: 1px solid red; }
```

Delete it when you're done.

---

## JavaScript

### Nothing happens and I don't know why

**Open the console.** `Cmd + Option + J` (Mac) or `F12` → Console (Windows). The
error is almost always sitting there waiting for you. A page that "does nothing"
is usually a page with one red error on line 12.

### `Uncaught ReferenceError: x is not defined`

You used a variable or function name that doesn't exist. Usually a typo, or a
capitalization mismatch — `myButton` and `mybutton` are different.

### `Cannot read properties of null (reading 'addEventListener')`

`document.querySelector(...)` didn't find anything, so it returned `null`. Either:

- your selector doesn't match anything (check the spelling and the `.` or `#`), or
- your `<script>` tag runs **before** the HTML exists. Put your script tag at the
  very bottom, just before `</body>`, or add `defer` to it.

### My function runs immediately instead of on click

```js
button.addEventListener('click', doThing())   // WRONG -- calls it right now
button.addEventListener('click', doThing)     // right -- hands over the function
```

---

## Git and GitHub

### GitHub won't take my password

It hasn't since 2021. You need a **personal access token** — see
[Week01/Hello-World/README.md](Week01/Hello-World/README.md) for the steps.

### `fatal: not a git repository`

You're not inside your repo folder. `cd` into it first, and `ls` to confirm.

### `Please tell me who you are`

First time using git on this machine:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@nyu.edu"
```

### `git push` says everything is up to date, but GitHub looks empty

You committed nothing. `git add .` then `git commit -m "message"` then
`git push`. Run `git status` between each step and actually read what it says.

### I edited files in the CLASS repo and now `git pull` fails

Don't work inside my repo — copy things out into your own. To fix it, throw away
your local changes:

```bash
cd WebDevF26
git checkout .
git pull
```

If that doesn't work, delete your copy of my repo and clone it again. You haven't
lost any of your own work — that lives in your repo.

### I don't want to commit some file

Nothing you're doing in this class needs to be secret, so this is rare. If you
need it, add the filename to a `.gitignore` file at the top of your repo.

---

## Still stuck?

Bring me:

1. The **exact** error text — screenshot the whole thing, don't paraphrase it
2. What you ran, and what folder you were in
3. What you'd already tried

That isn't bureaucracy. It's the difference between a two-minute fix and a
twenty-minute one, and it's exactly what a senior developer will ask you for at
your first job.

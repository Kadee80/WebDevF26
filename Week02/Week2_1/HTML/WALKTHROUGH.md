# Week 02 — Pushing, Cloning, and Hello HTML

*Get your homework pushed, clone the class repo, then build your first web page · ~2 hours (≈40 min setup, ≈70 min HTML)*

> **Following along at home?** Work through the steps in order. Each step shows what changed, and the full file underneath it. If you get lost, the finished code is in `end-of-class/`.

**First half:** finish setting up GitHub. You'll create a personal access token, push the homework you wrote last class, and clone the class repo so you can get my notes and examples all semester.

**Second half:** you build your first web page from an empty file.

Have your recipe from the homework open — you'll be marking up **your** recipe, not mine, so your page won't look identical to the screen at the front. That's expected.

Type every line rather than copying and pasting. It feels slower and it isn't.

Stuck? Check [TROUBLESHOOTING.md](../../../TROUBLESHOOTING.md).

---

## Steps

1. [Commit what you made last class](#step-1)
2. [If Git asks who you are](#step-2)
3. [Make a personal access token](#step-3)
4. [Push, and paste the token](#step-4)
5. [Clone the class repo](#step-5)
6. [Make the file, and see it in a browser](#step-6)
7. [The skeleton every page has](#step-7)
8. [The title tag](#step-8)
9. [Headings, and hierarchy](#step-9)
10. [Paragraphs](#step-10)
11. [Emphasis inside a paragraph](#step-11)
12. [Lists](#step-12)
13. [Images, and your first attribute](#step-13)
14. [Links — the thing that makes it a *web*](#step-14)
15. [Inspect it, then push it](#step-15)

---

<a id="step-1"></a>

## Step 1 — Commit what you made last class

You've each got a repo with a `Week01` folder and a `recipe.md` inside it. Those files are on your laptop — they are **not** on GitHub yet. Let's fix that.

Open a terminal and `cd` into your repo. `ls` should show your `Week01` folder. Then:

- `git status` shows what Git has noticed but isn't tracking yet
- `git add .` stages everything new or changed
- `git commit -m "..."` saves a snapshot, with a message

Read what each command prints. Git tells you what it did.

```bash
cd ~/Desktop/WebDevClassFiles/your-repo-name
ls
git status
git add .
git commit -m "week 1 homework"
```

---

<a id="step-2"></a>

## Step 2 — If Git asks who you are

The first time you commit on a new machine, Git wants a name and an email to stamp on your commits. You only ever do this once.

Use the same email as your GitHub account. Then run your `git commit` again.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@nyu.edu"
```

---

<a id="step-3"></a>

## Step 3 — Make a personal access token

Now the part that trips everybody up. When you push, GitHub asks for a username and password — **and your GitHub password will not work.** GitHub stopped accepting account passwords for this in 2021.

What you need instead is a **personal access token**. It's just a password with an expiry date, generated for one purpose.

In your browser:

*(All of this is written up in [SETUP.md](../../../SETUP.md#4-a-personal-access-token) if you need it again later.)*

1. GitHub → click your avatar (top right) → **Settings**
2. Scroll all the way to the bottom of the left sidebar → **Developer settings**
3. **Personal access tokens** → **Tokens (classic)**
4. **Generate new token** → **Generate new token (classic)**
5. **Note:** `class laptop`
6. **Expiration:** pick a date after this semester ends
7. Check the **`repo`** box — the top-level one. Ticking it selects everything underneath, which is what you want
8. Scroll down, **Generate token**

**Copy it right now and paste it somewhere you can find again.** GitHub shows it to you exactly once. If you lose it, you don't recover it — you generate a new one.

---

<a id="step-4"></a>

## Step 4 — Push, and paste the token

Now push. When it asks for your **username**, that's your GitHub username. When it asks for your **password**, paste the **token**.

The terminal will not show anything as you paste — no dots, no stars, nothing. That's normal and deliberate. Paste and press Enter.

Then run the second command so your computer remembers the token and stops asking every single time.

**Now refresh your repo page on github.com.** Your `Week01` folder and your recipe should be there. If you can see it in the browser, I can see it — that's what handing in an assignment means in this class.

```bash
git push

# then, so you are not asked every time:
git config --global credential.helper osxkeychain   # Mac
git config --global credential.helper store         # Windows / Git Bash
```

---

<a id="step-5"></a>

## Step 5 — Clone the class repo

One more piece of setup, then we write HTML.

There are **two** repos in this class and they do different jobs:

- **Your repo** — your homework. You push to it. I grade from it.
- **My repo** — class notes and every in-class example. You clone it and pull it. You never push to it.

Clone mine next to yours, not inside it. Then any time I add new material, `cd` into the folder and `git pull` to get it.

**Don't edit files inside my repo.** If you do, `git pull` starts failing with merge conflicts. Copy anything you want to play with into your own folder first.

*(Also in [SETUP.md](../../../SETUP.md#5-two-repositories).)*

```bash
cd ~/Desktop/WebDevClassFiles
git clone https://github.com/Kadee80/WebDevF26.git
cd WebDevF26
git pull
```

---

<a id="step-6"></a>

## Step 6 — Make the file, and see it in a browser

In **your** repo, make a folder called `Week02` and create a file inside it called **`index.html`**.

The name matters. `index.html` is the file a web server hands out when someone visits a folder — it's the front door. And the `.html` extension is how VS Code knows to colour your code and help you type it.

Type one word in it, save, then right-click the file in VS Code and choose **Open with Live Server**.

**`index.html`**  — new file

```html
hello
```

---

<a id="step-7"></a>

## Step 7 — The skeleton every page has

Every HTML page has the same bones. Type them out:

- **`<!DOCTYPE html>`** — tells the browser "this is modern HTML." It goes first, always, and it's the one line that isn't a normal tag.
- **`<html>`** — wraps the entire page.
- **`<head>`** — information *about* the page. Nothing in here is visible on the page itself.
- **`<body>`** — everything people actually see.

Notice tags come in **pairs**: `<body>` opens, `</body>` closes. The slash means closing. Think of them like brackets — whenever you open one, you close it.

**`index.html`**

What changed:

```diff
@@ -1,2 +1,10 @@
-hello
+<!DOCTYPE html>
+<html>
+  <head>
+    <meta charset="UTF-8" />
+    <title>My Recipe</title>
+  </head>
+  <body>
+  </body>
+</html>
 
```

<details>
<summary>Full file after this step</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>My Recipe</title>
  </head>
  <body>
  </body>
</html>
```

</details>

---

<a id="step-8"></a>

## Step 8 — The title tag

The first thing we put in `<head>`. Save and look at the **browser tab** — not the page.

The title does two jobs: it names the tab, and it's the blue clickable line that shows up in Google results. It's the most valuable line of text on a web page, and it's invisible on the page itself.

**`index.html`**

What changed:

```diff
@@ -3,5 +3,5 @@
   <head>
     <meta charset="UTF-8" />
-    <title>My Recipe</title>
+    <title>Buttermilk Pancakes</title>
   </head>
   <body>
```

<details>
<summary>Full file after this step</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Buttermilk Pancakes</title>
  </head>
  <body>
  </body>
</html>
```

</details>

---

<a id="step-9"></a>

## Step 9 — Headings, and hierarchy

Six sizes, `<h1>` through `<h6>`. But they are **not** about size — they're about **importance**. Think of a research paper outline: the title, then the sections, then the subsections.

**One `<h1>` per page.** It's the title of this page's content. Everything else nests under it in order — don't skip from `h1` to `h4` just because you like the size. We fix sizes with CSS next week; the tag is about meaning.

This matters for real reasons: screen readers use headings to navigate a page, and search engines use them to work out what your page is about.

**`index.html`**

What changed:

```diff
@@ -3,7 +3,10 @@
   <head>
     <meta charset="UTF-8" />
-    <title>Buttermilk Pancakes</title>
+    <title>My Recipe</title>
   </head>
   <body>
+    <h1>Buttermilk Pancakes</h1>
+    <h2>Ingredients</h2>
+    <h2>Instructions</h2>
   </body>
 </html>
```

<details>
<summary>Full file after this step</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>My Recipe</title>
  </head>
  <body>
    <h1>Buttermilk Pancakes</h1>
    <h2>Ingredients</h2>
    <h2>Instructions</h2>
  </body>
</html>
```

</details>

---

<a id="step-10"></a>

## Step 10 — Paragraphs

Most of the words on most web pages live in `<p>` tags.

Try typing a sentence straight into the `<body>` without a tag around it. It shows up — so why bother? Because the browser has no idea what it is. It isn't a paragraph, it's loose text, and you can't style it, script it, or let a screen reader announce it properly.

**Mark up all your text.** Loose text in the body is a bug.

**`index.html`**

What changed:

```diff
@@ -7,4 +7,8 @@
   <body>
     <h1>Buttermilk Pancakes</h1>
+    <p>
+      Light, fluffy pancakes that take about twenty minutes start to
+      finish. Makes roughly eight.
+    </p>
     <h2>Ingredients</h2>
     <h2>Instructions</h2>
```

<details>
<summary>Full file after this step</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>My Recipe</title>
  </head>
  <body>
    <h1>Buttermilk Pancakes</h1>
    <p>
      Light, fluffy pancakes that take about twenty minutes start to
      finish. Makes roughly eight.
    </p>
    <h2>Ingredients</h2>
    <h2>Instructions</h2>
  </body>
</html>
```

</details>

---

<a id="step-11"></a>

## Step 11 — Emphasis inside a paragraph

Two tags that go **inside** a paragraph, wrapped around a few words:

- **`<em>`** — emphasis, renders as *italic*
- **`<strong>`** — strong importance, renders as **bold**

These are your first **inline** tags. Everything before this — headings, paragraphs — is a **block**: it stacks vertically and takes the full width. Inline tags sit inside a line of text without breaking it.

That block-versus-inline distinction runs through the whole rest of this course.

**`index.html`**

What changed:

```diff
@@ -8,6 +8,7 @@
     <h1>Buttermilk Pancakes</h1>
     <p>
-      Light, fluffy pancakes that take about twenty minutes start to
-      finish. Makes roughly eight.
+      Light, fluffy pancakes that take about <strong>twenty minutes</strong>
+      start to finish. Makes roughly eight. The trick is <em>not</em>
+      overmixing the batter.
     </p>
     <h2>Ingredients</h2>
```

<details>
<summary>Full file after this step</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>My Recipe</title>
  </head>
  <body>
    <h1>Buttermilk Pancakes</h1>
    <p>
      Light, fluffy pancakes that take about <strong>twenty minutes</strong>
      start to finish. Makes roughly eight. The trick is <em>not</em>
      overmixing the batter.
    </p>
    <h2>Ingredients</h2>
    <h2>Instructions</h2>
  </body>
</html>
```

</details>

---

<a id="step-12"></a>

## Step 12 — Lists

Two kinds, and choosing between them is a content decision, not a style one:

- **`<ul>`** — *unordered*. Bullets. The order doesn't matter.
- **`<ol>`** — *ordered*. Numbers. The order **does** matter.

Both hold `<li>` — list item — and each `<li>` is one bullet or one number.

So: ingredients are a `<ul>` (you can buy them in any order), and instructions are an `<ol>` (you very much cannot melt the butter after you flip the pancake).

Notice the nesting and the indentation — `<li>` sits inside `<ul>`, so it's indented one step further.

**`index.html`**

What changed:

```diff
@@ -12,6 +12,25 @@
       overmixing the batter.
     </p>
+
     <h2>Ingredients</h2>
+    <ul>
+      <li>3/4 cup melted butter</li>
+      <li>2 tbsp whole milk</li>
+      <li>1 egg</li>
+      <li>3/4 cup all purpose flour</li>
+      <li>2 tsp baking powder</li>
+      <li>2 tbsp sugar</li>
+      <li>1/2 tsp salt</li>
+    </ul>
+
     <h2>Instructions</h2>
+    <ol>
+      <li>Melt the butter and let it cool slightly.</li>
+      <li>Beat together the milk, butter and egg.</li>
+      <li>Add the flour, baking powder, sugar and salt.</li>
+      <li>Mix until just combined. Lumps are fine.</li>
+      <li>Drop by the tablespoon into a hot buttered pan.</li>
+      <li>Flip when the top is full of little holes.</li>
+    </ol>
   </body>
 </html>
```

<details>
<summary>Full file after this step</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>My Recipe</title>
  </head>
  <body>
    <h1>Buttermilk Pancakes</h1>
    <p>
      Light, fluffy pancakes that take about <strong>twenty minutes</strong>
      start to finish. Makes roughly eight. The trick is <em>not</em>
      overmixing the batter.
    </p>

    <h2>Ingredients</h2>
    <ul>
      <li>3/4 cup melted butter</li>
      <li>2 tbsp whole milk</li>
      <li>1 egg</li>
      <li>3/4 cup all purpose flour</li>
      <li>2 tsp baking powder</li>
      <li>2 tbsp sugar</li>
      <li>1/2 tsp salt</li>
    </ul>

    <h2>Instructions</h2>
    <ol>
      <li>Melt the butter and let it cool slightly.</li>
      <li>Beat together the milk, butter and egg.</li>
      <li>Add the flour, baking powder, sugar and salt.</li>
      <li>Mix until just combined. Lumps are fine.</li>
      <li>Drop by the tablespoon into a hot buttered pan.</li>
      <li>Flip when the top is full of little holes.</li>
    </ol>
  </body>
</html>
```

</details>

---

<a id="step-13"></a>

## Step 13 — Images, and your first attribute

```html
<img src="img/pancakes.jpg" alt="A stack of buttermilk pancakes" />
```

Two new ideas here.

**Attributes.** `src` and `alt` are attributes — extra information inside the opening tag, written as `name="value"`. `src` says *where the image file is*. You'll see attributes on almost every tag from now on.

**Self-closing tags.** There's no `</img>`, because an image has no content to wrap — the tag *is* the whole thing. It closes itself with `/>`.

**`alt` is not optional.** It's what a screen reader announces, and what displays if the image fails to load. Describe the image; don't write "image of".

**`index.html`**

What changed:

```diff
@@ -7,4 +7,7 @@
   <body>
     <h1>Buttermilk Pancakes</h1>
+
+    <img src="img/pancakes.jpg" alt="A tall stack of buttermilk pancakes" />
+
     <p>
       Light, fluffy pancakes that take about <strong>twenty minutes</strong>
```

<details>
<summary>Full file after this step</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>My Recipe</title>
  </head>
  <body>
    <h1>Buttermilk Pancakes</h1>

    <img src="img/pancakes.jpg" alt="A tall stack of buttermilk pancakes" />

    <p>
      Light, fluffy pancakes that take about <strong>twenty minutes</strong>
      start to finish. Makes roughly eight. The trick is <em>not</em>
      overmixing the batter.
    </p>

    <h2>Ingredients</h2>
    <ul>
      <li>3/4 cup melted butter</li>
      <li>2 tbsp whole milk</li>
      <li>1 egg</li>
      <li>3/4 cup all purpose flour</li>
      <li>2 tsp baking powder</li>
      <li>2 tbsp sugar</li>
      <li>1/2 tsp salt</li>
    </ul>

    <h2>Instructions</h2>
    <ol>
      <li>Melt the butter and let it cool slightly.</li>
      <li>Beat together the milk, butter and egg.</li>
      <li>Add the flour, baking powder, sugar and salt.</li>
      <li>Mix until just combined. Lumps are fine.</li>
      <li>Drop by the tablespoon into a hot buttered pan.</li>
      <li>Flip when the top is full of little holes.</li>
    </ol>
  </body>
</html>
```

</details>

---

<a id="step-14"></a>

## Step 14 — Links — the thing that makes it a *web*

```html
<a href="https://www.nyu.edu">NYU</a>
```

`<a>` is the **anchor** tag, and `href` is the attribute holding the destination. The text between the tags is what the user sees and clicks.

This is the *hypertext* in HyperText Markup Language — text with links in it. It is the entire reason the web is a web and not a pile of documents.

Write link text that describes where it goes. "Click here" tells someone tabbing through your links with a screen reader nothing at all.

**`index.html`**

What changed:

```diff
@@ -36,4 +36,9 @@
       <li>Flip when the top is full of little holes.</li>
     </ol>
+
+    <p>
+      Adapted from a recipe I have been making since college. More at
+      <a href="https://www.nyu.edu">NYU</a>.
+    </p>
   </body>
 </html>
```

<details>
<summary>Full file after this step</summary>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>My Recipe</title>
  </head>
  <body>
    <h1>Buttermilk Pancakes</h1>

    <img src="img/pancakes.jpg" alt="A tall stack of buttermilk pancakes" />

    <p>
      Light, fluffy pancakes that take about <strong>twenty minutes</strong>
      start to finish. Makes roughly eight. The trick is <em>not</em>
      overmixing the batter.
    </p>

    <h2>Ingredients</h2>
    <ul>
      <li>3/4 cup melted butter</li>
      <li>2 tbsp whole milk</li>
      <li>1 egg</li>
      <li>3/4 cup all purpose flour</li>
      <li>2 tsp baking powder</li>
      <li>2 tbsp sugar</li>
      <li>1/2 tsp salt</li>
    </ul>

    <h2>Instructions</h2>
    <ol>
      <li>Melt the butter and let it cool slightly.</li>
      <li>Beat together the milk, butter and egg.</li>
      <li>Add the flour, baking powder, sugar and salt.</li>
      <li>Mix until just combined. Lumps are fine.</li>
      <li>Drop by the tablespoon into a hot buttered pan.</li>
      <li>Flip when the top is full of little holes.</li>
    </ol>

    <p>
      Adapted from a recipe I have been making since college. More at
      <a href="https://www.nyu.edu">NYU</a>.
    </p>
  </body>
</html>
```

</details>

---

<a id="step-15"></a>

## Step 15 — Inspect it, then push it

Right-click anywhere on your page and choose **Inspect**.

That panel is your HTML, as the browser understands it. You can expand and collapse the tags — and you'll see the nesting you typed, drawn as a tree. Hover over a line and the browser highlights that element on the page.

You'll use this every single class for the rest of the semester. Get comfortable with it now.

Then commit and push, exactly like last class:

```bash
git add .
git commit -m "week 1: recipe in html"
git push
```

---

**You built a web page.** Every tag on it is one you typed and can explain.

Notice what it *doesn't* have: any colours, fonts, or layout. That's deliberate — HTML is the **content and structure**, CSS is the **looks**. Keeping them separate is one of the oldest and best ideas on the web, and CSS starts next week.

**Homework** is in [HW.md](../../HW.md).

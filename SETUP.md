# Setup

Everything you need installed and configured for this class, in one place. Work
through it once; come back to it whenever something breaks.

If you get stuck, check [TROUBLESHOOTING.md](TROUBLESHOOTING.md).

---

## 1. Software

### A browser

Chrome or Firefox. You will live in the **developer tools** — open them with
`Cmd + Option + I` (Mac) or `F12` (Windows).

### VS Code

[Download VS Code](https://code.visualstudio.com/download). This is the editor
we use all semester, and what I teach on.

Install these extensions — open the extensions panel with `Shift + Cmd + X`
(Mac) or `Shift + Ctrl + X` (Windows), search, click Install:

- **Prettier – Code formatter** — formats your code so it stays readable
- **Live Server** — right-click an HTML file, "Open with Live Server", and the
  page reloads in the browser every time you save. You will use this constantly.

Then turn on format-on-save: `Cmd/Ctrl + ,` → search "format on save" → check the
box. Search "default formatter" and set it to Prettier.

### Git

- **Mac:** open Terminal and type `git --version`. If it offers to install the
  developer tools, say yes.
- **Windows:** install [Git for Windows](https://git-scm.com/download/win) and
  accept the defaults. This also installs **Git Bash**, which is the terminal to
  use for this class — it makes your commands match mine.

### A GitHub account

[Create one](https://github.com/join). Use an email you will still have after you
graduate. Your username appears on everything you submit, so pick something you
would put on a résumé.

### Later in the semester

- **Wireframing** — Figma, Keynote, PowerPoint, or anything you can draw
  rectangles in.
- [Adobe Creative Cloud](http://www.adobe.com/creativecloud/buy/students.html) —
  NYU Tandon has a student discount. We don't need Photoshop for several weeks,
  but ask about the discount now.

---

## 2. The terminal

**Terminal** on Mac, **Git Bash** on Windows. Not PowerShell, not Command
Prompt — they behave differently and you'll spend the semester translating.

```bash
ls          # list the files in the folder you are currently in
pwd         # print where you currently are
cd Desktop  # change directory -- move INTO a folder
cd ../      # move back OUT to the parent folder
```

> **Pro tip:** type `cd`, then a space, then **drag a folder onto the terminal
> window**. It fills in the path for you.

---

## 3. Tell Git who you are

Once per computer. Use the same email as your GitHub account.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@nyu.edu"
```

---

## 4. A personal access token

When you push, GitHub asks for a username and password — **and your GitHub
password will not work.** GitHub stopped accepting account passwords for this in
2021. You need a **personal access token** instead: a password with an expiry
date, generated for one purpose.

1. GitHub → your avatar (top right) → **Settings**
2. Scroll to the bottom of the left sidebar → **Developer settings**
3. **Personal access tokens** → **Tokens (classic)**
4. **Generate new token** → **Generate new token (classic)**
5. **Note:** `class laptop`
6. **Expiration:** a date after this semester ends
7. Check the **`repo`** box — the top-level one
8. **Generate token**

**Copy it immediately and paste it somewhere you can find again.** GitHub shows
it to you exactly once. If you lose it, you generate a new one — it is not
recoverable.

A token is a password. Don't commit it, don't share it, don't screenshot it.

So you are not asked every single time you push:

```bash
git config --global credential.helper osxkeychain   # Mac
git config --global credential.helper store         # Windows / Git Bash
```

---

## 5. Two repositories

This is the part that confuses everybody, so let's be explicit.

| | |
|---|---|
| **My repo** (this one) | Class notes and in-class examples. You **clone** it and **pull** it for updates. You never push to it. |
| **Your repo** | Your homework. You create it, you push to it, I grade from it. |

### Clone mine

```bash
cd ~/Desktop
mkdir WebDevClassFiles
cd WebDevClassFiles
git clone https://github.com/Kadee80/WebDevF26.git
```

Any time I add new material:

```bash
cd WebDevF26
git pull
```

> **Don't edit files inside my repo.** If you do, `git pull` starts failing with
> merge conflicts. Copy anything you want to play with into your own folder
> first. If pull is already broken, delete your copy and clone it again — you
> haven't lost anything of your own.

### Make yours

On GitHub: avatar → **Your repositories** → **New**.

- Name it something descriptive — `intro-web-dev-f26` works
- **Public**
- Check **Add a README file**
- **Create repository**

Then clone it next to mine:

```bash
cd ~/Desktop/WebDevClassFiles
git clone [paste your repo url here]
```

`ls` should now show two folders: `WebDevF26` (mine) and yours.

---

## 6. Handing in work

Every assignment, all semester, is this loop. Make a folder for the week inside
**your** repo, put your work in it, then:

```bash
cd ~/Desktop/WebDevClassFiles/your-repo-name
git status                       # what has changed?
git add .                        # stage everything new or changed
git commit -m "week 2 homework"  # save a snapshot, with a message
git push                         # send it to GitHub
```

Then **refresh your repo page on github.com and look at it.** If you can see it
in the browser, I can see it. That is what handing in an assignment means in this
class.

There is a longer walkthrough of this workflow, with more explanation, in
[GitHub_Cheatsheet/README.md](GitHub_Cheatsheet/README.md).

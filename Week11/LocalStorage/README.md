# Local Storage

## 📚 Part 1 — What Is Local Storage? _(10 minutes)_

### The Problem It Solves

Imagine you build a to-do list app. You add five tasks, then refresh the page — and they're all gone. That's because JavaScript variables only live in memory **while the page is open**.

**Local Storage** solves this by letting you save data directly in the user's browser, so it survives page refreshes and even closing the tab.

### Key Facts

| Feature                   | Detail                                  |
| ------------------------- | --------------------------------------- |
| **Where is it stored?**   | In the user's browser (not on a server) |
| **How much can I store?** | ~5 MB per origin                        |
| **Does it expire?**       | No — it persists until cleared          |
| **Who can access it?**    | Only the website that saved it          |
| **Data type**             | Strings only (we use JSON for objects)  |

### The localStorage Object

The browser gives you a built-in object called `localStorage`. It works like a simple key–value dictionary:

```
key         →  value
"username"  →  "alex"
"theme"     →  "dark"
"score"     →  "42"
```

> 💡 **Real-world uses:** Saving user preferences (dark mode), remembering a shopping cart, keeping a user logged in, saving game progress.

---

## 🔧 Part 2 — The Four Core Methods _(15 minutes)_

Open your browser's **DevTools Console** (press `F12` or `Cmd+Option+J`) to follow along live.

### 1. `setItem(key, value)` — Save data

```javascript
localStorage.setItem('username', 'alex')
localStorage.setItem('theme', 'dark')
localStorage.setItem('score', '42')
```

### 2. `getItem(key)` — Read data

```javascript
let user = localStorage.getItem('username')
console.log(user) // "alex"

let theme = localStorage.getItem('theme')
console.log(theme) // "dark"
```

> ⚠️ If the key doesn't exist, `getItem` returns `null`. Always handle this!

```javascript
let missing = localStorage.getItem('doesntExist')
console.log(missing) // null
```

### 3. `removeItem(key)` — Delete one item

```javascript
localStorage.removeItem('score')
console.log(localStorage.getItem('score')) // null
```

### 4. `clear()` — Delete everything

```javascript
localStorage.clear()
// All localStorage data for this site is now gone
```

### ✅ Try It Yourself (5 min)

In your DevTools Console, run these commands one at a time:

```javascript
// 1. Save your name
localStorage.setItem('myName', 'YOUR_NAME_HERE')

// 2. Read it back
console.log(localStorage.getItem('myName'))

// 3. Check the Storage tab in DevTools — you'll see it there!

// 4. Remove it
localStorage.removeItem('myName')

// 5. Confirm it's gone
console.log(localStorage.getItem('myName')) // should be null
```

---

## 🔄 Part 3 — Storing Complex Data with JSON _(10 minutes)_

Remember: localStorage **only stores strings**. So what do we do when we want to save an object or an array?

### The Solution: JSON

**JSON** (JavaScript Object Notation) is a way to convert objects into strings — and back again.

```javascript
// ❌ This won't work as expected
let user = {name: 'Alex', age: 25}
localStorage.setItem('user', user)
console.log(localStorage.getItem('user')) // "[object Object]" 😱
```

```javascript
// ✅ Use JSON.stringify() to convert object → string
let user = {name: 'Alex', age: 25}
localStorage.setItem('user', JSON.stringify(user))

// Use JSON.parse() to convert string → object
let retrieved = JSON.parse(localStorage.getItem('user'))
console.log(retrieved.name) // "Alex"
console.log(retrieved.age) // 25
```

### The Golden Rule

> 📌 **Saving?** Use `JSON.stringify()` → converts object to string  
> 📌 **Loading?** Use `JSON.parse()` → converts string back to object

### Arrays Work the Same Way

```javascript
let tasks = ['Buy groceries', 'Walk the dog', 'Study JS']
localStorage.setItem('tasks', JSON.stringify(tasks))

let loaded = JSON.parse(localStorage.getItem('tasks'))
console.log(loaded[0]) // "Buy groceries"
```

---

## 🛡️ Part 4 — Safe Loading Pattern _(5 minutes)_

What if there's no saved data yet? `getItem` returns `null`, and `JSON.parse(null)` can cause bugs. Always use a safe fallback:

```javascript
// Safe pattern — load or use a default value
let tasks = JSON.parse(localStorage.getItem('tasks')) || []

// Now tasks is always an array, even on first visit
console.log(tasks) // [] if nothing saved yet
```

This `|| []` means: _"use an empty array if nothing was found."_

You can use any default:

```javascript
let theme = localStorage.getItem('theme') || 'light'
let score = parseInt(localStorage.getItem('score')) || 0
let user = JSON.parse(localStorage.getItem('user')) || {name: 'Guest'}
```

---

## 🏗️ Part 5 — Build It: Persistent Notes App _(15 minutes)_

Let's build a mini notes app that **remembers your notes** even after you refresh.

### The HTML (`index.html`)

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>My Notes</title>
    <style>
      body {
        font-family: sans-serif;
        max-width: 500px;
        margin: 40px auto;
        padding: 0 20px;
      }
      textarea {
        width: 100%;
        height: 200px;
        font-size: 16px;
        padding: 10px;
        box-sizing: border-box;
      }
      button {
        margin-top: 10px;
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
      }
      #status {
        margin-top: 10px;
        color: green;
        font-size: 14px;
      }
    </style>
  </head>
  <body>
    <h1>📝 My Notes</h1>
    <textarea id="notepad" placeholder="Start typing your notes..."></textarea>
    <br />
    <button onclick="saveNote()">💾 Save</button>
    <button onclick="clearNote()">🗑️ Clear</button>
    <p id="status"></p>

    <script src="notes.js"></script>
  </body>
</html>
```

### The JavaScript (`notes.js`)

```javascript
// Step 1: Load any saved note when the page opens
window.onload = function () {
  let savedNote = localStorage.getItem('myNote') || ''
  document.getElementById('notepad').value = savedNote
}

// Step 2: Save the note to localStorage
function saveNote() {
  let text = document.getElementById('notepad').value
  localStorage.setItem('myNote', text)

  let status = document.getElementById('status')
  status.textContent = '✅ Saved!'
  setTimeout(() => (status.textContent = ''), 2000)
}

// Step 3: Clear the note from localStorage and the screen
function clearNote() {
  localStorage.removeItem('myNote')
  document.getElementById('notepad').value = ''
  document.getElementById('status').textContent = '🗑️ Cleared.'
  setTimeout(() => (document.getElementById('status').textContent = ''), 2000)
}
```

### 🧪 Test It

1. Type something in the textarea and click **Save**
2. Refresh the page — your note should still be there!
3. Click **Clear** and refresh — the note is gone

---

## 🔍 Part 6 — Inspecting localStorage in DevTools _(5 minutes)_

You can view, edit, and delete localStorage entries directly in your browser:

1. Open DevTools (`F12`)
2. Click the **Application** tab (Chrome) or **Storage** tab (Firefox)
3. In the left sidebar, expand **Local Storage**
4. Click your site's origin (e.g., `http://localhost` or `file://`)
5. You'll see all your key–value pairs — you can double-click to edit them!

> 💡 This is incredibly useful for debugging your apps.

---

## ⚠️ Important Limitations to Know

| Limitation                    | Why It Matters                                 |
| ----------------------------- | ---------------------------------------------- |
| **5 MB limit**                | Don't store large files or images              |
| **Strings only**              | Always use JSON for objects/arrays             |
| **Not secure**                | Never store passwords or sensitive data        |
| **Same origin only**          | `mysite.com` can't read `othersite.com`'s data |
| **Not shared across devices** | Data stays on the local machine                |

---

## 🧠 Knowledge Check

Answer these questions to check your understanding:

1. What method do you use to **save** an item to localStorage?
2. What does `getItem` return if the key doesn't exist?
3. Why do we use `JSON.stringify()` before saving an object?
4. What is the opposite of `JSON.stringify()`?
5. Why should you **never** store passwords in localStorage?

<details>
<summary>👉 Click to reveal answers</summary>

1. `localStorage.setItem(key, value)`
2. `null`
3. Because localStorage only stores strings — `JSON.stringify()` converts objects to strings
4. `JSON.parse()`
5. localStorage is accessible by any JavaScript on the page, making it vulnerable to XSS attacks

</details>

---

## 🚀 Bonus Challenge _(if you finish early)_

Extend the Notes app with these features:

**Level 1 — Easy:**

- Add a character counter that shows how many characters are saved
- Auto-save the note every time the user types (hint: use the `input` event)

**Level 2 — Medium:**

- Support **multiple notes** using an array stored in localStorage
- Add a dropdown to switch between saved notes

**Level 3 — Hard:**

- Add timestamps to each note (when it was last saved)
- Use `JSON.stringify` with the notes array + metadata object

---

## 📖 Summary

| Concept                      | What You Learned                         |
| ---------------------------- | ---------------------------------------- |
| `localStorage.setItem(k, v)` | Save a string value                      |
| `localStorage.getItem(k)`    | Read a value (returns `null` if missing) |
| `localStorage.removeItem(k)` | Delete one key                           |
| `localStorage.clear()`       | Wipe all localStorage for this site      |
| `JSON.stringify(obj)`        | Convert object → string before saving    |
| `JSON.parse(str)`            | Convert string → object after loading    |
| Safe load pattern            | `JSON.parse(...) \|\| defaultValue`      |

---

## 📚 Further Reading

- [MDN: Web Storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)
- [MDN: localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- [MDN: JSON.stringify()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify)
- [MDN: JSON.parse()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse)

---

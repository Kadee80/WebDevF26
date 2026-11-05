# HOMEWORK, Week 10: make it respond

**Due before Tuesday's class.**

Take your own page from Week 09, the random generator you built, whatever it
generates, and make it respond to a click. Not my show page: yours.

If your Week 09 homework did not get finished, do that first. A page with no
state is a page with nothing to wire up.

## What it has to do

- [ ] **One `render()` function** that draws the whole page from your state.
      Everything worked out from something that can change lives inside it.
- [ ] **Your state at the top**, in `let` variables, with a comment saying so.
      Anything that never changes stays `const`.
- [ ] **At least two controls** that change the state and call `render()`. A
      stepper, a regenerate button, a filter, a "next" button, your choice.
- [ ] **One `input` listener** that updates the page as you type. A search box,
      a name, a promo code, a quantity typed rather than clicked.
- [ ] **A guard on at least one control**, a limit that cannot be gone past,
      and a disabled button when you reach it.

## Check it before you push

- Click every control **ten times**, including past its limits. Does anything
  go negative, show `NaN`, or let you do something impossible?
- Change the state in the source, reload, and confirm every part of the page
  follows. Anything that does not move is a calculation left outside
  `render()`.
- Open the console. **Any red is something to fix or explain.**

## If you want to go further

Pick one, not all three:

- **A reset button** that puts the state back to how it started. Remember an
  input's `.value` has to be cleared as well as the state behind it.
- **A keyboard shortcut**, a `keydown` listener on `document` so a key does
  what one of your buttons does. Log `event.key` first to see what you get.
- **A `mouseover`** that shows extra information while the pointer is over
  something, and takes it away when it leaves. Two listeners, one element.

## Common errors this week

| Message | Nearly always |
|---|---|
| `Cannot read properties of null` | a misspelled selector, or a missing `#` or `.` |
| Button works once, on load | brackets after the function name in `addEventListener` |
| One number changes but another does not | a calculation left outside `render()` |
| Nothing happens, no error | the listener never ran, or it is attached to the wrong element |
| `Assignment to constant variable` | state declared with `const` |

## Next week

`classList`: showing, hiding and toggling. You will need a working page with
state in it, so this homework is what next week is built on.

## Submitting

```bash
git add .
git commit -m "week 10 homework"
git push
```

Open your pushed page from GitHub and click everything once. Things that work
locally and not when hosted are usually a path with a capital letter in it.

# Keyframe Animations

*On your own time · ~30 minutes to read, longer if you start playing — and you will*

A **transition** goes from A to B when something triggers it: hover, focus, a
class changing. Two states, one trip, and it needs a reason to start.

A **keyframe animation** runs on its own. As many steps as you like, in any
order, looping forever if you want, with no trigger at all.

If you can describe it as *"when I hover, it gets bigger"* — that is a
transition, and you already know how. If it is *"it pulses constantly"* or
*"it slides in, pauses, then fades"* — that is this.

You met `transform` and `transition` in class. Everything here builds on both.

---

## Two parts, always

**1. Describe the animation** with `@keyframes` and give it a name.

```css
@keyframes pulse {
  from {
    transform: scale(1);
  }
  to {
    transform: scale(1.1);
  }
}
```

**2. Apply it** to something with `animation`.

```css
.heart {
  animation: pulse 1s ease-in-out infinite alternate;
}
```

That is a complete, working animation. The rest of this page is detail.

---

## `from` / `to`, or percentages

Two steps is `from` and `to`. More than two needs percentages — `0%` to `100%`
across the animation's duration:

```css
@keyframes bounce {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(-30px);
  }
  100% {
    transform: translateY(0);
  }
}
```

`from` is just another way of writing `0%`, and `to` is `100%`.

You can stack several percentages on one step when nothing changes between
them, which is how you build in a pause:

```css
@keyframes attention {
  0%, 60% {
    transform: rotate(0deg);     /* still for the first 60% */
  }
  70% {
    transform: rotate(-10deg);
  }
  80% {
    transform: rotate(10deg);
  }
  90% {
    transform: rotate(-5deg);
  }
  100% {
    transform: rotate(0deg);
  }
}
```

Six seconds of that with `animation: attention 6s infinite` is a shake every
six seconds and stillness in between — because 60% of the time, nothing is
happening.

---

## The `animation` shorthand

```css
animation: pulse 1s ease-in-out 0.5s infinite alternate both;
/*         ^     ^   ^          ^    ^        ^         ^
           name  |   timing     |    count    direction fill-mode
                 duration       delay                              */
```

Nobody remembers that order, and you do not have to — the properties can be
written separately, and often should be while you are working things out:

```css
.thing {
  animation-name: pulse;
  animation-duration: 1s;
  animation-timing-function: ease-in-out;
  animation-delay: 0.5s;
  animation-iteration-count: infinite;
  animation-direction: alternate;
  animation-fill-mode: both;
}
```

**The only rule about the shorthand:** the **first** time value is the
duration and the **second** is the delay. `animation: x 1s 3s` waits three
seconds and runs for one. Swap them and you get the opposite.

### The ones worth understanding

**`animation-iteration-count`** — a number, or `infinite`. `3` runs it three
times.

**`animation-direction`** — this is the one that saves you writing steps:

| Value | What happens |
|---|---|
| `normal` | 0% → 100%, jump back to the start, repeat |
| `alternate` | 0% → 100% → 0% → 100%… runs backwards on every other pass |
| `reverse` | 100% → 0% |
| `alternate-reverse` | the same, starting backwards |

`alternate` is why the `pulse` example at the top only needed two steps. With
`normal` it would grow, snap back, grow, snap back.

**`animation-fill-mode`** — what the element looks like *outside* the
animation:

| Value | What happens |
|---|---|
| `none` | before it starts and after it ends, the element uses its normal CSS |
| `forwards` | **stays on the last keyframe** when it finishes |
| `backwards` | takes the first keyframe's look during the delay |
| `both` | both of the above |

**`forwards` is the one you will want.** Without it, an element that fades in
over one second snaps back to invisible the moment the animation ends — which
looks exactly like a bug.

```css
@keyframes fade-in-up {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.card {
  opacity: 0;                                   /* starts invisible */
  animation: fade-in-up 600ms ease-out forwards;
}
```

**`animation-play-state`** — `running` or `paused`. Pair it with `:hover` and
you can stop something on pointer:

```css
.marquee:hover {
  animation-play-state: paused;
}
```

---

## Four you can use today

### Spinner

```css
@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.spinner {
  width: 40px;
  height: 40px;
  border: 4px solid #e4e7ec;
  border-top-color: #c1440e;
  border-radius: 50%;
  animation: spin 800ms linear infinite;
}
```

Only one keyframe is needed — with no `from`, the browser uses the element's
current state as the start. `linear` matters here: a spinner that eases looks
like it is struggling.

### Staggered entrance

```css
@keyframes fade-in-up {
  from { opacity: 0; transform: translateY(20px); }
  to   { opacity: 1; transform: translateY(0); }
}

.card {
  opacity: 0;
  animation: fade-in-up 500ms ease-out forwards;
}

.card:nth-child(1) { animation-delay: 0ms; }
.card:nth-child(2) { animation-delay: 100ms; }
.card:nth-child(3) { animation-delay: 200ms; }
.card:nth-child(4) { animation-delay: 300ms; }
```

Same animation, different delays — the cards arrive one after another. A tenth
of a second apart is about right; much more and it feels slow.

### Gradient text shimmer

```css
@keyframes shimmer {
  to {
    background-position: 200% center;
  }
}

.shimmer {
  background: linear-gradient(90deg, #c1440e, #f0c36d, #c1440e);
  background-size: 200% auto;
  background-clip: text;
  -webkit-background-clip: text;
  color: transparent;
  animation: shimmer 3s linear infinite;
}
```

`background-clip: text` paints the background through the letter shapes. Move
the background and the shine travels across the text.

### Typewriter

```css
@keyframes type {
  from { width: 0; }
  to   { width: 18ch; }
}

@keyframes blink {
  50% { border-color: transparent; }
}

.typewriter {
  width: 18ch;
  overflow: hidden;
  white-space: nowrap;
  border-right: 2px solid currentColor;
  animation: type 2s steps(18) forwards, blink 700ms step-end infinite;
}
```

Two animations on one element, comma-separated. **`steps(18)`** is a timing
function that jumps in discrete chunks rather than gliding — which is what
makes it look like typing rather than stretching. `ch` is the width of one
character, so set it to the length of your text.

---

## Animate these, not those

Browsers animate some properties almost free and others expensively.

**Cheap — use these:**

- `transform` (translate, scale, rotate, skew)
- `opacity`
- `filter`

**Expensive — avoid animating:**

- `width`, `height`, `top`, `left`, `margin`, `padding`

The reason is the same thing you learned with transforms: changing a width
forces the browser to work out the layout of the page again, possibly for every
frame. A `transform` does not move anything, so it can be handed to the
graphics card.

To slide something across, use `transform: translateX(200px)`, not
`left: 200px`. Same look, a fraction of the work, and it will not stutter on a
phone.

---

## The responsible bit: `prefers-reduced-motion`

Some people get genuinely ill from motion on screen — vestibular disorders,
migraine, motion sensitivity. They set a preference in their operating system,
and the browser passes it to you. It takes four lines to respect it:

```css
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

Paste that at the bottom of any stylesheet with animation in it. The page still
works; it just stops moving for the people who asked it to.

You can test it: **System Settings → Accessibility → Display → Reduce motion**
on a Mac.

This is not an optional nicety. It is the difference between a site that is
careful and one that is not, and it is four lines.

---

## Restraint

Keyframes are the most fun thing in CSS and the easiest to ruin a page with.
Some honest guidance:

- **One or two animated things per screen.** If everything moves, nothing reads
  as important.
- **Fast.** 200–500ms for anything triggered. Looping ambient motion can be
  slower, but subtle.
- **Infinite loops are for things that mean something** — a spinner says "still
  working." A heading that pulses forever says nothing and gets annoying in
  about nine seconds.
- **Motion should explain something:** where a thing came from, that something
  arrived, that something is loading. Motion for its own sake is the web
  equivalent of a slide transition in a presentation.
- **Watch it twenty times.** Everything looks good once.

---

## Transition or animation?

| Use a **transition** when | Use an **animation** when |
|---|---|
| there is a trigger — hover, focus, a class | it should start on its own |
| two states, A to B | three or more steps |
| it should reverse when the trigger goes away | it loops |
| a button, a link, a card lifting | a spinner, an entrance, a shimmer |

Most of what you need on a midterm is transitions. Reach for keyframes when
you genuinely need something to move without being asked.

---

## Try it

1. Add a **spinner** somewhere, even if nothing is loading. Get it spinning
   smoothly, then try it with `ease` instead of `linear` and see why `linear`
   wins.
2. Give your midterm's cards or gallery items a **staggered fade-in**. One
   animation, four delays.
3. Take something you already animate on hover with a transition and ask
   whether it would be better as a keyframe animation. Usually the answer is
   no — that is a useful thing to find out.
4. Add the **`prefers-reduced-motion`** block, turn Reduce Motion on in System
   Settings, and reload. Everything should go still.

[MDN: Using CSS animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animations/Using_CSS_animations)
is the reference worth having open.

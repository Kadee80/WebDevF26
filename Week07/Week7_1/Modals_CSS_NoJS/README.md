# Modals with CSS Only

A modal is a pop-up window that sits on top of the page — a sign-up box, an
enlarged photo, a "are you sure?" confirmation. You would assume this needs
JavaScript. It does not. You can build a working one with an anchor tag, an
`id`, and one CSS pseudo-class you have not met yet.

We build this one live. The finished modal lands in
[InClassExercise/](InClassExercise/) after class — read the notes below first,
then follow along.

## The trick: `:target`

You already know that an anchor can jump to another part of the same page:

```html
<a href="#info">Open Modal 1</a>
...
<div id="info">...</div>
```

What you did not know is that when the URL ends in `#info`, the element with
`id="info"` matches a special selector called `:target`. Click the link, the URL
becomes `page.html#info`, and CSS can now style that one element differently.

```css
.info_modal {
  opacity: 0;              /* invisible by default */
  pointer-events: none;    /* and un-clickable */
}

.info_modal:target {
  opacity: 1;              /* visible once its id is the target */
  pointer-events: auto;
}
```

That is the entire mechanism. The link is the button, the `id` is the wiring,
and `:target` is the switch.

## The overlay

The modal has two parts: a full-screen tinted overlay, and the white box sitting
on top of it.

```css
.info_modal {
  position: fixed;         /* fixed to the viewport, not the document */
  top: 0; bottom: 0; left: 0; right: 0;   /* stretch to all four edges */
  background-color: rgba(0, 0, 0, 0.3);   /* 30% black */
  z-index: 99999;          /* above everything else on the page */
}
```

Three things worth naming:

- **`position: fixed` with all four offsets at `0`** is how you cover the whole
  screen. It stays put when the page scrolls, which is what you want — the
  overlay should not scroll away.
- **`rgba(0, 0, 0, 0.3)`** is black at 30% opacity. The fourth value is alpha.
  This is how you tint what is behind something.
- **`z-index`** decides what stacks in front of what. It only works on
  positioned elements — `static` elements ignore it entirely. That is the number
  one reason a z-index "does not work."

## Why `pointer-events` matters

`opacity: 0` makes something invisible, but **it is still there**. Without
`pointer-events: none`, your invisible overlay sits over the whole page
swallowing every click, and nothing on your site works. Try commenting that line
out and clicking a button. This is a bug you will otherwise spend an hour on.

## Fading it in

```css
.info_modal {
  transition: opacity 400ms ease-in;
}
```

**Define the transition on the starting state, not on `:target`.** Put it on the
resting rule and it animates in both directions — opening and closing. Put it
only on `:target` and it snaps shut.

## Centring the box

```css
.center {
  display: flex;
  justify-content: center;   /* horizontally */
  align-items: center;       /* vertically */
}
```

Three lines of flexbox and the modal sits dead centre no matter the screen size.
This used to take a pile of negative margins and guesswork.

## The child selector

```css
.info_modal > div { ... }
```

The `>` means **direct child only** — the white box inside the overlay, not
every `div` nested anywhere below it. Without the `>`, these styles would leak
into any div you later put inside the modal content.

## Closing it

```html
<a href="#close" class="close">X</a>
```

The close button is just a link to an `id` that does not exist. Nothing matches
`:target` any more, so the modal fades back out. Slightly cheeky, completely
valid.

## Where the markup goes

Put your modals at the **bottom of the body**, after your page content. They
render last, which means they land in front without you having to fight the
stacking order.

## In Class Exercise

Add a modal to your own site:

- One trigger link and one modal, using `:target`
- A tinted full-screen overlay with a centred white box
- A close button that dismisses it
- A transition so it fades rather than snaps

Bonus — a second modal on the same page. Remember: new button, new `id`, new
target. Watch out for the copy-paste bug where both buttons point at the same
`id`.

Bonus 2 — make the modal responsive. `width: 600px; height: 600px` is fine on a
laptop and terrible on a phone. Try `max-width` and a percentage instead, and
check it at 375px wide.

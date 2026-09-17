# Box Model and Layout 101

Our pure HTML pages have some default styles applied, but what if we want a more complicated layout?

CSS allows us to take our **HTML content**, and define **styles** for how things look and how a page is layed out visually. This tutorial will go over a few **CSS properties** that effect the **box model**. Once we get these concepts under our belt, we can start worrying about colors, fonts, shadows, and more "designy" properties.

When we looked at semantic markup and wireframes, we used tags like `<header>` or `<aside>` to help organize our content, but nothing really rendered much differently on our page. Today we will start defining styles and introducing some more HTML elements to effect the presentation of a few simple elements. Once we have that under our belt, we will take a look at a couple of common page layouts and how to build them.

> **Quick reference:** [LAYOUTS.md](LAYOUTS.md) has both of today's layouts built
> three ways side by side, with screenshots. For the live version you can poke at,
> open [`layout-playbook.html`](layout-playbook.html) in your browser.

#### A Note on Block vs Inline Elements

* Block elements will always start a new line, and take up the full width of their parent element (or browser). Block elements can receive a height and width.

* Inline elements do not start a new line, and only take up as much space as they require (you can stack them horizontally as long as there is enough room). Inline elements ignore `width` and `height`.

* There's a third option that gives us the best of both: **`display: inline-block`**. It sits side-by-side like an inline element, but accepts `width` and `height` like a block element. That's the tool we're reaching for today.

**^We need to keep this in mind when figuring out our layouts and dimensions.**

> A note on floats: you'll see `float: left` used for layout in a lot of older tutorials and Stack Overflow answers from the 2010s. It works, but it pulls the element out of the normal flow of the page (its parent doesn't even "see" it anymore, which causes all kinds of collapsing-container bugs), and fixing that takes an extra hack called a clearfix. `inline-block` gets us side-by-side boxes without any of that. You don't need floats for layout anymore — flexbox and grid (preview at the end of this doc, deep dive in a few weeks) replaced them for good reason.

## Tag Selectors

Tag/Element selectors apply to every instance of the tag on the page unless overridden by a more specific tag, `.class`, or `#id` selector.

### Global Body Styles

Applying styles to the `body` tag will trickle down to all child components unless overwritten with a more specific style. Let's start by changing some font styles across the entire page.

```css
body{
      font-family: 'Helvetica Neue', Arial, sans-serif;
      color: #555;
      font-style: italic;
    }
```

Now let's use a tag selector to add a transparent background to all divs on our page so we can see our boxes better.

```css
div {
        background-color: rgba(0, 0, 200, 0.1);
    }
```

## Layout 1: A Nav Bar

Nav bars are the classic real-world example of "I need a list of things to sit in a row instead of stacking." Our starting markup is just a `<ul>` of links — completely unstyled, it'll stack vertically because `<li>` is a block element by default.

```html
<nav>
  <ul class="nav">
    <li class="nav-item"><a href="#">Home</a></li>
    <li class="nav-item"><a href="#">Projects</a></li>
    <li class="nav-item"><a href="#">About</a></li>
    <li class="nav-item"><a href="#">Contact</a></li>
  </ul>
</nav>
```

```css
.nav {
    list-style: none;
    margin: 0;
    padding: 0;
}

.nav-item {
    display: inline-block;
    padding: 12px 20px;
    background-color: #006FFF;
}

.nav-item a {
    color: white;
    text-decoration: none;
}
```

Switching `.nav-item` from `display: block` (the `<li>` default) to `display: inline-block` is all it takes to get them sitting in a row. Notice we didn't need a clearfix here — `inline-block` elements stay in the normal flow of the page, so the parent `<ul>` still wraps around them correctly.

### The whitespace gap (inline-block's classic gotcha)

Look closely at the rendered nav bar. There's a small gap between each nav item, even though we haven't set any margin on `.nav-item`. What's going on?

`inline-block` elements live in what's called an *inline formatting context* — the same context that handles the words in a paragraph. And in that context, whitespace in your HTML source (including the line break between `</li>` and `<li>`) renders as a real, visible space, the same way a space between two words does.

A few old-school ways people used to deal with this (you'll see these in older code — you don't need to memorize them):

* Remove the whitespace in the HTML entirely, so tags butt up against each other: `<li>Home</li><li>Projects</li>` — works, but it's ugly and easy to break.
* Comment out the whitespace: `<li>Home</li><!--\n--><li>Projects</li>`
* Set `font-size: 0` on the parent and reset it on the children — works, but now font-size is doing two jobs.

None of these are great. Keep that thought — we'll see how flexbox and grid make this whole problem disappear at the bottom of this doc.

## Layout 2: A Photo Grid

Our second layout is a grid of cards — think a portfolio or photo gallery. Same idea as the nav bar (get block-level boxes to sit in a row), but now we care about the *size* of each box, not just whether it's inline.

```html
<div class="gallery">
  <div class="gallery-item"></div>
  <div class="gallery-item"></div>
  <div class="gallery-item"></div>
  <div class="gallery-item"></div>
</div>
```

## Pixel Based Fixed Layout

```css
.gallery {
        width: 900px;
        margin: 0 auto;
        padding: 10px;
    }
```

* Start with a base width of 900px.
* We can center using `margin: 0 auto` on left/right — margin does not add to the dimensions of our container box. Think of margin as "personal space" outside the box.
* Adding a little padding will add to our original 900px dimensions — now our container is 920px wide, but it gives us a little breathing room.

```css
.gallery-item {
    display: inline-block;
    width: 200px;
    height: 200px;
    vertical-align: top;
}
```

* Give each card a fixed width and height. By default, `.gallery-item` is a `<div>`, which is block, so it'd stack vertically until we switch it to `inline-block`.
* `inline-block` elements default to `vertical-align: baseline`, which lines boxes up like they were letters sitting on a line of text — that gets weird fast once your boxes have different amounts of content in them. `vertical-align: top` fixes that.
* Divs have no height until filled by content or given a height via styles.

*(You should also see the whitespace gap between these cards, just like the nav bar — same cause, same fix options.)*

## Borders and their effect on Box Model Dimensions

Now let's update our layout to get a better understanding of how margins, borders, and padding effect the box model and the final dimensions of our elements.

```html
<div class="gallery">
    <div class="gallery-item-border"></div>
    <div class="gallery-item-border"></div>
    <div class="gallery-item-border"></div>
</div>
```

```css
.gallery-item-border {
    display: inline-block;
    vertical-align: top;
    width: 200px;
    height: 200px;
    border-width: 5px;
    border-style: solid;
    border-color: #006FFF;
}
```

Here we used a hexadecimal color to create a blue border — think `#RRGGBB`, see the section on intro to CSS for more info on color values.

Hey, the border added 10px total width to each box, much like the margin example above — we need to do some math to figure out the new width of our cards when they contain a border.

If we want three cards with a 5px border on left and right to still fit at 200px each:
`200px - 2*5px = 190px` of that 200px is now border, not content.

## Padding and its effect on Box Model Dimensions

Let's see how adding padding effects the layout. Notice in this example we have some content within our divs to show how padding effects our layout. This was generated by http://www.lipsum.com — a great website for generating placeholder text for testing out layouts!

```html
<div class="gallery">
    <div class="gallery-item-border-padding">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam suscipit sit amet purus at iaculis. Morbi lorem metus, facilisis fermentum metus id, tincidunt ullamcorper augue.</div>
    <div class="gallery-item-border-padding">Fusce euismod nisl at dui venenatis, a fermentum quam tristique. Cum sociis natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus.</div>
    <div class="gallery-item-border-padding">Vestibulum id urna facilisis, aliquam lacus eget, euismod lorem. Proin et sollicitudin purus. Sed euismod erat a posuere viverra.</div>
</div>
```

```css
.gallery-item-border-padding {
    display: inline-block;
    vertical-align: top;
    width: 190px;
    height: 190px;
    border: 5px solid #006FFF;
    /* Much nicer, but the padding below adds 40px to each div's dimensions! Rework width/height to account for it. */
    padding: 20px;
    width: 150px;
    height: 150px;
}
```

* Shorthand for border: `width | style | color`
* The text is right up against the border, not very user friendly and hard to read. We fix that with `padding: 20px`.
* But that padding added 40px to each div's dimensions (20px on every side), so we shrink `width`/`height` again to compensate.

## Percentage Based Fluid Layouts

Browser windows come in all sizes — more on media queries and responsive mobile styles later in the course — but if we want a more scalable, fluid layout we can use percentages instead of pixels.

```html
<div class="gallery-fluid">
    <div class="gallery-item-fluid"></div>
    <div class="gallery-item-fluid"></div>
    <div class="gallery-item-fluid"></div>
    <div class="gallery-item-fluid"></div>
</div>
```

```css
.gallery-fluid {
    width: 90%;
    margin: 0 auto;
    padding: 2%;
}
```

* `%` always refers to the percentage of possible width an element can take up, relative to its **parent**. Since `.gallery-fluid` has no parent except `body` (which takes up 100% of the browser by default), our container will take up 90% of the browser width with equal margins on the left and right.

Notice our fixed-pixel card doesn't jive with our percentage-based container so well. Let's make a 4-column grid using percentages. We still need pixels for height, so our empty divs have something to display.

```css
.gallery-item-fluid {
    display: inline-block;
    vertical-align: top;
    width: 25%;
    height: 200px;
    margin: 0 1%;
}
```

We added some spacing via margin to our cards.

Note that `4 * 25% + 8 * 1% = 108%`. That's going to push the last card onto a new line. Why `* 8`? Because we have 1% on the left *and* 1% on the right of each of our 4 cards. Let's fix the math so we land on 100%:

* `100% - 8*2% = 84%` left over for our card widths, once we bump the margin to 2% each side.
* Split 4 ways: `84% / 4 = 21%`.

```css
.gallery-item-fluid {
    display: inline-block;
    vertical-align: top;
    width: 21%;
    margin: 0 2%;
}
```

*Doing this margin math by hand, every time, for every layout, is exactly the kind of busywork flexbox and grid exist to get rid of — see below.*

## The Modern Way: Same Layouts, Flexbox and Grid

Everything above is a real, useful technique — `inline-block` still shows up in real codebases, and the box-model math you just did (border, padding, percentages) applies **everywhere**, including flexbox and grid. But watch what happens to our two layouts when we change one property on the parent.

### Nav bar → Flexbox

```css
.nav {
    display: flex;
    gap: 8px;
    list-style: none;
    margin: 0;
    padding: 0;
}
```

`display: flex` on the parent is enough to lay the `<li>` children out in a row — no `inline-block` needed on the children at all, and the whitespace-gap problem is simply gone. `gap: 8px` gives us real spacing, with no margin math and no doubled-up margins to account for.

### Photo grid → CSS Grid

```css
.gallery {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 2%;
}

.gallery-item {
    height: 200px;
}
```

Compare `grid-template-columns: repeat(4, 1fr)` to the percentage math we just did by hand (`84% / 4 = 21%`, remember to double the margins...). Grid does that arithmetic for us, and `gap` handles spacing without the doubled-up-margin trick.

All three versions of both layouts are lined up side by side in [LAYOUTS.md](LAYOUTS.md), and running live in [`layout-playbook.html`](layout-playbook.html).

**We're not teaching flexbox or grid syntax in depth today** — that's Week 4 for flexbox and Week 9 for CSS Grid. For now, just notice what disappeared: no whitespace-gap hack, no `vertical-align: top` fix, no percentage math, and a real `gap` property. Everything you learned today about width, height, border, padding, and margin still applies once we get there — the box model doesn't change, just the tools for arranging boxes.

## In Class Exercise:

Our cards below need some air between them. Using `display: inline-block` and pixel margins, rework either the container class (addition) or the card class (subtraction) to make the layout work in 1 row and 4 columns — and deal with the whitespace gap however you'd like.

Use the following new class names and start from scratch.

```
<h3>In Class Exercise</h3>
<div class="gallery-inclass">
    <div class="gallery-item-inclass"></div>
    <div class="gallery-item-inclass"></div>
    <div class="gallery-item-inclass"></div>
    <div class="gallery-item-inclass"></div>
</div>
```

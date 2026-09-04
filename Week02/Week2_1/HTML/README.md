# Hello HTML!

> **Slides:** [HTML_Basics.pptx](HTML_Basics.pptx)
>
> **Missed this class, or want to follow along again?**
> [WALKTHROUGH.md](WALKTHROUGH.md) rebuilds this page from an empty file in ten
> steps, showing exactly what changed at each one. The finished code is in
> [end-of-class/](end-of-class/).

Everything in this lesson uses the same example: a recipe. You wrote one in
plain text last week. Today we mark it up, and by the end of class it is a real
web page. Next class we build the form someone would use to submit a recipe to
us.

**HTML** stands for **HyperText Markup Language**.

**Hypertext** = text with links in it.

**Markup Languages** are designed for the processing, definition and presentation of text. Text can become tables, lists, images and more.

**HTML** provides the _structure_ of the page.

**CSS** (Cascading Style Sheets) provides the visual layout of the page.

- HTML = content.
- CSS = style.
- HTML + CSS = good looking content!

## What is a tag?

HTML tags are keywords (tag names) surrounded by angle brackets and are used to lay out the web page.

Tags are also called elements.

Tags usually come in pairs: an opening tag and a closing tag.

`<tagname>` content `</tagname>`
Opening tag: `<tagname>`
Closing tag: `</tagname>`

_notice the end tag contains a slash before the tag name!_

## Page structure:

There are always two parts to an HTML file: the `<head>` and the `<body>`.

The head contains information _about_ your HTML file — its title, its character
encoding, later on its stylesheet. Nothing in the head is drawn on the page.

The body is where you put your content, such as text, images, and links. The content in the body is what will be visible on your webpage.

The body goes inside the `<html>` tags, right after the closing `</head>` tag.

_Placing one HTML tag inside of another is called nesting._

You can think of tags as being like parentheses: whenever you open one, you should close it. Tags also nest, so you should close them in the right order.

```html
<p>Adapted from <a href="https://www.foodandwine.com/">Food &amp; Wine</a></p>
```

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Buttermilk Pancakes</title>
  </head>
  <body>
    <h1>Buttermilk Pancakes</h1>
    <h3>Ingredients</h3>
    <p>Light, fluffy, and about twenty minutes start to finish.</p>
  </body>
</html>
```

Let's take a look at the code above. There are a few tags (elements) to review.

#### The title tag:

```html
<title>Buttermilk Pancakes</title>
```

The title tag is always nested inside the `<head></head>` of our document. The title tag does a few things. Whatever text you put between the title tags will show up at the top of your browser tab. The title tag (along with a few others we will discuss later) also tells search engines about the content of your webpage. The title text is what shows up as the link in search results.

So `<title>Buttermilk Pancakes</title>` is a good title. `<title>Document</title>`,
which is what your editor gives you for free, is not.

#### The paragraph tag:

The majority of the content on webpages are probably paragraphs.

```html
<p>
  Light, fluffy pancakes that take about twenty minutes start to finish. Makes
  roughly eight.
</p>
```

##### Bold and Italic text within a paragraph:

Sometimes we need to emphasize or strengthen some of the words in a paragraph.

For italic text, we wrap the word, words, or sentence with the `<em></em>` (short for emphasis) tag:

```html
<p>The trick is <em>not</em> overmixing the batter.</p>
```

For bold text, we wrap the word, words or sentence with the `<strong></strong>` tag:

```html
<p>These take about <strong>twenty minutes</strong> start to finish.</p>
```

Both tags are about **meaning**, not about looks. `<em>` means "read this with
emphasis," and italic is just the browser's default way of showing that. When
you want something italic purely because it looks nicer, that is a CSS job.

#### Headings and Hierarchy:

Think of headings as the "titles" of the sections of your webpage. The number of the heading tag describes how important that section is — it is an outline, not a set of font sizes.

There are 6 heading sizes:

```html
<h1>Buttermilk Pancakes</h1>
<h2>Ingredients</h2>
<h2>Instructions</h2>
<h2>Notes</h2>
<h3>Substitutions</h3>
<h3>Leftovers</h3>
<h4>Freezing</h4>
<h4>Reheating</h4>
```

Read those top to bottom and you have the recipe's table of contents. That is
exactly what a screen reader announces when someone asks it to list the sections
of your page.

**Only one `<h1>` per page**, and do not skip levels to get a size you like.

#### Links and Anchors:

What if you want to send a user to another part of your site or an external site? You use hyperlinks/links:

```html
<a href="https://www.foodandwine.com/buttermilk-pancakes-7089420">
  Original recipe
</a>
```

You'll notice there is some extra text in our opening anchor tag:

`href="https://www.foodandwine.com/buttermilk-pancakes-7089420"`

That is an **attribute** named **href**!

The href value tells your link where to go once it is clicked by a user.

An **attribute** is a special code that can enhance or modify a tag. They are usually located in the starting tag after the tag name. We will see a lot of them as we progress.

Two more you will use today:

```html
<!-- opens in a new tab -->
<a href="https://www.foodandwine.com/" target="_blank">Food &amp; Wine</a>

<!-- a link you have not written yet: # is the universal placeholder -->
<a href="#">Nutrition info, coming soon</a>
```

#### Adding images to a page:

What about images? We add them like this:

```html
<img
  src="https://placecats.com/300/300"
  alt="A placeholder cat, standing in for a photo we have not taken yet"
/>
```

Whoa, there's another one of those attributes! The `src` attribute tells us the location of the image file. Every image on the web has its own image URL — right-click an image and choose "Copy image URL," then paste that URL in quotes after `src=`.

More often, though, the image lives in your own project folder next to your HTML file:

```html
<img
  src="./img/pancakes.jpg"
  alt="A tall stack of buttermilk pancakes with butter melting on top"
/>
```

`./img/` means "the folder named img, sitting right next to this HTML file." If
your image does not show up, that path is the first thing to check — nine times
out of ten the file is somewhere else, or the extension is `.JPG` and you typed
`.jpg`.

##### About that `alt` attribute:

`alt` is the text a screen reader reads out loud in place of the image, and the
text you see if the image fails to load. It is not optional and it is not the
filename. Describe what is in the picture.

```html
<!-- no -->
<img src="./img/pancakes.jpg" alt="pancakes.jpg" />

<!-- yes -->
<img src="./img/pancakes.jpg" alt="A tall stack of pancakes, butter melting" />
```

But let's talk a little more about the image tag itself.

Remember paragraphs and headings and anchor tags all had opening and closing tags:

```html
<p></p>
<h1></h1>
<a href="#"></a>
```

The image tag does not have a closing tag. It closes itself:

```html
<img src="imagesource.jpg" />
```

These are called self closing tags. We will see a few of them as we move forward.

##### A note about indentation:

You'll notice that when we nest tags, we indent them more. This helps us keep track of the hierarchy of the site and makes the code way more readable.

```html
<!DOCTYPE html>
<html>
  <head>
    <title></title>
  </head>
  <body></body>
</html>
```

#### Lists and List Items:

There are 2 kinds of lists. Ordered lists and unordered lists. You guessed it. Ordered lists have numbers, and unordered lists do not.

A recipe happens to need one of each:

Ingredients are an **unordered** list — you need all of them, but there is no
first one:

```html
<ul>
  <li>3/4 cup melted butter</li>
  <li>2 tbsp whole milk</li>
  <li>1 egg</li>
</ul>
```

Instructions are an **ordered** list — do them in this order or you get soup:

```html
<ol>
  <li>Melt the butter and let it cool slightly.</li>
  <li>Beat together the milk, butter and egg.</li>
  <li>Add the flour, baking powder, sugar and salt.</li>
</ol>
```

You will notice that the `<ol></ol>` and the `<ul></ul>` tags just wrap the `<li></li>` list items within them. Each list item is one "bullet point." Nothing else goes directly inside a `<ul>` — no stray paragraphs, only `<li>`.

They help separate this list content from surrounding paragraphs and headings, and will be very useful when we start styling these elements with CSS.

## Live Code: Our first HTML page!

We build the recipe page together, from an empty file. Follow along in
[WALKTHROUGH.md](WALKTHROUGH.md) if you fall behind or want to replay it later.

## In Class Exercise

- In your hw repo, you should have uploaded a plain text file with a favorite recipe.
- Copy the plain text from that file into the `<body>` of a new `index.html` file.
- Using the HTML tags we just covered, mark up the plain text recipe so that it renders as valid HTML.
  - One `<h1>` for the dish name
  - `<h2>` for Ingredients and Instructions
  - `<ul>` for ingredients, `<ol>` for steps
  - At least one `<strong>` or `<em>` where the recipe actually needs emphasis
- Bonus — find an image of the dish. Link to it with an image tag, and write a
  real `alt` description for it.

[Tag reference: StarterFiles/index.html](StarterFiles/index.html) is
the annotated version of every tag above, all in one recipe page. Open it in the
browser and in your editor side by side.

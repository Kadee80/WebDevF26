# Intro to HTML Form Elements

Last class we marked up a recipe as a web page. But how did that recipe get to
us in the first place? Somebody typed it into a form.

So that is what we are building today: the **Class Cookbook submission form**.
Every input type in this lesson earns its place because this one form actually
needs it — a dish has one difficulty (radio), several dietary tags (checkbox),
a number of servings (number), a photo (file), and so on.

[StarterFiles/index.html](StarterFiles/index.html) is the page shell — the
heading and the intro, with a comment where the form goes. **Copy it into your
own repo and build the form along with me in class.** Everything we write
together lands in [InClassExercise/](InClassExercise/) afterwards, so if you
fall behind, keep watching and pick the code up from there.

---

Aside from text and images, sometimes we need to collect/submit data from the user. We do this with a `<form>` and various `<input>` elements.

At the very minimum, we need to nest our inputs inside of our form element:

```html
<form>
  <input type="text" />
  <input type="submit" />
</form>
```

The form above leaves a LOT to be desired. We have a text box and a submit button. But what kind of data are we sending? Where does it go? We need to add a few form specific attributes to make a functional, semantically defined form.

```html
<form name="submitRecipe" action="addRecipe.php" method="POST">
  ...
</form>
```

The form `action` attribute usually contains a script or file to process the form data. For example `addRecipe.php` or `createuser.asp`. In our example we just have a dummy `#`, because we have no backend yet.

The form `method` attribute is either GET or POST. We will cover this more when we dive into JavaScript. For now consider a few facts about these methods:

`GET` will submit data via a URL and query string, for example:

`http://mysubmitscript.js?dishName=Pancakes&servings=8`

That is fine for a search. But watch what happens when there is a password in the form:

`http://mysubmitscript.js?email=katie@nyu.edu&password=pass123`

YIKES! That URL gets cached in my browser and written to my browser history. In general, `GET` requests should only be used to **retrieve** data. (Stay tuned for APIs after midterms!)

`POST` requests submit data as the body of a POST request. Think of a message with a destination, and the data in a sealed envelope. Our cookbook form is **writing** a new recipe, and it collects a password, so it is a POST.

You can read up more on [GET vs POST here](http://www.w3schools.com/tags/ref_httpmethods.asp). This doesn't concern us in week 2 of intro to web dev, but it is good to be aware of these things, right?

The `name` attribute is particularly helpful if you have multiple forms on the same page, or even the same site. For example, `name="submitRecipe"` vs `name="login"`. This becomes important when you start writing scripts to handle different form submissions. For now, just name your forms. They describe what the form is for.

---

OK, now we have our form element filled out with the appropriate attributes. (Note: if you don't have scripts to handle a `method` or `action`, use a `#` as a placeholder.) Let's start adding some basic form `input` types, `labels` and their corresponding `attributes`.

```html
<form name="submitRecipe" action="#" method="POST">
  <label for="cookName">Your name: </label>
  <input type="text" name="cookName" id="cookName" required />

  <label for="dishName">Dish name: </label>
  <input
    type="text"
    name="dishName"
    id="dishName"
    placeholder="Buttermilk Pancakes"
    maxlength="60"
  />

  <label for="password">Pick a password: </label>
  <input type="password" name="password" id="password" />

  <button type="submit">Add to the cookbook</button>
</form>
```

First, let's talk about `<label>`. You guessed it, it's a label for a form input! Notice how, unlike a `<p>` element, it does not `clear` the entire width of its container. That is because `<label>` and `<input>` are *inline* elements. More on **inline** vs **block** next week when we start tackling some basic CSS.

Just remember:

- Block elements always start a new line and take up the full width of their parent element (or browser).
- Inline elements do not start a new line, and only take up as much space as they require (you can stack them horizontally as long as there is room).

```html
<label for="cookName">Your name: </label>
<input type="text" name="cookName" id="cookName" />
```

Notice `<label>` has an attribute `for="cookName"`. That matches the `id="cookName"` on the input it labels. Get this right and clicking the words "Your name" puts the cursor in the box — and a screen reader announces the right label when the user tabs into the field. Get it wrong and the field is just an unexplained rectangle.

**Rule of thumb: the label's `for`, the input's `name`, and the input's `id` should all be present and all be the same value.**

The `name` attribute is very important! Name your form inputs wisely — this is how our data will be tagged when submitted to a database. Remember the GET request? A submitted cookbook form would look something like:

```
http://mysubmitscript.js?cookName=Katie&dishName=Buttermilk+Pancakes&servings=8
```

Aside from `name` we also have an input `type`. There are many types. Here are the ones our cookbook form uses, and why:

| Input | Why the cookbook needs it |
| --- | --- |
| `type="text"` | Your name, the dish name |
| `type="email"` | Where we send you the link — the browser checks for an `@` |
| `type="password"` | Your cookbook account |
| `type="number"` | Servings, prep time in minutes |
| `type="range"` | Spice level, 0 to 5 |
| `type="radio"` | Difficulty — a dish has exactly **one** |
| `type="checkbox"` | Dietary tags — a dish can have **several** |
| `type="file"` | A photo of the finished dish |
| `type="url"` | A link to the original recipe |
| `type="date"` | The last time you made it |
| `type="color"` | The color of your page in the cookbook |
| `type="submit"` | Send it |

There are a few more you will run into: `hidden`, `month`, `week`, `time`, `reset`, `button`. Notice how the browser renders each type differently, with no CSS from you at all.

Let's take a look at our dish name input. Notice the extra attributes:

```html
<label for="dishName">Dish name: </label>
<input
  type="text"
  name="dishName"
  id="dishName"
  placeholder="Buttermilk Pancakes"
  maxlength="60"
/>
```

`placeholder` is a greyed-out hint that disappears as soon as the user types. It is **not** a label — if you delete the label and rely on the placeholder, the field becomes unlabeled the moment someone starts typing.

`maxlength` limits the entry to 60 characters. `required` refuses to submit an empty field. `min`, `max` and `step` do the same job for numbers:

```html
<label for="servings">Servings:</label>
<input type="number" name="servings" id="servings" min="1" max="24" />

<label for="prepTime">Prep time (minutes):</label>
<input type="number" name="prepTime" id="prepTime" min="0" max="480" step="5" />
```

## Radio vs checkbox

This is the one people get wrong, and the cookbook makes the difference obvious.

**Difficulty is a radio group.** A recipe is easy or medium or hard — one answer. Same `name`, different `value`, and the browser enforces "pick one":

```html
<input type="radio" name="difficulty" id="easy" value="easy" />
<label for="easy">Weeknight easy</label>

<input type="radio" name="difficulty" id="hard" value="hard" />
<label for="hard">Clear your afternoon</label>
```

**Dietary tags are checkboxes.** A dish can be vegetarian *and* gluten free. Same `name`, different `value`, any number checked:

```html
<input type="checkbox" name="diet" id="veg" value="vegetarian" />
<label for="veg">Vegetarian</label>

<input type="checkbox" name="diet" id="gf" value="gluten-free" />
<label for="gf">Gluten free</label>
```

> If you can select more than one radio button in the same group, you have a typo. Grouping only works when every input in the group has the **exact same `name`**.

Also notice: the words the user reads live in the `<label>`. The `value` is what gets sent to the database. When you copy-paste a row, **change both**. A form where every checkbox submits `value="dog"` is the single most common bug in this lesson.

## Longer answers: textarea

Ingredients and instructions are too long for a one-line input, so they get `<textarea>`:

```html
<label for="ingredients">Ingredients, one per line:</label>
<textarea name="ingredients" id="ingredients" cols="50" rows="8"></textarea>
```

`textarea` has a closing tag and no `value` attribute. Anything you type *between* the tags is the starting content — so keep them jammed together like above, or your box opens with a mysterious blank line already in it.

## Dropdowns: select + option

```html
<label for="course">Course:</label>
<select name="course" id="course">
  <option value="breakfast">Breakfast</option>
  <option value="main">Main</option>
  <option value="dessert">Dessert</option>
</select>
```

The `<select>` carries the `name`. Each `<option>` carries a `value` — that is what writes to the database — and the text between the tags is what the human reads. They do not have to match.

## Buttons

```html
<button type="submit">Add to the cookbook</button>
<button type="reset">Start over</button>
```

Inside a form, a `<button>` submits by default. `type="reset"` wipes every field — users hate it, and it is included here mostly so you recognize it. A `disabled` input is greyed out and its value is **not** submitted at all.

## Live code: the Class Cookbook form

We build it fieldset by fieldset. `<fieldset>` and `<legend>` are old-school tags that group inputs visually with no CSS at all — think of them as form groups. Code goes into the repository as we move along.

## In Class Exercise

Take the recipe page you marked up last class and add a form to it:

- A `<form>` with a `name`, `method` and `action`
- One text input, correctly labeled — `for`, `name` and `id` all matching
- One radio group and one checkbox group, and be ready to say why each one is the type it is
- A `<textarea>`
- A submit button

Bonus — add a `<select>` dropdown and one input type that is not in the list above. There are a lot of them; find one and tell us what it does.

## 4 ways of fixing a "select" form field bad practice

# The firestarter

Yesterday, a younger colleague of mine and extremely talented designer, shared his latest mockup on our Basecamp.

As always, it was beatiful and well balanced... until my heart suddenly started to bleed while my inner Caesar was screaming: *"Et tu, Brute?"*

This is what I tweeted shortly after:

%[https://twitter.com/trumbitta/status/469399930738835456]

# Challenge accepted

Over the years, I often found myself having to explain to interns and young colleagues why this:

```html
<select>
  <option value=" ">Choose one...</option>
  <option value="foo">First choice</option>
  <option value="bar">Second choice</option>
</select>
```

is in fact always a bad practice, and how to avoid it in a bunch of different situations.

This time I'm trying to explain my opinion on the topic once and for all, really just hoping to end up having a handy link to point people to when needed.

# Why "Choose one..." is bad

Let's examine the following HTML code:

```html
<select name="icecream">
  <option value=" ">Choose one...</option>
  <option value="banana">Banana</option>
  <option value="cream">Cream</option>
  <option value="smurf">Smurf</option>
</select>
```

The problem is, that first option doesn't have any purpose which is suitable for an option.

That first "Choose one..." option is there just to make you choose the actual option you wanted to choose in the first place!

![Madness](https://cdn.hashnode.com/res/hashnode/image/upload/v1625782682675/ek3fxBXT4.jpeg)
<figcaption>This is madness, and I promise you no Spartan ain't going to kick me down a well for writing it.</figcaption>

It doesn't make any sense to have "Choose one..." as an option.

Making you want to choose an `<option>`, is the job of a `<label>`. Not of another `<option>`.

# And how to fix it

## The basics

The first rule of the *Fight The "Choose one..." Madness Club* is you always talk about it and you also use a `<label>`.

```html
<label for="icecream">Choose an icecream:</label><br />
<select name="icecream" id="icecream">
  <option value="banana">Banana</option>
  <option value="cream">Cream</option>
  <option value="smurf">Smurf</option>
</select>
```

The second rule being: if you really, really have to use the word *"Choose"*, use it inside the `<label>`.

Now that we know the basics, let's see them in action in a couple different scenarios and let's also see how you can choose proper first options for each one of those scenarios.

## 1. A filter-like `<select>`, mandatory choice

When the `<select>` is used to filter a list of possible items, you nearly always have to include a first *show them all* option.

```html
<label for="superheroes">Show superheroes:</label><br />
<select name="superheroes" id="superheroes">
  <option value="all">All</option>
  <option value="flying">Flying</option>
  <option value="super-force">Super-force</option>
  <option value="super-sight">Super-sight</option>
</select>
```

### Notes:

1. The value of the *show them all* `<option>` could be empty.  
It depends on your backend implementation and coding style.
1. Since in this example the choice is mandatory, having the *show them all* option as the first one perfectly fits the requirement.
1. A *don't actually filter anything* option is often a good default one.

## 2. A filter-like `<select>`, not mandatory choice

Ok, I cheated. The first example is valid in both cases.

It doesn't really matter if the choice is mandatory or not: when you have a filter-like `<select>`, you can always avoid using the dreadful "Choose one..." first option. Just stick with the winning `<label>` + *show them all* first option combo.

## 3. A simple choose-one `<select>`, mandatory choice

When the `<select>` is used inside a form to make you choose one out of some possible options, things can get tricky for the inexperienced *Padawan*.

Good for us I'm writing this very blog post!

Consider this:

```html
<label for="opponent">Choose your opponent:</label><br />
<select name="opponent" id="opponent">
  <option value="godzilla">Godzilla</option>
  <option value="optimus_prime">Optimus Prime</option>
  <option value="shaq">Shaq</option>
  <option value="batman">Batman</option>
  <option value="chuck_norris">Chuck Norris</option>
  <option value="indiana_jones">Indiana Jones</option>
</select>
```

If Godzilla is the default opponent you want poor human fighters go against every time, your job here is done.

But you could also be a bit less evil and let those warriors try themselves against Indiana Jones by default – beware that gun, though!

{% youtube 4DzcOCyHDqc %}

Nothing easier than that. Just add a `selected="selected"` attribute (or just `selected`) to the desired `<option>`:

```html
<label for="opponent">Choose your opponent:</label><br />
<select name="opponent" id="opponent">
  <option value="godzilla">Godzilla</option>
  <option value="optimus_prime">Optimus Prime</option>
  <option value="shaq">Shaq</option>
  <option value="batman">Batman</option>
  <option value="chuck_norris">Chuck Norris</option>
  <option value="indiana_jones" selected="selected">Indiana Jones</option>
</select>
```

*Et voilà!* Indy is the first opponent.

## 4. A simple choose-one `<select>`, not mandatory choice

Now what if the choice is not mandatory, what if I can skip it and walk away without fighting vs anyone?

You add a *Monty Brewster* option and call it a day.

![None of the above](https://cdn.hashnode.com/res/hashnode/image/upload/v1625782684720/WvTK0-9Wk.jpeg)
<figcaption>Sometimes a choice can be a non-choice – Confucius.</figcaption>

Wait, what's a *Monty Brewster* option?

The following is a simple choose-one `<select>` with a not mandatory choice, which uses a *Monty Brewster* option instead of that bad bad bad "Choose one..." option.

```html
<label for="opponent">Choose your opponent:</label><br />
<select name="opponent-2" id="opponent-2">
  <option value=" ">None</option>
  <option value="godzilla">Godzilla</option>
  <option value="optimus_prime">Optimus Prime</option>
  <option value="shaq">Shaq</option>
  <option value="batman">Batman</option>
  <option value="chuck_norris">Chuck Norris</option>
  <option value="indiana_jones">Indiana Jones</option>
</select>
```

### Notes:

1. Just like before, the value of the *Monty Brewster* `<option>` can be anything you want. It depends on your backend implementation and coding style.
1. If you want to be super-sure that any browser will always render your desired option as the default one, just add the `selected="selected"` attribute.

## Closing thoughts
As I hope I have demonstrated, it is always possible to avoid using the "Choose one..." first option in a `<select>`.

Using it is cheap, lazy, confusing to newcomers, and now you have this blog post with examples on how to implement the right thing.

What do you think about it?
Did you find a use case I forgot to cover?

Let me know on [Twitter](https://twitter.com/trumbitta), or leave a comment!

> Originally posted here: https://www.williamghelfi.com/blog/2014-05-24-select-form-field-bad-practices-and-how-to-fix-it/

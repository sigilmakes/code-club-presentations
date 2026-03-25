---
title: What the hell is a Monad?
info: |
  Code Club Talk — Willow Sparks

theme: seriph
themeConfig:
  primary: '#d4639a'
  secondary: '#9b72cf'
  background: '#1a1020'

drawings:
  persist: false

class: text-center
transition: fade-out
mdc: true
layout: cover
hideInToc: true

download: true
---

# What the hell is a Monad?

Willow Sparks · Code Club · 1 April 2026

---
layout: center
hideInToc: true
---

# "A monad is just a monoid in the category of endofunctors"

<v-click>

<div class="mt-8 text-2xl" style="color: #d4639a">

...yeah, let's ignore that.

</div>

</v-click>

---
layout: center
---

# A monad is a box you can put things in.

<v-click>

<div class="mt-8 text-lg opacity-70">

That's it. That's the starting point. Let's unpack it.

</div>

</v-click>

---

# The Problem

Things go wrong. Values might be missing. And Python makes you deal with it manually.

<v-click>

```python
user = get_user(42)           # might return None
if user is not None:
    address = get_address(user)
    if address is not None:
        postcode = get_postcode(address)
        if postcode is not None:
            print(postcode)
```

</v-click>

<v-click>

<div class="mt-4" style="color: #d4639a">

Every step needs a check. It gets worse the more steps you have.

What if the box could handle this for us?

</div>

</v-click>

---

# A Smarter Box: Maybe

What if our box *knew* it might be empty — and handled that automatically?

<v-click>

```python
class Maybe:
    def __init__(self, value):
        self.value = value

    def bind(self, func):
        if self.value is None:
            return Maybe(None)    # empty? skip it
        return func(self.value)   # otherwise, run the function
```

</v-click>

<v-click>

```python
# Same example as before — but now the box handles the None checks
Maybe(42).bind(get_user).bind(get_address).bind(get_postcode)

# If get_user returns Maybe(None), the chain stops.
# No ifs. No crashes. No pyramid of doom.
```

</v-click>

<v-click>

<div style="color: #d4639a">

The box handles the emptiness between steps for you.

</div>

</v-click>

---

# The Same Trick, Different Box: Lists

A list is a box that holds **multiple values at once**. Bind means: apply a function to each one, and flatten the results.

<v-click>

```python
def expand(x):
    return [x, x * 10]

# bind: apply to each element, flatten
[1, 2, 3]  →  expand each  →  [1, 10, 2, 20, 3, 30]
```

</v-click>

<v-click>

You already do this — it's a list comprehension:

```python
[(x, y) for x in [1, 2, 3] for y in ['a', 'b']]
# [(1,'a'), (1,'b'), (2,'a'), (2,'b'), (3,'a'), (3,'b')]
```

Each `for` is a bind. The list box handles the iteration and combination for you.

</v-click>

<v-click>

<div class="mt-4" style="color: #9b72cf">

Maybe handles missing values. List handles multiple values. Same pattern.

</div>

</v-click>

---

# The Pattern

So what do these boxes have in common? Three ingredients:

<v-clicks>

1. **A box type** — wraps a value
   - `Maybe(42)`, `[1, 2, 3]`

2. **Wrap** — put a value in the box
   - `Maybe(x)`, `[x]`

3. **Bind** — chain operations: `box.bind(f)` gives you a new box
   - Maybe's bind skips if empty
   - List's bind applies to each element and flattens

</v-clicks>

<v-click>

<div class="mt-4" style="color: #d4639a">

A box + wrap + bind. That's a monad.

Where a class hides **state**, a monad hides **control flow**.

</div>

</v-click>

---

# Lifting: What a Functor Does

A functor takes a function between values and **lifts** it to a function between boxes.

<v-click>

<div class="mt-8 flex justify-center">
<div class="text-center">

<div class="flex items-center gap-4 justify-center text-xl">
  <div class="px-4 py-2 rounded" style="background: rgba(155, 114, 207, 0.2)">5</div>
  <div>──<code>double</code>──→</div>
  <div class="px-4 py-2 rounded" style="background: rgba(155, 114, 207, 0.2)">10</div>
</div>

<div class="flex items-center gap-4 justify-center my-4 text-sm opacity-60">
  <div>↓ wrap in list</div>
  <div class="w-24"></div>
  <div>↓ wrap in list</div>
</div>

<div class="flex items-center gap-4 justify-center text-xl">
  <div class="px-4 py-2 rounded" style="background: rgba(212, 99, 154, 0.2)">[5]</div>
  <div>──<code>map(double)</code>──→</div>
  <div class="px-4 py-2 rounded" style="background: rgba(212, 99, 154, 0.2)">[10]</div>
</div>

</div>
</div>

</v-click>

<v-click>

<div class="mt-8 text-center">

```python
def double(x):  return x * 2       # a function between numbers
list(map(double, [5]))              # the same function, between lists
```

</div>

</v-click>

<v-click>

<div class="mt-4 text-center" style="color: #d4639a">

The box doesn't change what the function does — it just lets it work on boxed values.

Every monad is a functor. This is what <code>map</code> means.

</div>

</v-click>

---

# The Three Rules

What makes a box a *monad* and not just a box?

<v-clicks>

1. **Left identity:** `Maybe(a).bind(f)` = `f(a)`
   - Putting a value in a box and immediately using it should be the same as just using it. The box doesn't get in the way.

2. **Right identity:** `m.bind(Maybe)` = `m`
   - If all you do is re-wrap the value, nothing changes. The box doesn't add anything you didn't ask for.

3. **Associativity:** `m.bind(f).bind(g)` = `m.bind(lambda x: f(x).bind(g))`
   - It doesn't matter how you group a chain of operations — the result is the same. You can refactor freely.

</v-clicks>

---

# A Real Example: Processing Experimental Data

Without monads — every step needs its own check:

<v-click>

```python
data = read_csv("readings.csv")
if data is not None:
    column = parse_floats(data, "temperature")
    if column is not None:
        cleaned = remove_outliers(column)
        if cleaned is not None:
            result = mean(cleaned)
            if result is not None:
                print(f"Mean temperature: {result}")
```

</v-click>

<v-click>

With Result — the box handles it, *and* tells you what went wrong:

```python
result = (Result.ok("readings.csv")
    .bind(read_csv)
    .bind(lambda d: parse_floats(d, "temperature"))
    .bind(remove_outliers)
    .bind(mean))
# Either Ok(23.4) or Err("no data left after removing outliers")
```

</v-click>

---

# Why the Weird Name?

<v-clicks>

- Monads come from **category theory** — a branch of abstract maths
- Haskell borrowed the concept in the 1990s for handling side effects
- The *ideas* are simple; the *vocabulary* is intimidating
- Most people learn monads by using them, then realise what they were

</v-clicks>

<v-click>

<div class="mt-8 text-center text-xl" style="color: #d4639a">

You don't need the maths to use the pattern.

You just need a box and a chain.

</div>

</v-click>

---
layout: center
hideInToc: true
---

# A monad is a box you can put things in

<div class="mt-4 text-lg opacity-50">

and chain operations on, without worrying about the plumbing

</div>

<v-click>

<div class="mt-12 text-center" style="color: #d4639a">

Questions?

</div>

</v-click>

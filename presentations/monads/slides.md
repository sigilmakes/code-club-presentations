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

</div>

</v-click>

---

# What If We Had a Box?

Imagine a box that holds a value — but knows it might be empty.

<v-click>

```python
box = Maybe(42)       # there's a 42 in here
empty = Maybe(None)   # this one's empty
```

</v-click>

<v-click>

And imagine you could **chain** operations on it — and the box handles the "is it empty?" question *for you*.

</v-click>

<v-click>

```python
result = Maybe(42).bind(get_user).bind(get_address).bind(get_postcode)
# If any step comes back empty, the whole chain just gives you Nothing.
# No ifs. No crashes.
```

</v-click>

---

# You Already Use Boxes — They're Called Classes

```python
class Box:
    def __init__(self, value):
        self.value = value           # put a thing in the box

    def apply(self, func):
        return Box(func(self.value)) # do something to what's inside
```

<v-click>

```python
Box(5).apply(lambda x: x * 2)   # Box(10)
```

A class **wraps data** and gives you **methods** to work with it.

</v-click>

<v-click>

<div class="mt-4" style="color: #9b72cf">

A monad is the same idea — but the method between steps handles the messy stuff *for you*.

Where a class hides **state**, a monad hides **control flow**.

</div>

</v-click>

---

# How Does the Box Work?

Two rules:

<v-clicks>

1. **Put things in** — wrap a value in the box
   - `Maybe(42)` → a box with 42 inside

2. **Chain operations** — take a boxed value, apply a function, get a new box
   - If the box has something → unwrap, apply the function, get a new box back
   - If the box is empty → skip it, stay empty

</v-clicks>

<v-click>

```python
def bind(self, func):
    if self.value is None:
        return Maybe(None)    # empty? pass it along
    return func(self.value)   # otherwise, run the function
```

</v-click>

<v-click>

<div style="color: #d4639a">

That's it. That's a monad. A box with a way in and a way to chain.

</div>

</v-click>

---

# Different Boxes, Different Plumbing

The *pattern* is always the same. What changes is what the box handles for you.

<v-clicks>

| Box | What's Inside | What It Handles |
|-----|--------------|----------------|
| **Maybe / Optional** | A value that might be missing | Null checks |
| **Result** | A value or an error | Error propagation |
| **List** | Multiple values | Looping over combinations |
| **Promise** | A value that isn't here yet | Async scheduling |

</v-clicks>

<v-click>

<div class="mt-4" style="color: #9b72cf">

You write the logic. The box handles the plumbing between steps.

</div>

</v-click>

---

# You Already Use These

<v-click>

### JavaScript Promises

```javascript
fetch('/api/user')
  .then(res => res.json())       // chain
  .then(user => fetch(user.url)) // chain
  .then(res => res.json())       // chain
```

`.then()` is bind. Promise is the box. You've been using a monad.

</v-click>

<v-click>

### Python list comprehensions

```python
[(x, y) for x in [1, 2, 3] for y in ['a', 'b']]
# [(1,'a'), (1,'b'), (2,'a'), (2,'b'), (3,'a'), (3,'b')]
```

Nested iteration where each step depends on the last? That's list monad bind.

</v-click>

---

# A Python Example

<v-click>

```python
class Maybe:
    def __init__(self, value):
        self.value = value

    def bind(self, func):
        if self.value is None:
            return Maybe(None)
        return func(self.value)

    def __repr__(self):
        return f"Maybe({self.value})"
```

</v-click>

<v-click>

```python
def half(x):
    return Maybe(x // 2) if x % 2 == 0 else Maybe(None)

Maybe(16).bind(half).bind(half).bind(half)
# Maybe(2)

Maybe(16).bind(half).bind(half).bind(half).bind(half)
# Maybe(None) — 1 is odd, chain stops cleanly
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

# The Three Rules

What makes a box a *monad* and not just a box? Three rules:

<v-clicks>

1. **Wrapping then chaining does nothing extra** — if you put a value in a box and immediately chain a function, it's the same as just calling the function
   - *Putting something in a box shouldn't change it*

2. **Chaining with "just re-wrap it" does nothing** — if your chain function just puts the value back in a box, you get the same box
   - *The box shouldn't add anything you didn't ask for*

3. **Chaining is associative** — it doesn't matter how you group a sequence of chains, the result is the same
   - *Like how (1+2)+3 = 1+(2+3)*

</v-clicks>

<v-click>

<div class="mt-4" style="color: #d4639a">

These guarantee your chains are predictable. No weird surprises from ordering.

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

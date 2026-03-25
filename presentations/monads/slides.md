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

...I'm so sorry. let me start over.

</div>

</v-click>

---
layout: center
---

# A monad is a box you can put things in.

<v-click>

<div class="mt-8 text-lg opacity-70">

I promise. We can go from here.

</div>

</v-click>

---

# The Problem

So things go wrong when processing stuff. Values might be missing and you need to handle it. You might do this:

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


Every step needs a check. This is four levels of nesting for three functions.

This is horrible and I hate it and you should too.


</div>

</v-click>

---

# A Smarter Box: Maybe

What if we put everything in a box that just... handled it?

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

The box handles everything. We don't have to think about it. This is nice.

</div>

</v-click>

---

# More Monads! The Result Monad

So Maybe has a problem — when something goes wrong it just... goes empty. Doesn't tell you *why*. This is annoying. Result fixes this.

<v-click>

Two states:
- `Ok(value)` — it worked, here's the answer
- `Err(message)` — it failed, here's why

</v-click>

<v-click>

```python
def bind(self, func):
    if self.is_err:
        return self               # failed? carry the error forward
    return func(self.value)       # otherwise, keep going
```

</v-click>

<v-click>

<div style="color: #d4639a">

Same bind, same pattern — but now the error message comes along for the ride. No more silent failures.

</div>

</v-click>

---

# Putting It Together: A Data Pipeline

<v-click>

Without monads:

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

With Result:

```python
result = (Result.ok("readings.csv")
    .bind(read_csv)
    .bind(lambda d: parse_floats(d, "temperature"))
    .bind(remove_outliers)
    .bind(mean))
# Ok(23.4) or Err("column 'temperature' contains non-numeric values")
```

</v-click>

---

# The Same Trick, Different Box: Lists

Ok but what if the box has *multiple things in it*? Bind means: apply a function to each one, flatten the results.

<v-click>

```python
class List:
    def __init__(self, values):
        self.values = values

    def bind(self, func):
        result = []
        for item in self.values:
            result.extend(func(item))   # apply, then flatten
        return List(result)
```

</v-click>

<v-click>

```python
def expand(x):
    return List([x, x * 10])

List([1, 2, 3]).bind(expand)
# List([1, 10, 2, 20, 3, 30])
```

</v-click>

<v-click>

<div class="mt-4" style="color: #9b72cf">

Maybe handles missing values. List handles multiple values. Same interface, same idea. I know. I know. Stay with me.

</div>

</v-click>

---
layout: center
---

# YOU ALREADY DO THIS

```python
[(x, y) for x in [1, 2, 3] for y in ['a', 'b']]
# [(1,'a'), (1,'b'), (2,'a'), (2,'b'), (3,'a'), (3,'b')]
```

<v-click>

<div class="mt-8 text-xl">

Each <code>for</code> is a bind. That's the list monad. You've been using it this whole time.

</div>

</v-click>

---

# The Pattern

Right, so what do these have in common? Three ingredients:

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

<img src="./images/izutsumi.png" class="absolute bottom-8 right-12 h-32" />

</v-click>

---

# The Three Rules

Ok here's where it gets a bit formal. Sorry. What makes a box a *monad* and not just a box?

<v-clicks>

1. **Left identity:** `Maybe(a).bind(f)` = `f(a)`
   - Putting a value in a box and immediately using it should be the same as just using it. The box doesn't get in the way.

2. **Right identity:** `m.bind(Maybe)` = `m`
   - If all you do is re-wrap the value, nothing changes. The box doesn't add anything you didn't ask for.

3. **Associativity:** `m.bind(f).bind(g)` = `m.bind(lambda x: f(x).bind(g))`
   - It doesn't matter how you group a chain of operations — the result is the same. You can refactor freely.

</v-clicks>

---

# Composing Monads

What if you have multiple files and each one might fail?

<v-click>

```python
files = ["data1.csv", "data2.csv", "data3.csv"]

def process(filename):
    return (Result.ok(filename)
        .bind(read_csv)
        .bind(lambda d: parse_floats(d, "temperature"))
        .bind(mean))

results = List(files).bind(process)
# [Ok(23.4), Err("file not found: data2.csv"), Ok(19.1)]
```

</v-click>

<v-click>

List handles the iteration. Result handles the errors. Each one does its job.

</v-click>

<v-click>

<div class="mt-4" style="color: #d4639a">

You just compose them. They don't step on each other.

</div>

<img src="./images/nonon_tea.jpeg" class="absolute bottom-8 right-12 h-28 rounded" />

</v-click>

---

# Why the Weird Name?

<v-clicks>

- Monads come from **category theory** — a branch of abstract maths
- Haskell borrowed the concept in the 1990s for handling side effects
- The ideas are genuinely simple. The naming is genuinely terrible. This is a branding problem more than anything
- Most people learn monads by using them, then go "oh wait, *that's* what that was?"

</v-clicks>

<v-click>

<div class="mt-8 text-center text-xl" style="color: #d4639a">

You do not need category theory to use a monad.

You need a box and a chain. I promise.

</div>

</v-click>

---

# Monads vs Classes

<div class="grid grid-cols-2 gap-8 mt-4">

<div>

### A class gives you:
- A wrapper for data (`__init__`)
- Methods that operate on that data
- Encapsulation — hide the internals

```python
class User:
    def __init__(self, name):
        self.name = name
    def greet(self):
        return f"hi, {self.name}"
```

</div>

<div>

### A monad gives you:
- A wrapper for data (`wrap`)
- A chaining operation (`bind`)
- Encapsulation — hide the *control flow*

```python
class Maybe:
    def __init__(self, value):
        self.value = value
    def bind(self, func):
        if self.value is None:
            return Maybe(None)
        return func(self.value)
```

</div>

</div>

<v-click>

<div class="mt-4 text-center" style="color: #d4639a">

Classes hide **what's inside**. Monads hide **what happens between steps**.

</div>

</v-click>

---

# Why Not Just Use Classes?

You *can* solve these problems with classes. People do. It gets messy.

<v-click>

```python
class SafeCSVReader(BaseReader):           # inheritance
class ValidatedCSVReader(SafeCSVReader):   # more inheritance
class LoggedValidatedCSVReader(ValidatedCSVReader):  # oh no
```

Classes compose through **inheritance** — you stack behaviours by subclassing. This does not scale.

</v-click>

<v-click>

Monads compose through **chaining** — you stack behaviours by combining boxes.

```python
Result.ok("data.csv").bind(read_csv).bind(validate).bind(log)
```

Each box does one thing. You snap them together. No inheritance tree. No diamond problem. No `LoggedValidatedSafeCSVReaderFactory`.

</v-click>

<v-click>

<div class="mt-4" style="color: #d4639a">

Composition over inheritance. The box doesn't need to know about the other boxes.

</div>

</v-click>

---

# So About That Quote

Remember this?

> "A monad is just a monoid in the category of endofunctors"

<v-click>

A **functor** takes a function between values and lifts it to a function between boxes.

<div class="mt-4 flex justify-center">
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

An **endofunctor** is a functor from a category to itself — our boxes map Python types to Python types.

A **monoid** is something with an identity and an associative operation — that's our three rules.

</v-click>

<v-click>

<div class="mt-4" style="color: #d4639a">

So yeah. A monad is a monoid in the category of endofunctors. You know what that means now.


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

<div class="mt-4 text-sm opacity-40">

I told you it wasn't that bad

</div>

<v-click>

<div class="mt-12 text-center" style="color: #d4639a">

Questions?

</div>

</v-click>

# Boswell and Foucher 2012, Why we Sleep: The New Science of Sleeping and Dreams

---

## Table of Contents

* **[Preface](#preface)**
  * [1: Code should be easy to understand](#1-code-should-be-easy-to-understand)
* **Part One: Surface-Level Improvement**
  * 2: Packing information into names
  * 3: Names that can't be misinstructed
  * 4: Aesthetics
  * 5: Knowing what to comment
  * 6: Making comments precise and compact
* **Part Two: Simplifying Loops and Logic**
  * 7: Making control flow easy to read
  * 8: Breaking down giant expressions
  * 9: Variables and readability
* **Part Three: Reorganizing Your Code**
  * 10: Extracting unrelated sub-problems
  * 11: One task at a time
  * 12: Turning thoughts into code
  * 13: Writing less code
* **Part Four: Selected Topics**
  * 14: Testing and Readability
  * 15: Designing and implementing a "minute/hour counter

---

## Preface

* goal: "help you make your code better"

### What the books is about

* topics:
  * surface-level improvement
    * naming, commenting, aesthetics
  * simplifying loops and logic
    * refine the loops, logics, and variables
  * reorganizing your code
    * higher-level ways to organize large blocks of code, attack problem at the function level
  * selected topics
    * easy-to-understand testing, larger data structure coding example

### How to read this book

* fun, casual, easy
* ordered by difficulties: earlier topics are easier

## Using code examples

* you may use code example to make your job done, don't need to request permission
  * unless reproducing a significant portion of the code
* appreciate to use:
  * `The Art of Readable Code by Dustin Boswell and Trevor Foucher. Copyright 2012 Dustin Boswell and Trevor Foucher, 978-0-596-80229-5`

---

## 1. Code should be easy to understand

> Code should be easy to understand

### What makes code better?

> "better code? mostly depending on our gut feel and intuition...

* generally speaking,

example A:

```c
for (Node* node = list->head; node != NULL; code = node->next)
  Print(node->data);
```

**is better than**,

example B:

```c
Node* node = list->head
if (node == NULL) return;

while(node->next != NULL){
  Print(node->data)
  node = node->next
}
```

* example A and B do the same work.
* but it's not always easy to say which one is better

for example:

example A':

```c
return exponent >= 0 ? mantissa * (1 << exponent) : mantissa / (1 << -exponent));
```

example B':

```c
if (exponent >= 0) {
  return mantissa * (1 << exponent);
} else {
  return mantissa / (1 << -exponent);
}
```

> "How we decide which one is better?"

### The fundamental Theorem of Readability

> "Code should be written to minimize the time it would take for someone else to understand it

* even if you work alone
  * you'll never know you may join your project.
  * having that quote as a guide would work often nicely.

### Is Smaller Always Better?

* generally speaking, yes
  * see -> [13: Writing less code](#13-writing-less-code)
* time to understand code:
  * 2000-line class
  * 5000-line class

> but, fewer lines isn't always better!

for instance. think about time to understand example A and B below.

example A:

```c
assert((!(bucket = FindBucket(key))) || !bucket->IsOccupied());
```

example B:

```c
bucket = FindBucket(key);
if (bucket != NULL) assert(!bucket->isOccupied());
```

* using comments means adding lines
  * but it helps your understanding

example C:

```c
// Fast version of "hash = (65999 * hash) + c"
hash == (hash << 6) + (hash << 16) - hash + c;
```

### Does Time-Til-Understanding Conflict with Other Goals?

* other constrains:
  * making code efficient
  * well-architectured
  * easy to test
  * ...

> if we only aim for easy-to-understand codes, doesn't it conflict to them?

* rest of this book discusses:
  * how to apply "easy to read" to different situation
  * but basically, readability is the most important

### The Hard Part

* before we write some code
  * if we want to think about how "others" could read the code
  * it just takes more time

but, if we encourage ourselves to have "easy to read" programming convention, we can program better codes.

---

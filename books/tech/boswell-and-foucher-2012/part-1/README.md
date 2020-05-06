# Part One: Surface-Level Improvement

Surface-level improves are:

* picking good names
* writing good comments
* formatting codes neatly

and those are easy to apply.

* no refactor needed
* no logic change needed
* no huge time investment needed

These topics are very important:

> They affect every line of code in your codebase

* looks small
  * but in aggregate, huge improvement of your codebase

expected results are:

* great names
* well-written comments
* clean use of whitespace

these makes your code **easier to read**.

---

## 2. Packing Information into Names

a lot of same principle can be applied when you name:

* variables
* functions
* classes
* etc.

> think about a name as tiny comment

KEY IDEA: **Pack information into your names**

many names are vague (e.g. `tmp`) and some looks reasonable but don't pack much information (e.g. `size`, `get`)

* This chapter's six topics:
  * choosing specific words
  * avoiding generic names (or knowing when to use them)
  * using concrete names instead of abstract names
  * attaching extra information to a name, by using a suffix or prefix
  * deciding how long a name should be
  * using name formatting to pack extra information

### 2.1. Choose Specific Words

* "packing information into names"
  * choosing words that are very _specific_ and avoiding _empty_ words

```python
def GetPage(url):
  ...
```

* the word `Get` does not say much
  * get a page from
    * a local cache
    * from a database?
    * from the Internet?

* if it's "from the Internet"
  * more specific names:
    * `FetchPage()`
    * `DownloadPage()`

Another example of a `BinaryTree` class:

```cpp
class BinaryTree {
  int Size();
  ...
}
```

* what would `Size()` return:
  * height of the tree?
  * number of nodes?
  * memory footfrint of the tree?

* Problem: _"`Size`, the word, does not convey much information"_
  * more specific names:
    * `Height()`
    * `NumNodes()`
    * `MemoryBytes()`

The other example, `Thread` class:

```cpp
class Thread {
  void Stop();
  ...
}
```

* the name `Stop()` is **okay**, but might be more specific names:
  * `Kill()`
    * if it's heavyweight operation that can't be undone
  * `Pause()`
    * if we have `Resume()`

#### 2.1.1. Finding More "Colourful" Words

| Word | Alternatives |
| :---: | :---: |
| send | deliver, dispatch, distribute, route |
| find | search, extract, locate, recover |
| start | launch, create, begin, open |
| make | create, set up, generate, compose, add, new |

* example in `PHP` , there is a function to `explode()` a string
  * it's a colourful name
  * paints a good picture of breaking something into pieces
  * **BUT, how is it any different from**
    * `split()`
  * they (`explode`, `split`) are different, but it's hard to guess the differences based on the names

KEY IDEA: **It's better to be clear and precise than to be cute.**

### 2.2. Avoid Generic Names Like `tmp` and `retval`

* `tmp`, `retval`, `foo`, those are:
  * "I can't think of a name"

instead of using them, **"Pick a name that describes the entity's value or purpose"**.

example of `JavaScript` function using `retval`

```js
var euclidean norm = function (v) {
  var retval = 0.0;
  for (var i = 0; i < v.length; i += 1)
    retval += v[i] * v[i]
  return Math.sqrt(retval);
};
```

It's tempting to use `retval`, kind of names, when you don't have any idea about naming. However, please think about how much information `retval` to give you. You may only get: "I am returning a value" which is VERY obvious.

> A better name would describe the purpose of the variable or the value it contains.

* going back to the example:
  * the `var` is accumulating the sum of the squares of `v`
  * they can be:
    * `sum_squares`
  * this is:
    * announce the purpose of the variable upfront
    * might help to _debug in the future_

for helping debug with good name, this about this:

if the inside of loop were accidentally written as:

```js
    retval += v[i];
```

instead of:

```js
    retval += v[i] * v[i]
```

it's hard to debug, but with the name `sum_squares`:

```js
    sum_squares += v[i]; // Where's the "square" that we're summing? Bug!
```

ADVICE: **The name retval doesn't pack much information. Instead, use a name that describes the variable's value**

HOWEVER, there are some cases that "generic names do carry meanings".

#### 2.2.1. `tmp`

a classical case of swapping two variables:

```c
if (right < left) {
  tmp = right;
  right = left;
  left = tmp;
}
```

In this case, using `tmp` is perfectly fine: the "sole" purpose of `tmp` is a temporary storage of values. The temporary variable has no other duties.

BUT, here is another example that `tmp` is used by **laziness**:

```cpp
String tmp = user.name();
tmp += " " + user.phone_number();
tmp += " " + user.email();
...
template.set("user_info", tmp);
```

* even if the variable, `tmp`
  * has short life span
  * being temporary storage is not the ideal choice
* instead that laziness
  * `user_info` would be more descriptive

another case, `tmp` in a name, but "a part of it":

```cpp
tmp_file = tempfile.NamedTemporaryFile()
...
SaveData(tmp_file, ...)
```

example above named the variable as `tmp_file`, not just `tmp` due to the file object. Imagine we named it as `tmp` and it looks like:

```cpp
SaveData(tmp, ...)
```

* when we look the line of code, it's not clear that `tmp` is:
  * a file?
  * a file name?
  * data that being written?

ADVICE: **The name `tmp` should be used only in cases when being short-lived and temporary is the most important fact about that variable.**

#### 2.2.2. Loop Iterators

* commonly used names as indices and loop iteration like:
  * `i`
  * `j`
  * `iter`
  * `it`
* even if these names are generic, it can be understood to mean "I am an iterator"
  * but, if you use it as _another purpose_, it's confusing
  * thus, PLEASE DON'T DO THAT

sometimes there are better iterator names than `i`, `j`, and `k`.

```cpp
for (int i = 0; i < clubs.Size(); i++)
  for (int j = 0; j < clubs[i].members.size(); j++)
    for (int k = 0; k < users.Size(); k++)
      if (clubs[i].members[k] == users[j])
        cout << "user[" << j << "] is in club[" << i << "]" << endl;
```

in the `if` statement, `members[]` and `users[]` are using the wrong index. Bugs like this are hard to find.

> This is because the line seems fine in isolation

```cpp
      if (clubs[i].members[k] == users[j])
```

for this, using precise names would be helpful.

* instead of using `i`, `j`, `k`,
  * `club_i`, `members_i`, `users_i`
  * `ci`, `mi`, `ui`
    * can help the bug stand out more:

```cpp
      if (clubs[ci].members[ui] == users[mi]) // Bug! First letters don't match up
```

correct usage is more noticeable for right index usages:

```cpp
      if (clubs[ci].members[mi] == users[ui])
```

for this, using precise names would be helpful.

* instead of using `i`, `j`, `k`,
  * `club_i`, `members_i`, `users_i`
  * `ci`, `mi`, `ui`
    * can help the bug stand out more:

```cpp
      if (clubs[ci].members[ui] == users[mi]) // Bug! First letters don't match up
```

correct usage is more noticeable for right index usages:

#### 2.2.3. The Verdict on Generic Names

There are some case where generic names are useful.

ADVICE: **If you're going to use a generic name like `tmp`, or `retval`, have a good reason for doing so.**

* Often, they're "overused" out of pure laziness.
* _This is understandable when nothing better comes to mind_.
  * maybe picking lazy name and carry on is quicker and faster to develop
* BUT, if you get a "habit" to spend just extra FEW SECONDS to think better names
  * > You'll find your "naming muscle" builds quickly!

### 2.3. Prefer Concrete Names over Abstract Names

### 2.4. Attaching Extra Information to a Name

### 2.5. How Long Should a Name Be?

### 2.6. Use Name Formatting to Convey Meaning

### 2.7. Summary

---

## 3. Names that Can't Be Misconstrued

### 3.1. Example: Filter()

### 3.2. Example: Clip(text, length)

### 3.3. Prefer `min` and `max` for (Inclusive) Limits

### 3.4. Prefer `first` and `last` for Inclusive Ranges

### 3.5. Prefer `begin` and `end` for Inclusive/Exclusive Ranges

### 3.6. Naming Booleans

### 3.7. Matching Expectations of Users

### 3.8. Example: Evaluating Multiple Name Candidates

### 3.9 Summary

---

## 4. Aesthetics

### 4.1. Why Do Aesthetics Matter?

### 4.2. Rearrange Line Breaks to Be Consistent and Compact

### 4.3. Use Methods to Clean Up Irregularity

### 4.4. Use Column Alignment When Helpful

### 4.5. Pick a Meaningful Order, and Use it Consistently

### 4.6. Organise Declarations into Blocks

### 4.7. Break Code into "Paragraphs"

### 4.8. Personal Style versus Consistency

### 4.9. Summary

---

## 5. Knowing What to Comment

### 5.1. What NOT to Comment

### 5.2. Recording Your Thoughts

### 5.3. Put Yourself in the Reader's Shoes

### 5.4. Final Thoughts - Getting Over Writer's Block

### 5.5. Summary

---

## 6. Making Comments Precise and Compact

### 6.1. Keep Comments Compact

### 6.2. Avoid Ambiguous Pronouns

### 6.3. POlish Sloppy Sentences

### 6.4. Describe Function Behaviour Precisely

### 6.5. Use Input/Output Examples That Illustrate Corner Cases

### 6.6. State the Intent of Your Code

### 6.7. "Named Function Parameter" Comments

### 6.8. Use Information-Dense Words

### 6.9. Summary

---

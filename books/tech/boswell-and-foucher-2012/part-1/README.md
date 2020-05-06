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
| --- | --- |
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

* naming for: variables, functions, elements, etc.
  * describe it concretely rather than abstractly

* e.g. an internal method `ServerCanStart()`
  * with test whether the server can listen on a given TCP/IP port
    * the name, `ServerCanStart()` is somewhat abstract
    * more concrete name:
      * `CanListenOnPort()`
        * which directly describe what it does

more details about this concept will be explain in the following subchapters.

#### 2.3.1. Example: `DISALLOW_EVIL_CONSTRUCTORS`

* an example from the codebase at Google
  * in C++
    * if you don't define a copy constructor
    * or assignment operator for your class
  * a default is provided.

even if this can be handy, can easily have some problems:

* memory leaks
* other mishaps

because they are executed with "behind the scenes" in place you might not have realised.

Thus, Google as a **convention** to disallow these "evil" constructors by employing a macro:

```cpp
class ClassName {
  private:
    DISALLOW_EVIL_CONSTRUCTORS(ClassName);

    public:
    ...
};
```

this macro was defined as:

```cpp
#define DISALLOW_EVIL_CONSTRUCTORS(ClassName) \
  ClassName(const ClassName&); \
  void operator=(const ClassName&);
```

* by placing it into `private:` section in the class
  * these two methods become private
  * so, they cannot be (accidentally) used

The name `DISALLOW_EVIL_CONSTRUCTORS` could not very good, though.

* the world `evil`
  * strong stance on a debatable issue
  * isn't clear what the macro disallowing
    * it disallows the `operator=()` method, and that's isn't even a "constructor"

It's been used for many years, but replaced with less provocative and more concrete:

```cpp
#define DISALLOW_COPY_AND_ASSIGN(ClassName)
```

#### 2.3.2. Example: `--run_locally`

* an optional command-line flag named: `--run_locally`
  * to print extra debugging info but run slowly

the flag "was" typically used when testing on a local machine, but when it runs on the _remote server_, performance is integral, thus this flag was not used.

here are some issues:

* a new member of the team did not know what it did, he/she would use it when running locally, but did knot know "why" it was needed
* sometimes, we need to print debugging info when we run the code at remote server, but passing `--run_locally` to the remote server sounds a paradox, also very confusing. (running locally, remotely?)
* sometimes, we need to run a performance test locally and did not want to the logging slowing it down, ergo, we would not use `--run_locally`

SOLUTION: `--extra_logging`, _more direct and explicit_

However, what if `--run_locally` needs to do more than just "extra logging"?

* an example:
  * imagine it needs to set up and use a special local database
  * the name `--run_locally` seems better than `--exra_logging`
    * since `--run_locally` can control both (setting up, use special local DB) at once
  * BUT
    * still, using `--run_locally` for that new purpose is not ideal.
    * **it's vague and indirect**
      * better to create a NEW FLAG:
        * `--use_local_database`
      * even if the number of flags increased
        * it's more explicit
        * _do NOT try to smash two orthogonal ideas into one_
        * also it gives you to have several options like: use one flag but not the other flag

### 2.4. Attaching Extra Information to a Name

* consider variable names like a tiny comment
* there are something very important about a variable,
  * you think the reader must know,
  * it's better to attaching an extra "word" to the name

* e.g. a variable that contained a hexadecimal string:

```cpp
string id; // Example: "af84ef89fkez"
```

you may want to name as `hex_id` instead, if it's important for the reader(s) to remember the format of the ID.

#### 2.4.1. Values with Units

* if a variable is measurement (e.g. amount of time, number of bytes)
  * it's helpful to encode the units into the variable's name

* for example, JavaScript code that measures the load time of a web page:

```js
var start = (new Date()).getTime();           // top of the page
...
var elapsed = (new Date()).getTime() - start; // bottom of the page
document.writeln("LOad time was:" + elapsed + " seconds");
```

it's totally okay code, but it does not work since `getTime()` returns **milliseconds**, not _seconds_

by using `_ms` as postfix makes it more explicit:

```js
var start_ms = (new Date()).getTime();                // top of the page
...
var elapsed_ms = (new Date()).getTime() - start_ms;   // bottom of the page
document.writeln("LOad time was:" + elapsed_ms / 1000 + " seconds");
```

besides times, many units we are using for programming:

| Function Parameter | Renaming Parameter to Encode Units |
| --- | --- |
| `Start(int delay)` | `delay` -> `delay_secs` |
| `CreateCache(int size)` | `size` -> `size_mb` |
| `ThrottleDownload(float limit`) | `limit` -> `max_kbps` |
| `Rotate(float angle)` | `angle` -> `degrees_cw` |

#### 2.4.2. Encoding Other Important Attributes

* technique of attaching extra info. to a name is NOT limited only values with units
* should use this techniques anytime for something can be dangerous/surprising about the variables

* for example, many security exploits come from not realising that some of the data which your program receives is not yet in a safe status, you may want to use name like:
  * `untrustedUrl` or `untrustedURL`
  * `unsafeMessageBody`
* after calling functions that cleanse the "unsafe" inputs
  * results might be trusted:
    * `trustedUrl` or `trustedURL`
    * `safeMessageBody`

other examples:

| Situation | Variable Name | Better Name |
| --- | --- | --- |
| A password is in "plaintext" and should be encrypted before further processing | `password` | `plaintext_password` |
| A user-provided comment that needs escaping before being displayed | `comment` | `unescaped_comment` |
| Bytes of html have been converted to UTF-8 | `html` | `html_utf8` |
| Incoming data has been "url encoded" | `data` | `data_urlenc` |

* be aware that you should NOT use `unescaped_` or `_utf8` for all names
  * it's used for very important place and also need to help future debugging
  * also to avoid someone's mistake
  * some places that has huge security bug concern
    * very critical things, variables, put additional info on the name

---

* **IS THIS HUNGARIAN NOTATION?**
  * the notation is a system of naming widely used by Microsoft
    * it encodes the "type" of every variable into name's prefix

| Name | Meaning |
| --- | --- |
| `pLast` | A pointer (p) to the last element in some data structure |
| `pszBuffer` | A pointer (p) to a zero-terminated (z) string (s) buffer |
| `cch` | A count (c) of characters (ch) |
| `mpcopx` | A map (m) from a pointer to a colour (pco) to a pointer to an x-axis length (px) |

* It is an example of "attaching attributes to names"
  * but, it's a more formal and strict system
    * focused on encoding a specific set of attributes

what we have talked in this section is a broader, more informal system by following this practice:

* identifying any crucial attributes of a variable
* encode them legibly

---

### 2.5. How Long Should a Name Be?

> When picking a good name, there's an implicit constraint that the name shouldn't be too long.

for example, no one likes to see this:

```java
newNavigationControllerWrappingViewControllerForDataSourceOfClass
```

* longer name, harder to remember
* more space consume on the screen, causing extra lines to wrap

if we avoid this too much, then

* using only single-word (or single letter)
  * may not be good depending on languages
    * although it's quite commonly used in Golang Community!

anyways, how's handle the tradeoffs?

* `d`
* `days`
* `days_since_last_update` or `daysSinceLastUpdate`

#### 2.5.1. Shorter Names Are Okay for Shorter Scope

* for a short vacation, we pack less luggage than long vacation's one
  * identifier for small "scope"
    * how many LOC(lines of code) you see with the name
  * we don't need to carry much info. (on the name)

you should use shorter names when all info., such as:

* what type the variable is
* its initial value
* how it's destroyed

is easy to see, like example below:

```cpp
if (debug) {
  map<string, int> m;
  LookUpNamesNumbers(&m);
  Print(m);
}
```

even if `m` does not pack any info., it's not an issue, since the reader already get enough information to understand the code.

However, let's think what if `m` were a:

* _class member_
* or _global variable_

```cpp
  LookUpNamesNumbers(&m);
  Print(m);
```

since the "scope" becomes larger, it's less readable. Therefore, if an identifier has a large scope, the name needs to carry enough info. to make it clear.

#### 2.5.2. Typing Long Names - Not a Problem Anymore

> they are harder to type

is not a good reason to avoid longer names, although there are still a lot of valid reasons why longer names are not good.

Now, **WORD COMPLETION** is very common, and actually quite a lot of people are not aware of it.

You can simply write a bit longer name with:

1. type the first few characters of the name
2. trigger the word-completion command
3. if the completed word is not correct, keep triggering the command unitl the correct name appears

| Editor | Command |
| --- | --- |
| Vi | Ctrl-p |
| Emacs | Meta-/ (hit ESC, then /) |
| Eclipse | Alt-/ |
| Intellij IDEA | Alt-/ |
| TestMate | ESC |

#### 2.5.3. Acronyms and Abbreviations

* we sometimes use acronyms and abbreviations to make name small
* e.g. naming a class `BEManager` rather than `BackEndManager`

> Is this shrinkage worth the potential confusion?

* **Project-specific abbrevations are usually a bad idea.**
  * they appear cryptic and intimidating for new people for the project

Think about: **would a new teammate understand what the name means?**. if yes, then it would be okay.

For instance, it's fair enough to use for programmers:

* `eval` than `evaluation`
* `doc` than `documentation`
* `str` than `string`

Thus, when the new member see `FormatStr()`, they probably easily catch its meaning, however what about `BEManager()`?

#### 2.5.4. Throwing Out Unneeded Words

* sometimes, we can throw some words from names without losing any info.

| Before | After |
| --- | --- |
| `ConvertToString()` | `ToString()` |
| `DoServeLoop()` | `ServeLoop()` |

### 2.6. Use Name Formatting to Convey Meaning

* use of:
  * underscores
  * dashes
  * capitalisation
* can also pack more info. in a name

For example, C++ Style Convention used for Google for open source projects:

```cpp
static const int kMaxOpenFiles = 100;

class LogReader {
  public:
    void OpenFile(string local_file);

  private:
    int offset_;
    DISALLOW_COPY_AND_ASSIGN(LogReader);
};
```

* having different formats for different entities is like a form of syntax highlighting
  * help you to read code more easily

* They are pretty common and known as:
  * CamelCase (strictly speaking, this can be PascalCase, camelCaseLooksLikeThis)
    * for class names
  * snake_case (lower_separated)
    * for variable names

but, there are also some other conventions that maybe surprising:

* constant value
  * `kConstantName` than `CONST_NAME`
    * this can be benefit because we can easily differentiate const value with `#define` "macros" (e.g. `MACRO_NAME` by the convention)
* class member variables
  * `offset_`
    * it looks weird, but being able to instantly distinguish members from other variables is handy
    * for example, when we are looking very large class and encounter this line of code:
      * `stats.Clear()`
      * we may wonder:
        * does `stats` belong to this class so that it changes internal state of this class?
        * with `member_` convention, we don't need to worry about it and we can simply answer as:
          * no (because it's not `stats_`, but `stats`)
      * in Go, we can use PascalCase for non-local variables whereas we can still use camelCase for local variables)

#### 2.6.1. Other Formatting Conventions

* depending on your project/language
  * there might be different style guides that you can use to make names contain more information

* e.g. in, `JavaScript: The Good Parts (Douglas Crockford, O'Reilly, 2008)`, the author suggest:
  * "constructors" (functions intended to be called with `new`) should be capitalised
    * other ordinary functions should start with a lowercase letter

```js
var x = new DatePicker(); // DatePicker() is a "constructor" function
var y = pageHeight();     // PageHeight() is an ordinary function
```

another JS example: when calling the jQuery library function (name is the single character `$`, a useful convention is to prefix jQuery results with `$` as well:

```js
var $all_images = $("img"); // $all_images is a jQuery object
var height = 250;           // height is not
```

final example: HTML/CSS, when giving an HTML/CSS tag an `id` or `class` attribute, both underscores and dashes are valid characters to use in the value

* possible convention:
  * use underscores to separate words in IDs
  * use dashes to separate words in classes

```html
<div id="middle_column" class="main-content"> ...
```

* what sort of conventions we use?
  * up to you or your team
  * whatever style guide you use
    * **be CONSISTENT** across your project(s)

### 2.7. Summary

* Theme: **pack information into your names**
  * readers can extract a lot of info. just by reading the names

| Tip | Example |
| --- | --- |
| **Use specific words** | instead of `Get`, words like `Fetch` or `Download` might be better, depending on context |
| **Avoid generic names** | like `tmp` and `retval`, unless there's a specific reason to use them |
| **Use concrete names** | that describe things in more detail, the name `ServerCanStart()` is vague compared to `CanListenOnPort()`|
| **Attach important details** | to variable names. appending `_ms` to a variable whose value is in milliseconds or prepend `raw_` to an unprocessed variable that needs escaping |
| **Use longer names for larger scopes** | don't use cryptic one or two letter names for variables that span multiple screens; shorter names are better for variables that span only a few lines |
| **Use capitalisation, underscores, and so on in a meaningful way** | you can append `_` to class members to distinguish them from local variables |

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

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

Watching out names that can be misunderstood!

KEY IDEA: **Actively scrutinise your names by asking yourself, "What other meanings could someone interpret from this name?"

* actively seeking "wrong interpretations"
  * help you spot ambiguous names

### 3.1. Example: Filter()

* imagine you are writing code to manipulate a set of database (DB) results

```py
results = Database.all_objects.filter("year <= 2011")
```

what's the result?

* object whose year is <= 2011?
* object whose year is NOT <= 2011?

this is because...

* problem: `filter` is an ambiguous word
  * unclear for
    * to pick out
    * to get rid of
  * thus, it's better not to use words like `filter`
    * since they are easily misconstrued

* better names for
  * to pick out
    * `select()`
  * to get rid of
    * `exclude()`

### 3.2. Example: Clip(text, length)

imagine this:

```py
# Cuts off the end of the text, and appends "..."
def Clip(text, length):
  ...
```

* two ways to guess how that function works:
  * it removes `length` from the end
  * it truncates to a maximum `length` (most likely)

you never know which one was original intent.

* rather than leave your reader which any nagging doubt,
  * it's better to name function `Truncate(text, length)`

Furthermore, the name of parameter, `length`, can be also problem. `max_length` can be clearer.

BUT, `max_length` is still arguable with potential multiple interpretation:

* a number of
  * bytes
  * characters
  * words

as we discussed about "attaching unit to names" we can improve this if was meant as: "number of characters"

```py
max_char
```

### 3.3. Prefer `min` and `max` for (Inclusive) Limits

imagine your shopping cart is limited to buy less than 10 items at once:

```py
CART_TOO_BIG_LIMIT = 10

if shopping_cart.num_items() >= CART_TOO_BIG_LIMIT:
  Error("Too many items in cart.")
```

one thing: classic _off-by-one_ bug, we should replace `>=` with `>` (or maybe change the limit as `11`, not `10`):

```py
if shopping_cart.num_items() > CART_TOO_BIG_LIMIT:
```

but, regardless the bug, `CART_TOO_BIG_LIMIT` is an ambiguous name. Did you mean:

* up to?
* up to and including?

ADVICE: **The clearest way to name a limit is to put `max_` or `min_` in front of the thing being limited.**

thus, we can change the name as `MAX_ITEMS_IN_CART` and refactored code looks better:

```py
MAX_ITEMS_IN_CART = 10

if shopping_cart.num_items() > MAX_ITEMS_IN_CART:
  Error("Too many items in cart.")
```

### 3.4. Prefer `first` and `last` for Inclusive Ranges

```image
first        last
  |           |
  v           v
 --- --- --- --- ---
| a | b | c | d |   |
 --- --- --- --- ---
```

an example where you cannot tell if it's "up to" or "up to and including":

```py
print integer_range(start=2, stop=4)
# Does this print [2,3] or [2,3,4] (or something else)?
```

even if `start` is a reasonable parameter name, `stop` can be understood in multiple ways.

for _inclusive range_, where the range should include both end points, a good choice is `first/last`:

```py
set.PrintKeys(first="Bart", last="Maggie")
```

unlike `stop`, the word `last` is clearly inclusive. Also the names `min/max` also work for inclusive ranges depending on the context (if they sounds right).

### 3.5. Prefer `begin` and `end` for Inclusive/Exclusive Ranges

```image
 --- --- --- --- ---
| a | b | c | d |   |
 --- --- --- --- ---
  ^               ^
  |               |
begin            end
```

It's often more convenient to use inclusive/exclusive ranges.

* e.g. if you want to print all the events that happened on the 16th of October
* it's easier to write:

```py
PrintEventsInRange("OCT 16 12:00am", "OCT 17 12:00am")
```

than

```py
PrintEventsInRange("OCT 16 12:00am", "OCT 16 11:59:59.9999pm")
```

* what's a good pair of those params?
  * a typical programming convention: `begin/end`
    * however, `end` is a bit ambiguous
      * e.g.
        * "I am at the end of book.", the `end` is inclusive
        * Unfortunately, English does not have a succinct word for _"just past the last value"_
    * since `begin/end` is so idiomatic
      * e.g. it's used in C++'s standard libraries
      * e.g. most of places where an array needs to be "sliced" in this way
      * it's the best option for now

### 3.6. Naming Booleans

picking a name for a boolean variables or a function that returns a boolean, make sure the `true` or `false` means clear.

* a dangerous example:

```c
bool read_password = true;
```

* two ways to understand above:
  * we need to read the password
  * the password has already been read

in this case, it's best to avoid word `read`, then name it as:

* `need_password`
* `user_is_authenticated`

generally, adding words like:

* `is`
* `has`
* `can`
* `should`

makes booleans more clear.

another example with a function named as `SpaceLeft()` should like:

* it might return a number

if this meant to be returning a boolean value, better name:

```c
HasSpaceLeft()
```

last example, it's best to avoid _negated_ terms in a name:

instead of:

```c
bool disable_ssl = false;
```

better code:

```c
bool use_ssl = true;
```

* easier to read
* more compact to mention

### 3.7. Matching Expectations of Users

some names can be misread due to user may have a preconceived idea of what the name means, even if you meant something else. with this case, it's best to just "give in" and change the name so that it's not misleading:

#### 3.7.1. `get*()`

* many programmers are used to the convention that methods starting with `get` are lightweight accessors
  * which simply return an internal member
  * going against this convention is likely to mislead those users
    * an example of Java what NOT to do:

```java
public class StatisticsCollector {
  public void addSample(double x)

  public double getMean() {
    // Iterate through all samples and return total / num_samples
  }
}
```

* in this case
  * the implementation of `getMean()
    * to iterate over past data
    * and calculate the mean on the fly
  * this step might be very expensive with a LOT of data
    * but some unsuspecting programmers may call `getMean()` carelessly because they may assume the function is an inexpensive call
* we can rename this:
  * `computeMean()`
    * this tells people it could be an expensive operation
      * or we can refactor to make it as more lightweight operation

#### 3.7.2. `list::size()`

an example in C++ standard library, which was a cause of a very-difficult-to-find bug that made one of our servers slow down to a crawl:

```cpp
void ShrinkList(list<Node>& list, int max_size) {
  while (list.size() > max_size) {
    FreeNode(list.back());
    list.pop_back();
  }
}
```

* the "bug": author did not know:
  * `list.size()` is an _O(n) operation_
    * therefore, this counts through the linked list node by node rather than using/returning a precalculated count
    * this makes `ShrinkList()` call as _O(n^2) operation_

let's think about this:

* the code is technically "correct"
* passed unit test

However, when `ShrinkList()` was called on a list with a million elements, it took almost a lot of time... this can be an hour, hours, days or forever!

You may think in this way:

* this is the caller's fault
  * they should have read the documentation more carefully

this is true but, THE FACT IS:

> list.size() isn't a constant-time operation is surprising

instead of the name `size()` those might give us more attention about operation cost:

* `countSize()`
* `countElements()`

the C++ standard library probably wanted to name the method `size()` to match all other containers like `vector` and `map`, however because if the name, programmers can be easily confused and they could consider it as a fast operator, the way it's for other containers.

Fortunately, the lastest C++ standard library mandates `size()` to be _O(1) operation_.

---

WHO'S THE WIZARD

* a while ago, one of the authors was installing OpenBSD OS
* during the disk formatting step, a complicated menu popped up, asking for disk parameters
  * one option: to go to "Wizard mode"
    * he was relieved to find this user-friendly option and select it
    * to his dismay, it dropped the installer into a low-level prompt waiting for manual disk formatting command with no clear way to get out of it
  * the "wizard" meant
    * you were the wizard
    * not the mode is wizard

---

### 3.8. Example: Evaluating Multiple Name Candidates

* deciding a good name, you may have multiple candidates
* better to debate which one is the best in your head before settling with the final one

here is an illustration of the critiquing process:

* high-traffic websites often use "experiments" to test
  * whether a change to the website improves business
  * an example of a config file that controls some experiments:

```config
experiment_id: 100
description: "increase font size to 14pt"
traffic_fraction: 5%
...
```

* each experiment is defined by about 15 attributes/value pairs
* unfortunately, when defining another experiment that's very similar: you need to copy and paste the previous config and change some:

```config
experiment_id: 101
description: "increase font size to 13pt"
[other lines identical to experiment_id 100]
...
```

imagine we want to fix this situation by introducing a new way to have on experiment reuse the properties from other (this is the "prototype inheritance" pattern).

the end result is that you would type like:

```config
experiment_id: 101
the_other_experiment_id_I_want_to_reuse: 100
[change any properties as needed]
```

* next question: should `the_other_experiment_id_I_want_to_reuse` really be named?
  * better considerations:
    * `template`
    * `reuse`
    * `copy`
    * `inherit`
  * any of those four makes sense to us since
    * we're the one adding this new feature to the config language

BUT, we may also consider this:
* how the name will sound to someone who comes across the code and does not know about this feature?

let's analyse each name

Option ONE: using the name `template`

```config
experiment_id: 101
template: 100
...
```

* problems:
  * unclear whether it's saying "I am a template." or "I am using this other template."
  * "template" is often something abstract
    * that must be "filled in" before it is _concrete_
    * some might think a templated experiment isn't a "real" experiment
  * thus, `template` is just too vague in this situation

Option TWO: using the name `reuse`

```config
experiment_id: 101
reuse: 100
...
```

* okay, but
  * as written some might consider "this experiment can be reused at most 100 times."
  * rename it as `reuse_id` may resolve the problem above
    * but the confused reader might think `reuse_id: 100` as
      * "my id for reuse is 100"

Option THREE: using the name `copy`

```config
experiment_id: 101
template: 100
...
```

* a good word, but by itself, `copy: 100` seems like it's saying "copy this experiment 100 times" or "this is the 100th copy of something"
  * to make it clear that this term refers to _another_ experiment, we can rename it as `copy_experiment`
    * it maybe the best name so far

Option FOUR: using the name `inherit`

```config
experiment_id: 101
template: 100
...
```

* familiar name for most of programmers
  * can be understood that further modifications are made after inheritance
    * with class inheritance, you get all the methods and members of another class
    * and then modify them or add more
  * in the real life, when you inherit possessions from a relative,
    * it's understood that you might sell them or own other things yourself
* let's make it clear that we are inheriting from another experiment
  * we can improve the name to
    * `inherit_from`
    * `inherit_from_experiment_id`

As result, `copy_experiment` or `inherit_from_experiment_id` can be good names since they most clearly describe what's happening and least likely to be misunderstood.

### 3.9 Summary

* the best names: ones that can't be misconstrued
  * the person reading your code will understand it the way you meant it without any other way
  * a lot of English words are ambiguous when it comes to programming
    * `filter`
    * `length`
    * `limit`
* before deciding names, play devil's advocate and imagine how your name might be misunderstood

> The best names are resistant to misinterpretation.

* upper/lower limit for a value
  * `max_` and `min_` are good prefixes
* inclusive ranges
  * `first` and `last` are good
* inclusive/exclusive ranges
  * `begin` and `end` are best (as they are the most idiomatic choice)

* naming booleans
  * use words like:
    * `is`
    * `has`
  * to make it clear that it's a boolean variable
  * avoid "negated" terms
    * `disable_ssl`
  
* be aware of users' expectation of certain words
  * user may expect get() or size() to be lightweight methods

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

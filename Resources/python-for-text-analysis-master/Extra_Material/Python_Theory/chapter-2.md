# Chapter 2 -- Control flow tools and files
**indentation, indentation, indentation...**

This chapter we will start working with files, and introduce for-loops, while-loops, and functions.
You will learn how to define your own functions, and use those functions to analyze
text. We will also discuss different file formats.

## For-loops
There are two kinds of loops: the for-loop and the while-loop.
In this paragraph, we will first introduce the for-loop, and we'll discuss while-loops in the next.

The for-loop is the most commonly used loop in Python. You'll find that, most of the
time, you just want to carry out some operation for all the items in a sequence.
And that's just what the for-loop does! It looks like this:

```python
for number in [1,2,3]:
    print(number)
```

This loop prints the numbers 1, 2, and 3, each on a new line. The variable name
`number` is just something I have chosen. It could have been anything, even something
like `sugar_bunny`. But `number` is nice and descriptive. OK, so how does the loop work?

1. The Python interpreter starts by checking whether there's anything to iterate over.
    If the list is empty, it just passes over the for-loop and does nothing.
2. Then, the first value in the iterable (in this case a list) gets assigned to the
variable `number`.
3. Following this, we enter a 'local context', indicated by the indentation (four spaces
    preceding the print function). This local context can be as big as you want. All Python
    cares about is those four spaces. Everything that is indented is part of the local context.
4. Then, Python carries out all the operations in the local context. In this case,
    this is just `print(number)`. Because `number` refers to the first element of the list,
    it prints `1`.
5. Once all operations in the local context have been carried out, the interpreter
    checks if there are any more elements in the list. If so, the next value (in this case `2`)
    gets assigned to the variable `number`.
6. Then, we move to step 3 again: enter the local context, carry out all the operations,
    and check if there's another element in the list, and so on, until there are no more
    elements left.

### Ranges of numbers

Sometimes, it is useful to do the same thing a number of times. To do this, you
can use the `range()` function:

```python
for i in range(10):
    print('I like repeating myself!')
```

This will print 'I like repeating myself' ten times. `range()` is a function that
returns a `range` object. Looping over that object will give you all the numbers in
a particular range, e.g. `range(5,9)` corresponds to [5,6,7,8], excluding 9.

(Note that `range` is not a list. It only keeps one number in memory at any given time,
    so it's really memory-efficient.)

### Enumerating sequences
Sometimes, it is also useful to have the index of an item as you iterate over a list.
For this purpose, there is another useful function called `enumerate()`. It works like this:

```python
for index, character in enumerate('Monty'):
    print('character with index', index, 'is', character)
```

The code above prints:

    character with index 0 is M
    character with index 1 is o
    character with index 2 is n
    character with index 3 is t
    character with index 4 is y

Note that we make use of **multiple assignment** here. Multiple assignment
is the practice of assigning values to multiple variables at once. The simplest example of
multiple assignment is this:

```python
>>> x,y = (1,2)
>>> print(x)
1
>>> print(y)
2
```

The `enumerate` example above is equivalent to this:

```python
for pair in enumerate('Monty'):
    index, character = pair
    print('character with index', index, 'is', character)
```

Or even this:

```python
for pair in enumerate('Monty'):
    index = pair[0]
    character = pair[1]
    print('character with index', index, 'is', character)
```

But both are longer and not as clear. We'll talk more about `enumerate` and multiple
assignment in the notebook.

## While-loops

While-loops are the oldest type of loops. Every programming language that has loops, has a while-loop. Here is an example:

```python
i = 0
while i < 10:
	print(i)
	i += 1
```

This is equivalent to:

```python
for i in range(10):
	print(i)
```

While-loops always start with the word `while`, followed by a **(boolean) condition**. You can read the first line of a while-loop as: "while the condition holds, perform all the operations in the local context." In this case, the only two operations are `print(i)` and `i += 1`. This incrementation of `i` ensures that the loop will eventually end, once `i` is equal to 10.

(Booleans are useful for `while`-loops, as well as `if... elif... else...`-statements. We'll cover them in the notebook.)

As you can see above, the for-loop is much shorter than the while-loop. This is usually the case when all you want to do is iterate over some collection of things. Since 'iterating over a collection of things' is usually enough for our purposes, we will mainly use the for-loop.

So when do you use `while`? Our rule-of-thumb is this:

> Use `while` if there is a clear condition for success (or failure), and you're not sure how long it will take to get to that point.

For example:

* If you're collecting data from webpage that doesnt always load: keep trying until it loads.
* If you're mining web pages for information, and you want to collect a certain amount of that information: keep looking until you have enough.
* If you're working with a Queue or a to-do list: keep working until there is nothing more to do. (You can even add stuff to the list during the loop.)

## If, elif, else

The `if... elif... else...`-statements provide a powerful way to structure your code. You can use them to run different bits of code depending on a particular set of conditions.

Here is an example:

```python
# Inter-rater reliability is a measure to assess annotation quality.
# Here is some code to interpret Cohen's Kappa.
# (according to Landis & Koch 1977)

if kappa == 0:
	print('Poor')
elif kappa <= 0.2:
	print('Slight')
elif kappa <= 0.4:
	print('Fair')
elif kappa <= 0.6:
	print('Moderate')
elif kappa <= 0.8:
	print('Substantial')
else:
	print('(Almost) perfect')
```

While `if... elif... else...`-statements are very useful, they do not always provide the optimal solution. Here is one example:

```python
# Beware if you do something like this:
if x == 1:
	y = 'a'
elif x == 2:
	y = 'b'
elif x == 3:
	y == 'c'
elif x == 4:
	y = 'd'

# Use a dictionary instead:
d = {1:'a', 2:'b', 3:'c', 4:'d'}
y = d[x]
```

The reason why the conditionals worked so well in the first example is that they cover a range of values. This is almost impossible to capture in a dictionary.

Most often, you'll probably use a single `if`-statement, or an `if` combined with an `else`. Long sequences of `if... elif... else...` like in the first example are not that common, but it's very useful to know!


## Functions

A function is really just a convenient way to re-use code. We've actually already
seen several kinds of functions. For example:

* The print function. All this function does is take some input (any object), and
    display that input on the screen.
* The `min()` and `max()` functions. These take a collection, and **return** either
    the smallest or the largest element of that collection.

The word 'return' is used when a function produces any output that can be used for
further computation. For example: `min([1,2,3])` returns `1`. But `print('hello')`
does not return anything. It just outputs text on your screen. When a function returns
output, you can assign that output to a variable, like so:

```python
x = min([1,2,3])
print(x) # This will print '1'.
```

When you try to do the same with `print('hello')`, you get a different result:

```python
x = print('hello')
print(x) # This will print None.
```

You could write a simple implementation of `min()` every time you wanted to get
the smallest number. For example:

```python
list_of_numbers = [2,3,1,4,5,6,2,4,0]
smallest = list_of_numbers[0]
for number in list_of_numbers:
    if number < smallest:
        smallest = number
# To show that this works:
print('smallest number is', smallest)
```

This would print `0`, which is the smallest number in the list. But if you wanted
to do this multiple times in your program, it would be a waste of time to write
the same piece of code multiple times. Functions are nothing but names for pieces
of code that are defined elsewhere. In the definition of `min()`, it is specified
that the **argument** of `min()` should correspond to `list_of_numbers`, and that
it should output `smallest` after determining which number is the smallest.

### Writing your own functions
Here is how you define a function:

* write `def`;
* the name you would like to call your function;
* a set of parentheses containing the argument(s) of your function;
* a colon;
* a **docstring** describing what your function does;
* the function definition;
* ending with a **return statement**.

Here's a re-definition of the `min()` function that's already provided with Python:

```python
def min_clone(list_of_numbers):
    """
    Function to determine which number is the smallest.
    The input is a list of number, and the output is a specific number.
    """
    smallest = list_of_numbers[0]
    for number in list_of_numbers:
        if number < smallest:
            smallest = number
    return smallest

# To show that this works:
lon = [2,3,1,4,5,6,2,4,0]
x = min(lon)
print('smallest number is', x)
```

Before you define a function, however, you should always check if that function hasn't been implemented already in Python's standard library. Also: ***never*** use a function name that has been defined already, even if you mean to replace Python's built-in functionality. (This makes your code more re-usable and future-proof.)

## Working with files

There are two ways to work with files. The preferred way is to use `with`-statement.
like this:

```python
# Open the file, call it 'f'
with open('some_file.txt') as f:
    # Get the text data.
    text = f.read()
```

Which is equivalent to:

```python
# Open the file.
f = open('some_file.txt')
# Get the text data.
text = f.read()
# ...Some time later, close the file.
f.close()
```

In both cases, you create a *file object* called `f` (the conventional name, but
you could call it `mickey_mouse` and it would still work). That object has a method
called `read()` that returns all the text in the file as one big string. This string
value gets assigned to the variable called `text`.

The main advantage of using the `with`-statement is that it automatically closes
the file once you leave the local context defined by the indentation level. If you
'manually' open and close the file, you risk forgetting to close the file.

The `open` function can be used in different *modes*. By default it opens files
in *read mode*, which means that Python can access the contents, but cannot modify
the file. You can make the mode explicit by adding an additional argument. For example,
you could use `open('some_file.txt', 'r')` to explicitly state that you want to open
the file in read mode. Here is a table with all the modes (copied from [here](https://docs.python.org/3.5/library/functions.html#open)):

| Character | Meaning |
| --------- | ------- |
|'r' |	open for reading (default)|
|'w' |	open for writing, truncating the file first|
|'x' |	open for exclusive creation, failing if the file already exists|
|'a' |	open for writing, appending to the end of the file if it exists|
|'b' |	binary mode|
|'t' |	text mode (default)|
|'+' |	open a disk file for updating (reading and writing)|
|'U' |	universal newlines mode (deprecated)|

**Common file operations** are:

* Skipping a line. For example if the first line of the document doesn't contain
  relevant information, use: `next(f)`.

* Going over a file line by line. Use a for-loop: `for line in f: ...`. This is
  strictly preferred over `f.readlines()`!

* Reading all text in a file: `f.read()`

* Writing a single line: `f.write(line)`

* Writing multiple lines: `f.writelines(list_of_lines)`. Note that lines have to
  end with a newline character (`\n`) or else there won't be any breaks!

We'll practice with these in the notebook.

**File names**:

You can put a lot of information inside a file name. Here's a small list of ideas
that may come in useful. Keep in mind how you might retrieve this information from
the file name once you've generated it. For example by separating all parts with an
underscore, so that you can 'dissect' the file name using the `str.split('_')` operation.

* The date and time. For now you can just assume we'll generate one file, and you
  can hard-code the date, using the YYYYMMDD format (this format is easiest to sort).
  Later you can use [the time module](https://docs.python.org/3.5/library/time.html#time.strftime)
  for this.
  
* The index/rank of the file in a sequence. You can either include the index in
  the file name directly, or better: pad the number with zeroes so that the file
  names are easier to sort and the numbers are nicely aligned: you can do this
  using the `rjust` command, like this: `'1'.rjust(5,'0')`.

* The extension. Your file doesn't have to end with '.txt' or '.csv'. You can use
  any extension you like! E.g. '.results' or '.log'.

### Plain text

Plain text is the most basic file format: it's just everyday characters that you're
used to, plus some special characters to add whitespace. You will mostly be using
 tab (`\t`) and newline (`\n`).

The only issue with plain text is that the characters maybe stored in some exotic
format. In that case you need to convert their encoding to unicode. You can use  
[the codecs module](https://docs.python.org/3.5/library/codecs.html) for this.

To learn more about unicode and character sets, see the following **readings**.
These two links are super relevant if you want to learn more about proper text
handling. Don't worry if you don't understand everything. We will discuss them in
class.

* [The absolute minimum every software developer absolutely, positively should know about unicode and character sets (no excuses)](http://www.joelonsoftware.com/articles/Unicode.html)
* [Ned Batchelder on 'pragmatic unicode'](http://nedbatchelder.com/text/unipain.html)

During the course, we will mostly just work with unicode, so encodings won't come
up very frequently.

### CSV and TSV files

CSV and TSV are one step up from plain text. These formats are used for data that
is structured in rows and columns (like a spreadsheet in Excel). Each line corresponds
to a row, and cells are either separated by commas (CSV) or tabs (TSV). These files
may start with a *header*: a row with labels for each column. We will use the `csv`
module to work with CSV and TSV files. See the notebook for exercises.

```python
# Open a TSV file for reading
# For CSV files you can leave out the delimiter argument.
with open('example.tsv') as f:
    reader = csv.reader(f, delimiter='\t')
    for row in reader:
        # Do stuff

# Open a TSV file for writing
with open('example.tsv','w') as f:
    writer = csv.writer(f, delimiter='\t')
    # If rows is an iterable containing rows, you can use writerows()
    # rows could be something like [[1,2,3],[4,5,6]]
    writer.writerows(rows)
```

### JSON files

JSON is one of the standard data formats for the web. [Wikipedia](https://en.wikipedia.org/wiki/JSON)
has a very good description of what JSON files are and what they look like. We will
use the `json` module to deal with them.

```python
# Load a json file as a dictionary (common use case)
with open('example.json') as f:
    d = json.load(f)

# Write out a dictionary as a JSON file. Note that not all objects can be written
# to a JSON file. E.g. sets are disallowed (you could cast them to lists instead).
with open('example.json','w') as f:
    json.dump(d,f)
```

### Making files both human- and machine-readable

There are no strict rules to make files both human- and machine-readable.
But there are some principles that you should follow:

* Structure your file.
* Use special characters to separate different parts. For example:
    * Use a colon for text fields ('Name: John')
    * Use dashes to separate entries ('--------------').
    * Use blank lines to separate different parts of an entry.
* Be consistent in your format.
* Think about usability: people make less mistakes if the format feels natural to use.

We'll work with several different examples in the notebook. One of these is the
*linguist list* data, which consists of messages sent to a mailing list. Messages
for each day are bundled together and sent to the subscribers of the mailing list.
The structure of each set of bundled messages is such that you can recover the main
properties of the individual messages. But at the same time, because the mailing
list is meant to be read by researchers, the editors took care to present everything
in a readable format.

### XML and HTML

Finally, we will talk about [XML](https://en.wikipedia.org/wiki/XML) and
[HTML](https://en.wikipedia.org/wiki/HTML) files. (Click on those links for an
explanation of these file types.) These formats are commonly used to store data,
or to present it on the web. We will use the `lxml` module to deal with the former,
and the `beautifulsoup` library to deal with the latter. See the notebook for examples.

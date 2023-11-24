# Python reference

## TOC

<!-- TOC -->
* [Python reference](#python-reference)
  * [TOC](#toc)
  * [General](#general)
  * [Cookiecutter - Project scaffolding](#cookiecutter---project-scaffolding)
  * [Package management](#package-management)
    * [Virtual environment (venv)](#virtual-environment--venv-)
    * [Requirements](#requirements)
  * [Style guide (linting, formatting)](#style-guide--linting-formatting-)
  * [Documentation - docstrings](#documentation---docstrings)
  * [Multi line code](#multi-line-code)
  * [Comments](#comments)
    * [Logical conditions (and, or, not...)](#logical-conditions--and-or-not-)
  * [Type hints](#type-hints)
  * [Variables](#variables)
  * [Builtin Data types](#builtin-data-types)
    * [None](#none)
    * [String](#string)
    * [List](#list)
    * [Tuple](#tuple)
    * [Dictionary](#dictionary)
    * [Deque](#deque)
    * [Set](#set)
    * [Decimal](#decimal)
  * [Imported data types](#imported-data-types)
    * [Dataframe](#dataframe)
    * [Numpy Array](#numpy-array)
    * [HDF5](#hdf5)
  * [Iterators](#iterators)
  * [Conditions (if, else...)](#conditions--if-else-)
  * [Loops](#loops)
    * [For](#for)
    * [While](#while)
  * [Functions](#functions)
  * [map, filter, reduce](#map-filter-reduce)
  * [Generators](#generators)
  * [Decorators](#decorators)
  * [Modules](#modules)
  * [Classes](#classes)
  * [Magic methods (dunder methods)](#magic-methods--dunder-methods-)
  * [Bitwise operations](#bitwise-operations)
  * [File I/O](#file-io)
    * [Open file](#open-file)
    * [Manipulate with files (move, copy)](#manipulate-with-files--move-copy-)
  * [Try -- Except](#try----except)
  * [Builtin functions](#builtin-functions)
  * [Builtin variables](#builtin-variables)
  * [Builtin modules](#builtin-modules)
    * [Sys](#sys)
    * [io](#io)
    * [os](#os)
    * [Pathlib - Work with paths](#pathlib---work-with-paths)
    * [Warnings](#warnings)
    * [re - Regular expressions](#re---regular-expressions)
    * [Subprocess - Run shell comands](#subprocess---run-shell-comands)
    * [Pickle](#pickle)
    * [Time and datetime](#time-and-datetime)
    * [Argparse - Command Line Interface (cli)](#argparse---command-line-interface--cli-)
  * [Concurrent - Asynchronous code](#concurrent---asynchronous-code)
  * [Imported libraries](#imported-libraries)
    * [Sphinx - Create documentation](#sphinx---create-documentation)
    * [Tests](#tests)
      * [Pytest](#pytest)
    * [Plots, graphs](#plots-graphs)
      * [Plotly](#plotly)
      * [Matplotlib](#matplotlib)
    * [Web](#web)
      * [Requests - API - GET, POST](#requests---api---get-post)
      * [Beautiful soup - web scrapping](#beautiful-soup---web-scrapping)
    * [Images, pictures](#images-pictures)
    * [Mathematics, statistics, linear algebra](#mathematics-statistics-linear-algebra)
    * [Signal processing and controll](#signal-processing-and-controll)
    * [Database](#database)
      * [pyodbc, sqlalchemy](#pyodbc-sqlalchemy)
    * [GUI](#gui)
    * [Misc](#misc)
      * [Tables](#tables)
    * [Jupyter, IPython](#jupyter-ipython)
      * [Magic](#magic)
      * [Misc](#misc-1)
  * [Building app (executables) - Pyinstaller](#building-app--executables----pyinstaller)
  * [Performance](#performance)
    * [Profiling](#profiling)
      * [Line profiling](#line-profiling)
    * [Max execution time function](#max-execution-time-function)
    * [Numba](#numba)
    * [Dask](#dask)
  * [Garbage collector](#garbage-collector)
  * [Miscellaneous](#miscellaneous)
  * [Misc](#misc-2)
    * [Snippets - examples](#snippets---examples)
<!-- TOC -->

## General

**Show where python is installed**

    # On windows
    where python

    # On linux
    which python

**Python on linux**

**Use python instead of python3**

Add to source ~/.bashrc

```shell
alias python=python3
alias pip=pip3
```

**Open terminal in current folder**

```console
sudo apt-get install nautilus-open-terminal
```

**Set default python on linux**

```console
sudo apt-get install update-alternatives
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.8 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 2
sudo update-alternatives --config python
```

## Cookiecutter - Project scaffolding

You can find typical python project structure here

[Starter project](https://github.com/Malachov/mypythontools/tree/master/content/project-starter)

It contains testing files, files for sphinx auto documentation, licence and more.

## Package management

**Install library**

```console
pip install library_name

# For anaconda
conda install library_name

# If conda not work
conda install -c anaconda library_name
```

**Show installed libraries**

```console
pip list
```

**Show outdated libraries**

```console
pip list --outdated
```

**If pip cannot be installed by SSL error**

    pip install ipykernel --upgrade pip --trusted-host pypi.org

### Virtual environment (venv)

```console

pip install virtualenv

# Create new virtual environment
virtualenv venv

# Activate virtual env
vevn\Scripts\activate.bat
```

### Requirements

File that describe all used libraries for some project. You can install all the libraries at once.

```console
pip install -r /path/to/requirements.txt

# Create requirements
pipreqs --encoding=utf8 C:\VSCODE\Diplomka

# Deprecated
(pip freeze > requirements.txt)
```

You can find much more about libraries in `Modules` section.

## Style guide (linting, formatting)

**pylint** - show you where problems are

**Black** - Strict auto formatting. Setup longer default line length for better user experience

Example for how to use it in VS Code - Add to settings.json:

```
"editor.formatOnSave": true,
"python.formatting.blackArgs": ["--line-length", "110"],
```

If you are not sure how to format code, you can try [pep 8](https://www.python.org/dev/peps/pep-0008/) or [google style guide](https://google.github.io/styleguide/pyguide.html)

## Documentation - docstrings

Posible modes - DocBlockR, ReST, Numpy, Google

**Google**
[Sphinx example](https://www.sphinx-doc.org/en/master/usage/extensions/example_google.html)
```python
"""One sentance overview.

Long description...

Example:
    Examples can be given using either the ``Example`` or ``Examples``
    sections. Sections support any reStructuredText formatting, including
    literal blocks::

        $ python example_google.py

Section breaks are created by resuming unindented text. Section breaks
are also implicitly created anytime a new section starts.

Attributes:
    module_level_variable1 (int): Module level variables may be documented in
        either the ``Attributes`` section of the module docstring, or in an
        inline docstring immediately following the variable.

        Either form is acceptable, but the two should not be mixed. Choose
        one convention to document module level variables and be consistent
        with it.

Todo:
    * For module TODOs
    * You have to also use ``sphinx.ext.todo`` extension

.. _Google Python Style Guide:
   https://google.github.io/styleguide/pyguide.html

"""

module_level_variable2 = 1
"""int: Module level variable documented inline.

The docstring may span multiple lines. The type may optionally be specified
on the first line, separated by a colon.
"""

def function_with_types_in_docstring(param1, param2):
    """Example function with types documented in the docstring.
    
    :pep:`484` type annotations are supported. If attribute, parameter, and
    return types are annotated according to `PEP 484`_, they do not need to be
    included in the docstring:
    
    Args:
        param1 (int): The first parameter.
        param2 (str): The second parameter.
    
    Returns:
        bool: The return value. True for success, False otherwise.
    
    Raises:
        AttributeError: The ``Raises`` section is a list of all exceptions
            that are relevant to the interface.
        ValueError: If `param2` is equal to `param1`.
    """
```

**Numpy**
[Sphinx example](https://www.sphinx-doc.org/en/master/usage/extensions/example_numpy.html)

**reStructured text**

```
Section Header
==============

Subsection Header
-----------------

Other headers

# with overline, for parts
* with overline, for chapters
  =, for sections
  -, for subsections
  ^, for subsubsections
  “, for paragraphs

Link
`Python <http://www.python.org/>`_

Internal link
`Internal and External links`_

List and bullets
* This is a bulleted list.
* It has two items, the second
  item uses two lines. (note the indentation)

1. This is a numbered list.
2. It has two items too.

#. This is a numbered list.
#. It has two items too.

.. image:: stars.jpg
  :width: 200px
  :align: center
  :height: 100px
  :alt: alternate text

Code reference

:py:mod:`mypythontools.helpers`
-------------------------------

1) An enumerated list item

.. image:: /path/to/image.jpg

A sentence with links to `Wikipedia`_

.. _Wikipedia: https://www.wikipedia.org/

+------------------------+------------+----------+
| Header row, column 1   | Header 2   | Header 3 |
+========================+============+==========+
| body row 1, column 1   | column 2   | column 3 |
+------------------------+------------+----------+
| body row 2             | Cells may span        |
+------------------------+-----------------------+

Literal - no linebreaks etc. (empty line and indentation necessary) ::

    Some literal text.

Python code in docstrings

.. code:: python

    print("A literal block directive explicitly marked as python code")
```

## Multi line code
    # This is one string
    long_string = (
        "Lorem ipsum long "
        "Lorem ipsum long"
        "Lorem ipsum long."
    )

    poi = { 
        "1": 3,
        "2": 4,
        "6": 8,
    }

## Comments

    # One line comment

    ## Multiline comment
    """Are usually used for documentation reasons.

    Like that
    """

### Logical conditions (and, or, not...)

**Not**

    not True # => False
    'a' is not 1
    'a' != 1

**Greater than, lower than**

    >=

**And, or**

    0 and 2 # => 0
    -5 or 0 # => -5
    0 == False # => True
    if 2 > 1 and 5 > 6:
        pass

    # Logical conditions can be combined

    1 < 2 < 3 # => True

## Type hints

    def sentence_has_animal(sentence: str) -> bool:
        return "animal" in sentence

## Variables

    y = int(2.8)

    x = 1
    x += 1 # Zkrácený zápis x = x + 1. Pozor, žádné x++ neexisuje

    # Int divided by int

    3 / 2  # = 1.5
    3 // 2  # = 1

**Declaration of more variables at once**

    a = b = c = 1  # If i change one, it doesn't affect the others

    a,b,c = 1,2,"john"

**Swap variables values**

    e = 2; d = 4
    e, d = d, e # d is now 5, e is now 4

**Bulk create variables**

    name = ['mike', 'john', 'steve']
    age = [20, 32, 19]

    for x,y in zip(name, age):
        globals()[x] = y  # mike = 20 ...

    uname = ['u{}'.format(n) for n in range(7)] # [u1, u2, u3...]

**Check if variable exist**

    if 'myVar' in locals():
        pass

**Import variables from other modules**

Creat config.py

There for example

    x = 1

In main then use

config.x = 2 ....

**Find name of variable, list or dictionary**

    var = 3
    my_var_name = [k for k,v in globals().items() if v == var][0]

## Builtin Data types

    a = type(var)  # Return type

**Type of variable as condition**

    if isinstance(var, str):  # Check if it's string (or int etc...)
        pass

    # More types at once

    import numpy as np
    y = np.array([1])
    isinstance(y, (np.ndarray, np.generic))  # pd.DataFrame For dataframe

    if a is list: # Zjistí, zda jde přímo o string
        pass

    # Also work if object is includes in class


### None

    # None is object (NULL, nil, ...)

    None  # => None

    # So don't use "==" for comparison. Rather use "is"

    None is None # => True

    # None, 0, and empty string/list/dictionary is False, everything else True

    bool(0) # => False
    bool("") # => False
    bool([]) # => False
    bool({}) # => False

### String

```python

a = "This is string."
a = 'This is also string.'

# In python 3

print('strings are now utf-8 \u03BCnico\u0394é!')  # strings are now utf-8 μnicoΔé!

# Strings can use + but don't use

a = "Hello " + "world!" # => "Hello world!"

# Can be concatenated without '+'

a = "Hello " "world!" # => "Hello world!"

# String is list of symbols

a = "This is list"[0] # => 'T'
```

**Format - depracated**

```python
# Old

a = '%s  %s' % ('one', 'two')

# Newer

a = '{} {}'.format('one', 'two')

# Format can be used multiple times

a = "{0} {1} stříkaček stříkalo přes {0} {1} střech".format("tři sta třicet tři", "stříbrných")

# You can use named arguments

a = "{jmeno} si dal {jidlo}".format(jmeno="Franta", jidlo="guláš") # => "Franta si dal guláš"
```

**f-strings - Correct way to format**

```python
name = 'Peter'
f"Hello, {name}. You are {2 * 17}. Also functions {name.lower()}."

# You can use raw strings (no escape characters, but still format)

a = 15
print(fr'Escape is here:\n but still {a}')  # Escape is here:\n but still 15

# Keep quotes in f-strings
print(f'He said his name is {name!r}.')  # "He said his name is 'Fred'."

# If you use """ no escape symbols will be used"""
# Also can use dictionaries, but "" is necesarry
# Dont use # in f strings

```

**Eval - String to code**

    mycode = 'x = 1'
    exec(mycode)

    # or
    x = eval("2+2") # number ze stringu

**Join - concatenate**

    words = ["this", 'is', 'a', 'list', 'of', "strings"]
    ' '.join(words)  #returns "This is a list of strings"

### List

```python

empty_list = []
lst = [1, 2, 3, 4]
lst.append(1)  # lst is now [1, 2, 3, 4, 1]
lst.insert(1, 2)  # insert 2 on index 1
lst.pop()  # Remove last value
lst.remove(2)  # Remove first 2 in list
del lst[1]  # Delete first element
del lst[-2:]  # Range delete - from the last one
b = [1,2]
c = lst + b  # Serializes
lst.extend(b)  # Serializes
```

**List comprehensions**

Way to create a list

    u_list = ['u{}'.format(n) for n in range(1, 6)]

```
# Print list
print(*a, sep = ", ")

# If lst a = lst b and we change one of them, the other also change therefor

lista = [1, 2, 3]
listb = lista.copy()
listb[2] = 5
print(lista)  # [1, 2, 5]

### Access members

lst = [1, 2, 3, 4]
lst[0]  # => 1
lst[-1]  # => 3

### Slices

lst[1:3]  # => [2, 4]
lst[2:]  # => [4, 3]
lst[:3] # => [1, 2, 4]

# Every second member

lst[::2] # =>[1, 4]

### Minimum

youngest = min(lst)

### Find maximum and it's index

m = max(lst)
[i for i, j in enumerate(a) if j == m] # pro a = [1,2,0]  # 1

### Sum

suma = sum(lst)

### If member exist

1 in lst # => True

### Length of list

len(lst) # => 6

### Every value just once

t =  [1,  2,  3,  1,  2,  5,  6,  7,  8]
lst = list(set(t))  # [1,  2,  3,  5,  6,  7,  8]

### Reverse

rev = lst[::-1] # => [3, 4, 2, 1]

# Or

rev = t.reverse()

### Iterate in reverse order

for i in reversed(lst):
    pass

### Check if list is empty or not

a = 6
if a:
    pass

if not a:
    pass

### Create list from 0 to 10

l = range(10) #  [0,  1,  2,  3,  4,  5,  6,  7,  8,  9]

### Create list - List comprehension

[x*5 for x in range(5)] #[0, 5, 10, 15, 20]
[x for x in range(5) if x%2 == 0] #[0, 2, 4]
[a if a else  2  for a in  [0,1,0,3]]

# List comprehension from more entities

list_1 = [1, 2, 3]
list_2 = [2, 3, 4]
[(i, j) for i, j in zip(list_1, list_2)] # [(1, 'a'), (2, 'b'...]

### Zip lists

zip(list_1, list_2) # {(a1, b1), (a2, b2)}

### Logical condition on lists

j2 = [i for i in list_1 if i >=  5]

### One value more times

listOfStr = ['Hi'] * 3 # ['Hi', 'Hi', 'Hi']

### Multiple list

my_list =  [1,  2,  3,  4,  5]
my_new_list =  [i *  5  for i in my_list]

### Find index

my_list.index(3)

### Nested lists

t = [[1,2], [3,4]]
print(t[1][1])  # 4

### Every first member of nested lists

L = [[[0,1,2],[3,4,5],[6,7,8]],  [[0,1,2],[3,4,5],[6,7,8]],  [[0,1,2],[3,4,5],[6,7,8]]]
R = [[x[0]  for x in sl ]  for sl in L ]  # [[0, 3, 6], [0, 3, 6], [0, 3, 6]]

# Or

lst = [[1,2,3],[11,12,13],[21,22,23]]
a = list(zip(*lst))[0]  # [1, 11, 21]

### Add first with first, second with second

[a + b for a, b in zip(list_1, list_2)]

### List of functions

def func1():return 1
def func2():return 2
def func3():return 3
fl = [func1,func2,func3]
[f() for f in fl] # [1, 2, 3]

### How many times members in list

import collections
print( collections.Counter(['a', 'b', 'c', 'a', 'b', 'b']))
```

### Tuple

Tuple is like list but imutable !!! [] i can change - () i cannot change !!!

    tuple = (1, 2, 3)
    tuple[0]  # => 1
    # tuple[0] = 3  # Raise TypeError

    a, = 5  # (5)

### Dictionary

    empty_dic = {}
    dic = {"jedna": 1, "dva": 2, "tři": 3}

**Dict comprehension**

    dic_variable = {key:value for (key,value) in dic.items()}

```python
### Add value

dic['čtyři'] = 4  # If key is already there it's updated

### Add more values at once

dic.update({"čtyři": 4, "pět": 5})


### Create dictionary from two lists

name = ['mike', 'john', 'steve']
age = [20, 32, 19]
dic=dict(zip(name, age))

### Assign multiple keys to one value

my_dict = dict.fromkeys(['a', 'b', 'c'], 10)

### Miscelanious

thisdict = {'b':1, 'c':2, 'd':3}
del thisdict['b']  # delete list
'e' in thisdict  # returns False
thisdict.items()  # returns [('a', 1), ('c', 'eggs')]

# All keys

dic.keys()

# You need list sometimes not iterables

list(dic.keys())

### Last key in dictionary

max(dic)

### For cycle for all keys

for s in dic:
    print(s)

### Maximum value and its index

stats = {'a':1000, 'b':3000, 'c': 100}
maxname = max(stats, key=stats.get)
maxvalue = stats[maxname]

### Values

list(dic.values()) # => [3, 2, 1]
"jedna" in dic # => True if value is in dictionary
dic.get("čtyři") # => None - don't raise error if key not in dic
dic.setdefault("pět", 5) # dic["pět"] default 5

### Find key from value

list(stats.keys())[list(stats.values()).index(100)]

### For cycle for dictionaries

for k in stats: # Iteruje přes všechny klíče
    print(k)

for k, v in stats.items(): # Iteruje řes všechny klíče a hodnoty
    print(k,v)

### Join two dictionaries

c = {**dic, **stats}

### Enumerate in dictionaries

for i, (j, k) in enumerate(dic.items()):
    pass

### Dictionary as arguments into function

def rep(*nonamed, **named):
    return nonamed, named

t = (47,11)
d = {'x':'extract','y':'yes'}
rep(*t, **d) # It's the same as f(47, 11, x=extract, y=yes)

### Nested dictionaries - for examples name of functions and it's parameters

def rep2(*inp, **inp2):
    return rep

models = {"AR (Autoregression)": rep, "Linear neural unit": rep2}
modelsparameters = {"AR (Autoregression)": {"predicts":  2}, "Linear neural unit": {"predicts": 4}}
modelscomplet = zip(models.keys(), models.values(),  modelsparameters.values())
modelsresults = []

for i, j, k in modelscomplet:
    modelsresults.append({i: j(1, **k)})

### Nested dictionaries - Find minimum

top = 1000000
for key, value in modelsparameters.items():
    for inkey, invalue in value.items():
        if invalue < top:
            best_model_name = key
            best_data = inkey
            top = invalue

### Dictionary comprehension

{x: x**2 for x in range(1, 5)} # => {1: 1, 2: 4, 3: 9, 4: 16}
{pismeno for pismeno in "abeceda"} # => {"d", "a", "c", "e", "b"}
```

**Two dictionaries intersection**

    d1 = {'a': 1, 'b': 2}
    d2 = {'b': 2, 'c': 3}

    d1.viewkeys() & d2.viewkeys()  # {'b'}

    # Or with set
    a = { 'x' : 1, 'y' : 2, 'z' : 3 }
    b = { 'u' : 1, 'v' : 2, 'w' : 3, 'x'  : 1, 'y': 2 }
    set( a.keys() ) & set( b.keys() )  # Output set(['y', 'x'])
    set( a.items() ) & set( b.items())  # Output set([('y', 2), ('x', 1)])

### Deque

```python
You can iterate from both sides

import collections
de = collections.deque([1, 2, 3])
de.append(0)  # Add x to the right side of the deque.
de.appendleft(6)  # Add x to the left side of the deque.
de.count(2)  # Count the number of deque elements equal to x.
lst = [1, 2, 3]
de.extend(lst)  # Extend the right side of the deque by appending elem   ents from the iterable argument.
de.pop()  # Remove and return an element from the right side of the deque. If no elements are present, raises an IndexError.
de.popleft()  # Remove and return an element from the left side of the deque. If no elements are present, raises an IndexError.
de.remove(2)  # Remove the first occurrence of value. If not found, raises a ValueError.
de.reverse()  # Reverse the elements of the deque in-place and then return None.
de.rotate(1)  # Rotate the deque n steps to the right. If n is negative, rotate to the left.
de.clear()  # Remove all elements from the deque leaving it with length 0.
```

### Set

It is not oredered and every value is just once!

```python
empty_set = set()
sett = {1, 1, 2, 2, 3, 4}  # {1, 2, 3, 4}
sett.add(5)  # {1, 2, 3, 4, 5}
jina_set = {3, 4, 5, 6}

# Intersect of 2 sets

sett & jina_set # => {3, 4, 5}

# Union

sett | jina_set # => {1, 2, 3, 4, 5, 6}

# Exception

{1, 2, 3, 4} - {2, 3, 5} # => {1, 4}

# If member exist

2 in sett # => True
9 in sett # => False
```

### Decimal

```python

In normal float 1 + 1 is not 2 (but 2.0000000000001...)
In decimal it's equal

import decimal
# Decimal
a = decimal.Decimal('0.1')
b = decimal.Decimal('0.2')
c = a + b # returns a Decimal representing exactly 0.3

# Rounding
"%.3f" % 1.2399 # returns "1.240"
"%.2f" % 1.2 # returns "1.20"
```

## Imported data types
### Dataframe

Panda library is necessary
If there is a parameter inplace=True, then changes are made on original, otherwise change is only made for new variable assign

```python
### Import from csv

impt = '''
import pandas as pd
data = pd.read_csv(
    "data/files/complex_data_example.tsv",
    sep='\t',  # Tab-separated value file.
    quotechar="'",  # single quote allowed as quote character
    dtype={"salary": int},  # Parse the salary column as an integer
    usecols=['name', 'birth_date'].  # Only columns
    parse_dates=['birth_date'],  # Intepret the birth_date column as a date
    skiprows=10,  # Skip the first 10 rows of the file
    na_values=['.', '??']  # Take any '.' or '??' values as NA
)
'''

### Save into CSV

# df.to_csv('newcsv.csv') # bez názvů , header=False

### Create

import pandas as pd
# one column dataframe

s2 = pd.Series([1,2,3,4])

# dataframe

s2 = pd.DataFrame([1,2,3,4])

# from list

data = [['tom', 10, 1], ['nick', 15, 2], ['juli', 14, 3]]
df = pd.DataFrame(data, columns = ['name', 'age', 'index'])

# from dictionary

dict = {'a': 1, 'b': 2}
newdatf = pd.DataFrame.from_dict(dict, orient='index')

# from array

array = np.array([[1, 2], [2, 3], [3, 4]])
#df2 = pd.DataFrame(data=array[1:,1:], index=range(len(data)), columns=data[0,1:])

### Access column

df['name']

### Subset of columns - Access members

# With name

df1 = df[['name','age']]

# with index

df2 = df.iloc[:,0:1]  #   # All rows, first and second column

df3 = df.iloc[0]  # First row

### Column to new dataframe

a = df.pop('index')

### Return name of column from index

a = df.columns[0]  # Columns return name of column

### Find index from column name

a = df.columns.get_loc("age")

### Logical conditions

df_new = df.loc[df['name'] == 'juli']

### Concat 2 columns

df['name_and_age'] = df['name'] + str(df['age'])

### Make index from colmn

df.set_index('name', inplace=True)
df.reset_index(level=None, drop=False, inplace=False)

### Convert into array

df['age'].values
b=df1.iloc[:,1:].values  # every column separately

### Convert into list

lst = df['age'].values.tolist()

### Miscelaneous

df.index  # RangeIndex(start=0, stop=x....)
df.dtypes  # age  int ...

### Lenght of dataframe

length = len(df.index)

### Date and time and datetime, range

# Datetime from values

year = 2020; month = 3; day = 14; hour = 4
start = pd.Timestamp(year=year, month=month, day=day, hour=hour)

# Range

df['EventStart'] = pd.date_range(start=start, periods=length, freq='H')

# Convert from datetime to time, the same to datetime

df['EventStart_time'] = df['EventStart'].dt.time

### Set index

df.set_index('EventStart', drop=True, inplace=True)
df.index = pd.to_datetime(df.index)

### Sort index
data_for_predictions_full.sort_index(inplace=True)

### Resample datetime dataframe

df_res = df.resample('M').sum() # Méně řádků na výstupu

### Copy of dataframe

# !!! If you use variables., change in one is also changed in the others, so if you want independent dataframes, use copy() !!!

df2 = df.copy()

### Add column on index 0
df.insert(0, 'New column', df.loc[:, 1])  # (loc, column, value)

### Pop, extract column to variable and drop it from original
df.pop(df.columns[0])

### Move column on index 1
df.insert(0,'predicted_column_name', df.pop(predicted_column_name.columns[1]))

### Join 2 dataframes

#### Concat
# possible parameters - axis, join, ignore_index, sort, keys, levels

df = pd.DataFrame([['a', 1], ['b', 2], ['c', 3]], columns=['letter', 'number'])
    #         letter  number
    #    0      a       1
    #    1      b       2
    #    2      c       3

df2 = pd.DataFrame([['c', 1], ['d', 2], ['e', 3]], columns=['letter', 'number2'])

    #        letter  number
    #    0      c       1
    #    1      d       2
    #    2      e       3

# Add rows

df_rows = pd.concat([df, df2])

    #        letter  number  number2
    #    0      a     1.0      NaN
    #    1      b     2.0      NaN
    #    2      c     3.0      NaN
    #    0      c     NaN      1.0
    #    1      d     NaN      2.0
    #    2      e     NaN      3.0

    # join='inner' means only columns that are in both dfs

# Add columns

df_columns = pd.concat([df, df2], axis=1)

    #        letter  number letter  number
    #    0      a       1      c       1
    #    1      b       2      d       2
    #    2      c       3      e       3

# or you can use

df4 = df.append(df2)
    #         letter  number  number2
    #    0      a     1.0      NaN
    #    1      b     2.0      NaN
    #    2      c     3.0      NaN
    #    0      c     NaN      1.0
    #    1      d     NaN      2.0
    #    2      e     NaN      3.0

### Merge
# Add database parameters like left join, outer join

result = pd.merge(df, df2, on='letter', how='left')

    #        letter  number  number2
    #    0      a       1      NaN
    #    1      b       2      NaN
    #    2      c       3      1.0

### Group by

gpd = df4.groupby('letter').agg({'number': np.mean, 'number2': np.size})

    #        number  number2
    #  letter
    #    a	   1.0	   1.0
    #    b	   2.0	   1.0
    #    c	   3.0	   2.0   # c only once, not twice
    #    d	   NaN	   1.0
    #    e	   NaN	   1.0

#### Get all from one group

gpd = df4.groupby('letter')
c = gpd.get_group('c')

    #        letter  number  number2
    #    2      c     3.0      NaN
    #    0      c     NaN      1.0

### Mean, standard deviation

mean = df['number'].mean()
std = df['number'].std()

### Rolling (moving) average and standard deviation

rolling_mean = df['number'].rolling(10).mean()
rolling_std = df['number'].rolling(10).std()

# Change axis
a = np.zeros((3, 4, 5))
a = np.moveaxis(a, 0, -1).shape
(4, 5, 3)

### Remove outliers

df_removed_outliers = df[ (df['number'] < 2 * std) ]

### Remove not a number columns
gpd = gpd.select_dtypes(['number'])

### Transpose - Rows into columns

df = df.T
```

### Numpy Array

```python

a = ar[1, 2]  # 5 - Access array
a = np.append(a, 3)  # Add element to the end
a = np.insert(a,1,[11,12])  # Into a on index 1 insert [11,12], next parameter can be axis
np.roll(a,2) # Posune array o 2 doprava
wo = np.array([1,2,3]) # Shape (3,)
wo = np.array([[1,2,3,4,5]]) # Shape (1,5)
wo = np.array([[1],[2],[3],[4],[5]]) # Shape (5,1)
shape = np.shape(a) # `(n,m) Number of rows, columns etc.
ar.shape[0]  # How many rows`

## Convert

a = np.array([1, 2, 3])
my_list = ar.tolist()  # Convert on list
one_dim_list = np.array(ar).reshape(-1).tolist()  # Convert to one-dimensional list
a_scal = a[0].item()  # from np.int convert on int

### Convert to other dtype

b = a.astype(int)  # convert on np.int


### Slicing

a = np.array([[1,2,3],[3,4,5],[4,5,6]])

    #    [[1 2 3]
    #     [3 4 5]
    #     [4 5 6]]

# Only column - not retain shape

b = a[:, 1]  # [2 4 5]

# Columns with same shape

b = a[:, 1:2]  #   [[2]
                #    [4]
                #    [5]]

# Only rows

b = a[1, :]

# Fancy indexing - Selecting with boolean mask

a = np.array([1, 2, 3, 4, 5, 6, 7])
mask = (a % 3 == 0)  # [False False True False False True False]
a = a[mask]  # [3, 6]

# Selecting subset of matrix with np.take

a = np.array([[4, 3, 5], [5, 7, 4], [6, 5,  8], [6, 5 , 8]])
b = np.take(a, [1, 2], axis=0)  # Must use axis, else flattened
            #  [[5 7 4]
            #   [6 5 8]]

b = np.take(a, [1, 2], axis=1)
            #  [[3 5]
            #   [7 4]
            #   [5 8]
            #   [5 8]]

# Swap two rows

data[[0, 2], :] = data[[2, 0], :]

### Join matrixes

np.vstack([a,a])

    #   [[1, 2, 3],
    #    [3, 4, 5],
    #    [4, 5, 6],
    #    [1, 2, 3],
    #    [3, 4, 5],
    #    [4, 5, 6]])

np.hstack([a,a])

    #    [[1, 2, 3, 1, 2, 3],
    #     [3, 4, 5, 3, 4, 5],
    #     [4, 5, 6, 4, 5, 6]]

# Swap two columns

a[:,[0, 1]] = a[:,[1, 0]]

# Create matrix from vectors

a = np.array([1, 2, 3])
b = np.array([2, 3, 4])
c = np.stack((a, b))    # [[1, 2, 3],
                        #  [2, 3, 4]]

# Concatenate

a = np.array([[1, 2], [3, 4]])
b = np.array([[5, 6]])
np.concatenate((a, b), axis=0)
                                        # array([[1, 2],
                                        #        [3, 4],
                                        #        [5, 6]])
np.concatenate((a, b.T), axis=1)
                                        #  array([[1, 2, 5],
                                        #         [3, 4, 6]])
np.concatenate((a, b), axis=None)
                                        # array([1, 2, 3, 4, 5, 6])

# Append
# Add column

x = np.array([[10,20,30], [40,50,60]])
y = np.array([[100], [200]])
z = np.append(x, y, axis=1)

    #   [[ 10  20  30 100]
    #    [ 40  50  60 200]]

### Find minimum value

min = np.amin(c, axis=1)

### Mean

mean = np.mean(x)

### Find maximum or minimum absolute values
aa = max(a.min(), a.max(), key=abs)  # ! Can be the negative one and keep the sign

### Standard deviation

std = np.std(x)

### Limit array values
​
aa = np.array([1., 2., 3., -4, 5, 6])
np.minimum(aa, 3, out=aa)  # array([ 1.,  2.,  3., -4.,  3.,  3.])

### Find index of smallest value

ind = np.unravel_index(np.argmin(a), shape=a.shape)

### Create zero or ones matrix of given shape

zeros = np.zeros_like(a)

# Ones

ones = np.ones((3,3))

    #   [[1., 1., 1.],
    #    [1., 1., 1.],
    #    [1., 1., 1.]]

### Save slice scope to variable
the_slice = np.index_exp[1:]  # the_slice = numpy.index_exp[1:3, 1:3]

### Replace all values with logical condition

a[a > .5] = .5

### Delete member

a = np.delete(a, 1, axis=0) # There need to be variable before!  axis 0 are rows, 1 are columns

### Numpy negative - invert logic
# Use ~

### If nan in array

reality_results_matrix[iterated_model_index, data_length_index]

### Delete Nan values

a = a[~np.isnan(a)]

### Delete all rows where are Nan

a = a[~np.isnan(a).any(axis=1)]

### Replace nan with number

a = np.nan_to_num(a, 0)

### Delete all members that are bigger than condition
aa = np.array([1, 2, 3, 4, 1, 0, 0, 0.2])
b = np.where(aa>=1,aa,1)

### Rolling slides
window = 2
aaa = np.array([1, 2, 3, 4])
bbb = np.array([[1, 2, 3, 4], [3, 4, 5, 6]])
shape = aaa.shape[:-1] + (aaa.shape[-1] - window + 1, window)
shape_b = bbb.shape[:-1] + (bbb.shape[-1] - window + 1, window)
strides = aaa.strides + (aaa.strides[-1],)
strides_b = bbb.strides + (bbb.strides[-1],)
ppp = np.lib.stride_tricks.as_strided(aaa, shape=shape, strides=strides)  #[[[1 2], [2 3], [3 4]]
qqq = np.lib.stride_tricks.as_strided(bbb, shape=shape_b, strides=strides_b)


            # [[[1 2]
            #   [2 3]
            #   [3 4]]
            #
            #  [[3 4]
            #   [4 5]
            #   [5 6]]]

### Sums

# Axis 0 is for sums on columns

np.sum([[0, 1], [0, 5]], axis=0)  # array([0, 6])
np.sum([[0, 1], [0, 5]], axis=1)  # array([1, 5])

### Dot product / multiplication

# Asterisk means each with each not matrix multiplication

# For vectors

x = np.array([1,2,3])
w = np.array([1,2,3])
v = x*w # [1, 4, 9]
v = np.dot(x, w) # 14

x = np.array([1,2,3])
w = np.array([[1],[2],[3]])
v = x*w

    #   [[1 2 3]
    #    [2 4 6]
    #    [3 6 9]]

v = np.dot(x, w) # 14

# For matrixes

x = np.array([[1,2,3], [1,2,3], [1,2,3]])
w = np.array([[1,2,3], [1,2,3], [1,2,3]])
v = x*w

    #   [[1 4 9]
    #    [1 4 9]
    #    [1 4 9]]

v = np.dot(x, w)

    #   [[6 12 18]
    #    [6 12 18]
    #    [6 12 18]]

### Shape

z = np.array([[1, 2, 3, 4],
                [5, 6, 7, 8],
                [9, 10, 11, 12]])
z.shape  # (3, 4)

### Number of members - count

count = z.size

### Cumulative sum

y = np.cumsum(x)

### Set min and max
a = np.array([10, 7, 4, 3, 2, 2, 5, 9, 0, 4, 6, 0])
print (np.clip(a,2,6))  # [6 6 4 3 2 2 5 6 2 4 6 2]

### Find members that are not in other array
a = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9])
b = np.array([3,4,7,6,7,8,11,12,14])
c = np.setdiff1d(a,b)  # array([1, 2, 5, 9])

### Remove non unique values

unique = np.unique(array, axis=0)

### Reshape

# reshape -1 add members automatically
z_res = z.reshape(-1) # array([ 1,  2,  3,  4,  5,  6])
z_res2 = z.reshape(-1,1)

    # array([[ 1],
    #        [ 2],
    #        [ 3],
    #        [ 4],
    #        [ 5],
    #        [ 6]])

z_res3 = z.reshape(-1, 2)
    # array([[ 1,  2],
    #        [ 3,  4],
    #        [ 5,  6],
    #        [ 7,  8],
    #        [ 9, 10],
    #        [11, 12]])

a = np.array([[1,2,3], [4,5,6]])

a_res = np.reshape(a, 6, order='F')
    # array([1, 4, 2, 5, 3, 6])

### Transpose

z = np.array([[[1, 1, 1, 1],
                [1, 1, 1, 1],
                [1, 1, 1, 1]],

                [[2, 2, 2, 2],
                [2, 2, 2, 2],
                [2, 2, 2, 2]],

                [[3, 3, 3, 3],
                [3, 3, 3, 3],
                [3, 3, 3, 3]]
                ])

z_tran = z.transpose(1, 0, 2)  # From (10, 100, 1000) create (10, 1000, 100)

    #    array([[[1, 1, 1, 1],
    #            [2, 2, 2, 2],
    #            [3, 3, 3, 3]],
    #
    #           [[1, 1, 1, 1],
    #            [2, 2, 2, 2],
    #            [3, 3, 3, 3]],
    #
    #           [[1, 1, 1, 1],
    #            [2, 2, 2, 2],
    #            [3, 3, 3, 3]]])

z_tran = z.transpose(1, 2, 0)

    #   array([[[1, 2, 3],
    #           [1, 2, 3],
    #           [1, 2, 3],
    #           [1, 2, 3]],
    #
    #          [[1, 2, 3],
    #           [1, 2, 3],
    #           [1, 2, 3],
    #           [1, 2, 3]],
    #
    #          [[1, 2, 3],
    #           [1, 2, 3],
    #           [1, 2, 3],
    #           [1, 2, 3]]])

### Generate points, arange, linspace, random and generate sin

q = np.random.randn(3, 3)
t = np.linspace(0,20,1000)  # Generate numbers with the same interval (beginning, end, number)
t = np.arange(1, 3, 1)  # Start, stop, step - [1, 2]
t = np.arange(3)  # 0, 1, 2
y = np.sin(t)

### Find index (one) with max / min value

np.argmax(x)  # or argmin. Also can use parameter axis

### Iterate more members

A = np.array([1, 2, 3, 4, 5, 6])
for i in range(len(A)):
    indices = range(i-1,i+1)
    neighbourhood = A.take(indices, mode='clip')  # mode wrap use other end of array on bounds, raise raise error
    print(neighbourhood)
                            # [1 1]
                            # [1 2]
                            # [2 3]
                            # [3 4]
                            # [4 5]
                            # [5 6]

# Or
A = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
neighbourhood = np.zeros(4)
for i in range(1, len(A)):
    if i < len(neighbourhood):
        neighbourhood[-i:] = A[:i]
    else:
        neighbourhood = A[i-4:i]
    print(neighbourhood)

                            # [0. 0. 0. 1.]
                            # [0. 0. 1. 2.]
                            # [0. 1. 2. 3.]
                            # [1 2 3 4]
                            # [2 3 4 5]
                            # [3 4 5 6]
                            # [4 5 6 7]
                            # [5 6 7 8]
                            # [6 7 8 9]

### Eigen values

eig = np.linalg.eig(q)

eigenvalues, V = np.linalg.eig(A)  # eigenvalues[0] is the first eigenvalue
# V is a 2D array (matrix) that contains the four eigenvectors as columns, hence, V[:,i] is the eigenvector corresponding to the eigenvalue eigenvalues[i]

plt.scatter(eigenvalues.real, eigenvalues.imag)  # Plot it

### Inverse matrix

q_inv = np.linalg.inv(q)

### Determinant

det = np.linalg.det(q)

### Fill with Nan values

z = np.zeros((2, 3))
z.fill(np.nan)

### Check if Nan values

if not np.isnan(np.any(z)):
    pass

### Zip for arrays

def azip(*args):
    iters = [iter(arg) for arg in args]
    for i in itertools.count():
        yield tuple([it.next() for it in iters])

# Check which blas / lapack library you use - never use default. Use Intel mkl or atlas
np.__config__.show()
```

### HDF5

```python
'''
### Create

import h5py
import numpy as np

arr = np.random.randn(1000)

with h5py.File('random.hdf5', 'w') as f:
    dset = f.create_dataset("default", data=arr)

### Read

with h5py.File('random.hdf5', 'r') as f:
    data = f['default']
    print(min(data))

### Specify Data Types to Optimize Space

with h5py.File('several_datasets.hdf5', 'w') as f:
    dset_int_1 = f.create_dataset('integers', (10, ), dtype='i1')
    dset_int_8 = f.create_dataset('integers8', (10, ), dtype='i8')
    dset_complex = f.create_dataset('complex', (10, ), dtype='c16')
    dset_int_1[0] = 1200
'''
```

## Iterators

    iterable = [1, 2, 3]
    iterator = iter(iterable)

Next value

    next(iterator) # => "one"

## Conditions (if, else...)

    variable = [10, 20, 30]
    for i in variable:
        if variable:
            print("variable exist")
        if i > 10:
            print("Something")
            pass  # Do nothing
        elif i < 10:
            print("Variable is smaller than 10")
            continue # Jump to other operation
        else:
            print("Variable is 10 or not a number")
            break # Jump out of the cycle

**Ternary operator - If in argument**

    variable = 3
    state = "nice" if variable else "not nice"

**Other shortened syntax**

    a = 4
    b = 3
    x = 77
    y = 66
    result = (y, x)[a > b]  # y if [True], x if [False]

## Loops

### For

    for zvire in ["pes", "kočka", "myš"]:
        print("zvire")
    for i in range(4):
        print(i)
    for i in range(4, 8):
        print(i)
    colors  =  ["red",  "green",  "blue",  "purple"]
    for  i  in  range(len(colors)):
        print(colors[i])

**Return indexes and elements**

    ints = [8, 23, 45, 12, 78]
    for idx, val in enumerate(ints):
        print(idx, val)

**Break out of nested loop**

    for x in xrange(10):
        for y in xrange(10):
            print x*y
            if x*y > 50:
                break
        else:
            continue  # only executed if the inner loop did NOT break
        break  # only executed if the inner loop DID break

**Generate values with strings**

    for k in range(5):
        exec(f'cat_{k} = k*2')

**For in list comprehension**

    [x*5 for x in range(5)] #[0, 5, 10, 15, 20]

**Generate list with for cycle and if**

    [x for x in range(5) if x%2 == 0]  #[0, 2, 4]
    [a if a else  2  for a in  [0,1,0,3]]

**Generate list with more parameters**

    list1 = [1, 2, 3]
    list2 = [3, 4, 5]
    [a + b for a, b in zip(list1, list2)]

**Nested lists**

    a = [[1,2,3],[2,3,4],[5,6,7]]
    print(a[1][1])

### While

    x = 0
    while x < 4:
        print(x)
        x += 1

    # You can use it also somewhere where you don't want to stop

    while True:
        listen_forever()

## Functions

    def secist(x, y):  # Create new function with def
        return x + y  # Return values with return

    secist(5, 6)  # Call the function with parameters, return 11
    secist(y=6, x=5)  # Named arguments

    def return_arguments(*argumenty):  # Variable number of arguments
        return argumenty

    return_arguments(1, 2, 3)  # => (1, 2, 3)

    def return_named_arguments(**pojmenovane_argumenty):  # Varaiable number of named arguments
        return pojmenovane_argumenty

    return_named_arguments(kdo="se bojí", nesmi="do lesa")  # {"kdo": "se bojí", "nesmi": "do lesa"}

    def return_all(*args, **kwargs):  # You can use combination
        print(args, kwargs)  # Return all parameters

**Jump out of function - return**

    def print_var():
        a = 8
        if a > 5:
            return
        print_var(2)

**Default parameter**

    def funkce(y, lags=50): #If we use lags in call, it will be overwritten
        return 1

**Unknown number of parameters - \*args, \*\*kwargs**

    def print_all(*args, **kwargs):
        print(args, kwargs)

    print_all(1, 2, a=3, b=4) # Use: (1, 2) {"a": 3, "b": 4}
    tuple = (1, 2, 3, 4)
    dic = {"a": 3, "b": 4}
    print_all(tuple)  # Is like print_all((1, 2, 3, 4)). One parameter - tuple
    print_all(*tuple)  # Is like print_all(1, 2, 3, 4)
    print_all(**dic)  # Is like print_all(a=3, b=4)
    print_all(*tuple, **dic)  # Is like print_all(1, 2, 3, 4, a=3, b=4)

**Global variables**

    x = 5
    def setx(cislo):  # Local variable override global
        x = cislo  # => 43
        print(x)  # => 43


    def setglobalx():
        global x
        print(x)  # => 5
        x = cislo  # Set global x on 6, so x = 6 also outside the function !!!
    print(x)  # => 6

**Functions are objects**

    def adder(pricitane_cislo):
        def scitacka(x):
            return x + pricitane_cislo
        return scitacka
    add_10 = adder(10)
    add_10(3)  # => 13

**callable**

Is object function or not

    if callable(a):
        pass

**Function in list generator**

    [add_10(i) for i in [1, 2, 3]]  # => [11, 12, 13]
    [x for x in [3, 4, 5, 6, 7] if x > 5]  # => [6, 7]

**Parse function arguments**

    import inspect
    frame = inspect.currentframe()
    args, _, _, values = inspect.getargvalues(frame)

## map, filter, reduce

From the functional programming

Map call funtion (first parameter) on all objects (second parameter)

    map(add_10, [1, 2, 3])

Filter create list (First paratemer), where function is true (second parameter)

    number_list = range(-5, 5)
    less_than_zero = list(filter(lambda x: x < 0, number_list))
    print(less_than_zero)  # [-5, -4, -3, -2, -1]

Reduce - Input sequention into function

    from functools import reduce
    def do_sum(x1, x2): return x1 + x2
    reduce(do_sum, [1, 2, 3, 4]) # 10

## Generators

Generators are functions, that instead return have yield

    def multiplier_2(sequention):
        for i in sequention:
            yield 2 * i

Generator generate values one after one, when it\s needed. Instead of been generated all at once
Example of generator is range(10000)

## Decorators

Dekorators are functions, that wrap other functions, by that
it can change it's behaviour.

    def nekolikrat(puvodni_funkce):
        def repeat_function(*args, **kwargs):
            for i in range(3):
                puvodni_funkce(*args, **kwargs)
        return repeat_function

    @nekolikrat
    def pozdrav(Name):
        print("Bye {}!".format(Name))

    pozdrav("Dan")  # Return 3x: Bye Dan!

**Use decorator with condition**

    from memory_profiler import profile
    use_decorator = False

    def empty_decorator(fu):
        return fu

    if use_decorator == False :
        profile = empty_decorator

    @profile
    def fu():
        print('content')

## Modules

Module in python is file with .py on the end. It is also imported python library (e.g. numpy...).

You can create your own and import it.

    from math import sqrt

    help(sqrt)  # Help with module

    dir(sqrt)  #  Show objects in module

    ### Manual import
    _temp = __import__('spam.ham', globals(), locals(), ['eggs', 'sausage'], 0)
    eggs = _temp.eggs
    saus = _temp.sausage

    ### Import module from path

    import importlib.util
    spec = importlib.util.spec_from_file_location("module.name", "/path/to/file.py")
    foo = importlib.util.module_from_spec(spec)
    spec.loader.exec_module(foo)

    ### Check if module installed without import

    importlib.find_loader(module_name)

    ### Reimport module
    import importlib
    importlib.reload(config)

**Import all scripts in folder (e.g. in **init**.py)**

    import os
    from os.path import dirname, basename, isfile, join
    import glob
    modules = glob.glob(join(dirname(__file__), "*.py"))
    __all__ = [ basename(f)[:-3] for f in modules if isfile(f) and not f.endswith('__init__.py')]

## Classes

```python
class Clovek(object):  # Class Human is child (it inherits from) class object

    druh = "H. sapiens"  # Class variable - it's shared with all objects

    def __init__(self, jmeno):
        self.jmeno = jmeno  # Add parameter to object

    def rekni(self, hlaska):
        return "{jmeno}: {hlaska}".format(jmeno=self.jmeno, hlaska=hlaska)

    @classmethod  # First parameter is class and can be changed in subclasses
    def vrat_druh(cls):
        return cls.druh

    @staticmethod  # Do not depend on object - like a normal function and is immutable
    def odkaslej_si():
        return "*ehm*"


# If no inheritance, no parenthesses prefered, just class Clovek:
# Class can inherit from more classes at once...
# class Clovek(tvor, bytost):

class DecoratorExample:

    def __init__(self):
        self.name = 'Decorator_Example'

    def example_function(self):
        print('I\'m an instance method!')


de = DecoratorExample()
de.example_function()


# Example of class method

class Date(object):

    def __init__(self, day=0, month=0, year=0):
        self.day = day
        self.month = month
        self.year = year

    @classmethod
    def from_string(cls, date_as_string):
        day, month, year = map(int, date_as_string.split('-'))
        date1 = cls(day, month, year)
        return date1

    @staticmethod
    def is_date_valid(date_as_string):
        day, month, year = map(int, date_as_string.split('-'))
        return day <= 31 and month <= 12 and year <= 3999


date2 = Date.from_string('11-09-2012')
is_date = Date.is_date_valid('11-09-2012')

### Create object

d = Clovek(jmeno="David")
a = Clovek("Adéla")
print(d.rekni("ahoj"))  # "David: ahoj"
print(a.rekni("nazdar"))  # "Adéla: nazdar"

### Call class method

d.vrat_druh()  # => "H. sapiens"

### Change atribute of class

d.vrat_druh()  # => "H. neanderthalensis"
a.vrat_druh()  # => "H. neanderthalensis"

### Call static method

Clovek.odkaslej_si()  # => "*ehm*"

### Check if class has atribute (method or )
if hasattr(Clovek, 'rekni'):
    print('Has David')

### Get all objects and values

print(Clovek.__dict__)

### Get all instances name
import gc

for obj in gc.get_objects():
    if isinstance(obj, Clovek):
        print(obj.jmeno)
```

## Magic methods (dunder methods)

You can overwrite some default functions to have specific feature for your class. For example if your object is vector, you can redefine function for adding to be able to use `vector1 + vector2`, which would otherwise failed.

Example **call** method (Allow input of object to function)

The \_\_call** method can be used to turn the instances of the class into callables. Functions are callable objects. A callable object is an object which can be used and behaves like a function but might not be a function. By using the \_\_call** method it is possible to define classes in a way that the instances will be callable objects. The \_\_call\_\_ method is called, if the instance is called "like a function", i.e. using brackets. The following example defines a class with which we can create abitrary polynomial functions:

    class Polynomial:

        def __init__(self, *coefficients):

            self.coefficients = coefficients[::-1]

        def __call__(self, x):

            res = 0

            for index, coeff in enumerate(self.coefficients):

                res += coeff * x** index

                return res

    p1 = Polynomial(42)
    p2 = Polynomial(0.75, 2)
    p3 = Polynomial(1, -0.5, 0.75, 2)

    for i in range(1, 10):
        print(i, p1(i), p2(i), p3(i))

Magic methods (For example for change of + behaviour)
What happens when we create an object in python class ?

    '''
    List of magic methods:
    Binary Operators

    Operator           Method\
    +                       object.__add__(self, other)\
    -                        object.__sub__(self, other)\
    *                        object.__mul__(self, other)\
    //                       object.__floordiv__(self, other)\
    /                        object.__div__(self, other)\
    %                      object.__mod__(self, other)\
    **                      object.__pow__(self, other[, modulo])\
    <<                     object.__lshift__(self, other)\
    >>                     object.__rshift__(self, other)\
    &                       object.__and__(self, other)\
    ^                       object.__xor__(self, other)\
    |                        object.__or__(self, other)

    Assignment Operators:

    Operator          Method\
    +=                     object.__iadd__(self, other)\
    -=                      object.__isub__(self, other)\
    *=                      object.__imul__(self, other)\
    /=                      object.__idiv__(self, other)\
    //=                     object.__ifloordiv__(self, other)\
    %=                    object.__imod__(self, other)\
    **=                     object.__ipow__(self, other[, modulo])\
    <<=                   object.__ilshift__(self, other)\
    >>=                   object.__irshift__(self, other)\
    &=                     object.__iand__(self, other)\
    ^=                      object.__ixor__(self, other)\
    |=                      object.__ior__(self, other)

    Unary Operators:

    Operator          Method\
    -                       object.__neg__(self)\
    +                      object.__pos__(self)\
    abs()                object.__abs__(self)\
    ~                      object.__invert__(self)\
    complex()        object.__complex__(self)\
    int()                  object.__int__(self)\
    long()               object.__long__(self)\
    float()               object.__float__(self)\
    oct()                object.__oct__(self)\
    hex()               object.__hex__(self)

    Comparison Operators

    Operator Method\
    <                      object.__lt__(self, other)\
    <=                    object.__le__(self, other)\
    ==                    object.__eq__(self, other)\
    !=                     object.__ne__(self, other)\
    >=                    object.__ge__(self, other)\
    >                      object.__gt__(self, other)
    '''

    # Let's take an example to override the functionality "+" '[__add__]' operator

    class  Vector(object):
        def __init__(self,  *args):
            """ Create a vector, example: v = Vector(1,2) """
            if len(args)  ==  0:
                self.values =  (0,0)
            else:
                self.values = args
        def __add__(self, other):
            """ Returns the vector addition of self and other """
            added = tuple(a + b for a, b in zip(self.values, other.values)  )
            return  Vector(*added)

    # Now use the "+" operator with two vectors

    v1 =  Vector(1,  2)
    v2 =  Vector(10,  13)
    v3 = v1 + v2
    v3.values  # (11,  15)

    # When statement "v3 = v1 + v2 " executes "__add__" is called and it returns a new Vector object.

**Repr**

class Pizza:
def **init**(self, ingredients):
self.ingredients = ingredients

    def __repr__(self):
        return f'Pizza({self.ingredients!r})'

If Pizza() return **repr**

## Bitwise operations

Unsigned integers can hold bigger values, but cannot be negative!!!

```python
### Convert number to bits

binn = bin(123)  # 0b1111011

# Or

four_bytes = 16.to_bytes(4, byteorder='big', signed=True)
print(four_bytes)

### Convert with keep bit structure

bin_number = f'{3:08b}'  # 00000011
bin_number = f'{3:#08b}'  # 0b000011

### Bit length

a.bit_length()

# Convert back to int again

int('11111111', 2)  # 255

# Or i = int.from_bytes(some_bytes, byteorder='big')

# Create empty bytes
empty_bytes = bytes(4)

### And, or etc...

a = 60            # 60 = 0011 1100
b = 13            # 13 = 0000 1101
c = 0

### Bit and
c = a & b;        # 12 = 0000 1100

### Bit or
c = a | b;        # 61 = 0011 1101

### bitwise exclusive or
c = a ^ b;        # 49 = 0011 0001

### Flip bits
c = ~a;           # -61 = 1100 0011

# Bit shift (4 bits)
c = a << 2;       # 240 = 1111 0000
c = a >> 2;       # 15 = 0000 1111

# Formating
bin = "{0:b}".format(i) # binary: 11111111
hex = "{0:x}".format(i) # hexadecimal: ff
oct = "{0:o}".format(i) # octal: 377

```

## File I/O

**Import fuction from other file**

    from math import sqrt  # First name of file, than function

**Find script's adress**

    import os
    # os.path.dirname(__file__)  # Not working in jupyter !

**Import variables from file in the same folder**

    # import file  # Then call file.value
    # from file import * # Now call just variable

**If file or dir exists**

    from pathlib import Path
    my_file = Path("/path/to/file")
    if my_file.is_file():  # File exists
        pass
    if my_file.is_dir():  # Directory exists
        pass
    if my_file.exists():   # File or dir exists
        pass

**Create module from folder**

Add file `__init.py__`
Inside do all imports from .autoregLNU import autoregLNU
Use relative imports with dots

**Show all files in folder**

    # all_files = os.listdir("test/")

**Filter for one type data**

    # txt_files = filter(lambda x: x[-4:]  ==  '.txt', all_files)

**Count files with certain suffix**

    tifCounter = len(glob.glob1(myPath,"*.tif"))

**Load all files with certain suffix**

    import glob
    imgs=glob.glob("*.png")  # Všechny obrázky ze složky

    # Wildcard

    for name in glob.glob('dir/file?.txt'):
        print (name)

    for name in glob.glob('dir/*[0-9].*'):
        print (name)

**Add file names from folder to list**

data_names_list = list(glob.glob((script_dir / '\*.dat').as_posix()))

**Import txt**

    from numpy import loadtxt
    # x=loadtxt('realna_data_klapky.txt')

### Open file

    f = open("test.txt", "w+") # open file in current directory

    'r'  Open a file for reading. (default)
    'w'  Open a file for writing. Creates a new file if it does not exist or truncates the file if it exists.
    'x'  Open a file for exclusive creation. If the file already exists, the operation fails.
    'a'  Open for appending at the end of the file without truncating it. Creates a new file if it does not exist.
    't'  Open in text mode. (default)
    'b'  Open in binary mode.
    '+'  Open a file for updating (reading and writing)


    f = open("C:/Python33/README.txt") # specifying full path
    f.close()

    with open("test.txt",'w')  as f:
        pass
    f.write("my first file\n")
    f.write("This file\n\n")
    f.write("contains three lines\n")

    !!!Use because this will autamatically close the file finally

    f = open("test.txt",'r',encoding =  'utf-8')
    f.read(4)  # Read the first 4 data - 'This'
    f.read(4)  # Read the next 4 data - ' is '
    f.read()  # Read in the rest till end of file - 'my first file\nThis file\ncontains three lines\n'
    f.read() # Further reading returns empty sting

    # !!! After editing text, jump with cursor to the beginning before read
    f.seek(0)
    f.read()  # Return all
    '''

### Manipulate with files (move, copy)

    '''
    ### Copy file ###

    import shutil, os
    s.chdir('C:\\')
    shutil.copy('C:\\spam.txt', 'C:\\delicious')

    ### Copy folder with all files ###
    shutil.copytree('C:\\bacon', 'C:\\bacon_backup')

    ### Move files ###
    shutil.move('C:\\bacon.txt', 'C:\\eggs')

    ### Remove - move to trash bin ###
    import send2trash
    send2trash.send2trash('bacon.txt')

    ### Remove file ###
    os.unlink(path)

    ### Remove folder with all content ###
    shutil.rmtree(path)

    ### File tree - walk ###
    import os

    for folderName, subfolders, filenames in os.walk('C:\\delicious'):
        print('The current folder is ' + folderName)

        for subfolder in subfolders:
            print('SUBFOLDER OF ' + folderName + ': ' + subfolder)
        for filename in filenames:
            print('FILE INSIDE ' + folderName + ': '+ filename)
    '''

## Try -- Except

    try:
        print(znam)
    except:
        print('except')
    else:
        print('else')
    finally:
        print('final')

    try:
        print(neznam)
    except:
        print('except')
    else:
        print('else')
    finally:
        print('final')

**Try for all errors and print it**

    try:
        linux_interaction()

    except Exception as error:
        print(error)

**Raise exception**

    # if somethingbad:
    #   raise Exception('x should not exceed 5. The value of x was: {}')  # Better concrete error. E.g. ValueError...

**Raise in except block**

    try:
        1/0
    except Exception:
        raise

**Assert - Require something or error**

    x = 5
    assert (x > 4), 'What happened'

**Print error details in except block**

    import traceback

    try:
        1/0
    except Exception as e:
        print(traceback.format_exc())

## Builtin functions

**Print**

    a = 'hi'
    print('as', a, 'fegg') # Use comma, you can join strings and variables

**Range**

    x=range(3+1) # x = 0, 1, 2, 3

**Reversed**

    for i in reversed(range(5)):
        print(i) # 4, 3, 2, 1

## Builtin variables

**\_\_name\_\_**

If file is runned from inside or is imported, code inside condition will not be executed. If file itsel will be started, code will execute.

    if __name__ == "__main__":
        pass

## Builtin modules

### Sys

**Get size on memory**

    import sys
    x = 2
    sys.getsizeof(x)

**Add path to sys path**

sys.path.insert(0, "/path/to/your/package_or_module")

### io

**Capture everything printed plus warnings**

    import sys, io
    stdout = sys.stdout
    sys.stdout = io.StringIO()

    warnings.warn(f"asdefwefwg")
    print("aho")

    # get output and restore sys.stdout
    output = sys.stdout.getvalue()
    sys.stdout = stdout

    print(output)

### os

**Change working directory**

    import os
    os.chdir(r"C:\Users\...")

**Environment variables**

    import os
    env_var = os.environ['ENV_VAR']

### Pathlib - Work with paths

```python

# Get path of running file
from pathlib import Path
import inspect
import os

# Get script path
file_path = Path(__file__)

# If it has to also work in jupyterm, you cannot use __file__
file_path = Path(os.path.abspath(inspect.getframeinfo(inspect.currentframe()).filename))

# Directory path
dir_path = file_path.parent

# Parents
dir_path = file_path.parents[2]

# Combining paths
file_to_open = data_folder / "subfolder" / "raw_data.txt"

# Current working directory
path = pathlib.Path.cwd()

# Pathlib as string

path.as_posix()

# Find full adress

path = pathlib.Path('test.md')  # test.md
path.resolve() # /home/gahjelle/realpython/test.md'

# Compare paths
path.resolve().parent == pathlib.Path.cwd() # False

# Work with file

filename = pathlib.Path("source_data/text_files/raw_data.txt")

print(filename.name) # prints "raw_data.txt"
print(filename.suffix) # prints "txt"
print(filename.stem) # prints "raw_data"

if not filename.exists():
    print("Oops, file doesn't exist!")
else:
    print("Yay, the file exists!")

# Path of Desktop
desktop = Path.home() / "Desktop"


'''

!! Next rows just historical - Do not do it that way !!

from os import path
import os

file_path = path.relpath("data/data.txt")

### Absolute path

# os.path.abspath(\__file__)  # Not working in jupyter

### Add path to files and modules

import os
data_folder = "folder/nextfolder/"
file_to_open = data_folder + "data.txt"

# For example if jupyter do not see some files

script_dir0 = os.path.abspath('') # C:\VScode

# Next

# script_path = os.path.abspath(\__file__)  # i.e. /path/to/dir/foobar.py  - notworking with jupyter
script_path = 'C://prog'
script_dir = os.path.split(script_path)[0]  #i.e. /path/to/dir/
rel_path =  "2091/data.txt"
abs_file_path = os.path.join(script_dir, rel_path)  # Result is relative '/path/to/dir/2091/data.txt'
filename = os.path.join(script_path,  '../same.txt')  # We can use name of file

dir_path = os.path.dirname(os.path.realpath(__file__))  # We can use adress of file... but not in jupyter

# You can also do

# script_dir = os.path.dirname(\__file__)
rel_path = "test_ data/realna_data_klapky.txt"
abs_file_path = os.path.join(script_dir, rel_path)
'''
```

### Warnings

    import warnings
    warnings.warn("Warning...........Message")

    # Warnings with word 'HessianInversionWarning' will be caused as errors

    warnings.filterwarnings('error', message=r"[\s\S]*HessianInversionWarning*")

    # Errory of category depracation will be show everytime

    warnings.filterwarnings('always', category=DeprecationWarning)
    warnings.filterwarnings('ignore')  # Errory budou vždy ignorovány

    # "default" will print the first occurrence of matching warnings for each location
    # (module + line number) where the warning is issued

    # Other possibilities

    # "once"    |    "error"    |    "ignore"    |    "always"    |    "module"

print the first occurrence of matching warnings for each module where the warning is issued (regardless of line number)

### re - Regular expressions

    regs = '''
    import re

    [] 	A set of characters 	"[a-m]"
    \ 	Signals a special sequence (can also be used to escape special characters) 	"\d"
    . 	Any character (except newline character) 	"he..o"
    ^ 	Starts with 	"^hello"
    $ 	Ends with 	"world$"
    * 	Zero or more occurrences 	"aix*"
    + 	One or more occurrences 	"aix+"
    {} 	Exactly the specified number of occurrences 	"al{2}"
    | 	Either or 	"falls|stays"

    \A 	Returns a match if the specified characters are at the beginning of the string 	"\AThe"
    \b 	Returns a match where the specified characters are at the beginning or at the end of a word 	r"\bain"
    r"ain\b"

    \B 	Returns a match where the specified characters are present, but NOT at the beginning (or at the end) of a word 	r"\Bain"
    r"ain\B"

    \d 	Returns a match where the string contains digits (numbers from 0-9) 	"\d"
    \D 	Returns a match where the string DOES NOT contain digits 	"\D"
    \s 	Returns a match where the string contains a white space character 	"\s"
    \S 	Returns a match where the string DOES NOT contain a white space character 	"\S"
    \w 	Returns a match where the string contains any word characters (characters from a to Z, digits from 0-9, and the underscore _ character) 	"\w"
    \W 	Returns a match where the string DOES NOT contain any word characters 	"\W"
    \Z 	Returns a match if the specified characters are at the end of the string 	"Spain\Z"

    [arn] 	Returns a match where one of the specified characters (a, r, or n) are present
    [a-n] 	Returns a match for any lower case character, alphabetically between a and n
    [^arn] 	Returns a match for any character EXCEPT a, r, and n
    [0123] 	Returns a match where any of the specified digits (0, 1, 2, or 3) are present
    [0-9] 	Returns a match for any digit between 0 and 9
    [0-5][0-9] 	Returns a match for any two-digit numbers from 00 and 59
    [a-zA-Z] 	Returns a match for any character alphabetically between a and z, lower case OR upper case
    [+] 	In sets, +, *, ., |, (), $,{} has no special meaning, so [+] means: return a match for any + character in the string

    '''

### Subprocess - Run shell comands

    # Subprocess replace the os.system()
    import subprocess

    subprocess.run('ls')
    p1 = subprocess.run(['ls', '-la']) # with capture_output=True only save to variable
    print(p1.stdout)

    # More general - can define stdout and more here
    sts = subprocess.Popen("mycmd" + " myarg", shell=True).wait()

### Pickle

Save file in binary format

    import pickle
    d = {"a": 1, "b": 2}
    # with open(r"someobject.pickle", "wb") as output_file: cPickle.dump(d, output_file)

Load files

    # with open(r"someobject.pickle", "rb") as input_file: e = cPickle.load(input_file)

Result is `e = {"a": 1, "b": 2}`

### Time and datetime

    import datetime as dt
    import time

    ### Print today's date

    date = datetime.datetime.now().strftime('%d-%m-%Y')

    ### Measure time interval

    start = time.time()
    a = 6
    end = time.time()
    print(end - start)


    ### Wait for some time

    time.sleep(1) # wait 1 second

### Argparse - Command Line Interface (cli)
argparse

If you want to call a script from terminal like `python myscript.py` and want to add some optional arguments (one letter -a , -b etc. or word with two dashes --var)

Nowadays argparse is better and more often used than sys.argv, getopt

    import argparse

    parser = argparse.ArgumentParser(description='A tutorial of argparse!')
    parser.add_argument('-l','--list', default=1, type=int, help="This is the 'l' variable")
    parser.add_argument("--education",
                        choices=["highschool", "college", "university", "other"],
                        required=True, type=str, help="Your name") # If type list, just append action='append'

    args = parser.parse_args()

    # If error with jupyter, then !!!
    par_args = parser.parse_known_args()
    myarg = par_args[0].myarg

    ed = args.education

    if ed == "college" or ed == "university":
        print("Has degree")
    elif ed == "highschool":
    print("Finished Highschool")
    else:
        print("Does not have degree")

## Concurrent - Asynchronous code

    from concurrent.futures import ThreadPoolExecutor

    executor = ThreadPoolExecutor(max_workers=3)
    executor.submit(myFunction())

    # Or use
    with ThreadPoolExecutor(max_workers=3) as executor:
            future = executor.submit(task, (2))
            future = executor.submit(task, (3))
            future = executor.submit(task, (4))

## Imported libraries

### Documentation
#### Sphinx - Create documentation

```console

pip install sphinx

# And
pip install alabaster

# Or
pip install sphinx_rtd_theme

# And if you want to use markdown
pip install recommonmark
pip install sphinxcontrib-napoleon

# Create documentation files

sphinx-quickstart

# Use separate build and source: yes
```

Fill in the `config.py` and `index.rst`

You can use mypythontools [project-starterexample](https://github.com/Malachov/mypythontools/tree/master/content/project-starter/docs/source)

You can build it locally with

```console
make html
```

Or you can make account on [readthedocs](https://readthedocs.org/) - It's free.
And the documentation will be automatically made on every github push to master.

### Tests
#### Pytest

```python
# Write test file

# Use assert functions

def secti(a, b):
return a + b

def test_secti():
    assert secti(1, 2) == 3

pip install pytest

pip install pytest-benchmark
def test_my_function(benchmark):
    result = benchmark(test)


# Run test

python -m pytest -v test_secteni.py

# Run tests in a module
pytest test_mod.py

# Run tests in a directory
pytest testing/

# Paraterize tests

@pytest.mark.parametrize(
        ['year', 'month', 'day'],
        [(2015, 12, 24),
        (2016, 12, 24),
        (2017, 1, 1),
        (2033, 7, 5),
        (2048, 7, 6)],
)

def test_some_holidays(year, month, day):
    """Test a few sample holidays"""
    holidays = isholiday.getholidays(year)
    assert (day, month) in holidays
```

### Plots, graphs

#### Plotly

```python
pltl = '''

import plotly as plo
import cufflinks as cf

def plot_df(df, save_plot=0, save_plot_path="plot.html"):

    cf.go_offline()

    if save_plot:
        fig = df.iplot(asFigure=True)
        plo.offline.plot(fig,filename=save_plot_path)
    else:
        df.iplot(kind='scatter', filename="ploot.html")

    return


import plotly as py
# import cufflinks as cf

fig = dict(data=data, layout=layout )
py.offline.plot(fig, filename='d3-cloropleth-map')

# or

init_notebook_mode(connected=True)
cf.go_offline()

data_ft_date.iplot(xTitle='Dates',yTitle='Returns',title='Returns')


### Plotly timeseries

py.plotly.iplot([{
    'x': data_for_predicts_csv_trimmed.index,
    'y': data_for_predicts_csv_trimmed[col],
    'name': col
}  for col in data_for_predicts_csv_trimmed.columns])

# Or the same with cufflinks
cf.go_offline()
df.iplot(kind='scatter', filename='cufflinks/cf-simple-line')

### From matplotlib to plotly

plot_mpl(fig)

### Save picture

import plotly.io as pio
static_image_bytes = pio.to_image(fig, format='png')
'''
```

#### Matplotlib

```python
mtpltlb = ''' Just to not load in jupyter...
### Simple plot

import matplotlib.pyplot as plt
plt.plot(data)

### Load txt and plot it

from numpy import loadtxt # For one column txt
x = loadtxt('realna_data_klapky.txt')
figure(figsize=(20,5))
plot(x,'-o',label="x");xlabel('i');ylabel('x [sec]');grid();title("realna_data_klapky.txt")
legend();show()
plt.savefig('fig1.png', dpi =  300) # uloží graf

### Jupyter plot one after one

# Use not show()
%matplotlib notebook
import matplotlib.pyplot as plt
import numpy as np

x = [1, 2, 3, 4, 5, 6]

fig, axes = plt.subplots()
axes.plot(range(10))
#figure one


fig, axes = plt.subplots()
axes.plot(range(10))
#figure two


fig, ((a,b),(c,d)) = plt.subplots(2,2)
a.plot(x, np.sin(x))
b.plot(x, np.cos(x))
c.plot(x, np.tan(x))
d.plot(x, np.tanh(x))

### Histogram

figure(figsize=(20,5))
nbins=int(round(1+3.3*log(N))) # Sturge's rule, but depend on data
xlabel("x");ylabel("number_of_bins"),title("number_of_bins  - values x in intervals");grid()
a=hist(x,bins=nbins) # vraci dve pole a histogram, tj a[0]=number_of_bins, a[1]=intervaly
print "number_of_bins=",a[0]
print "intervaly =",a[1]

### Curves and points

x=arange(-600,100) #
mu=m1;sigma=sqrt(s2);print "mu=",mu,"sigma=",sigma
f=1/(sigma*sqrt(2*pi))*exp(-(((x-mu)/sigma)**2)/2)
figure(figsize=(20,5))
plot(x,f,'g',label="f(x) spojita");grid()
plot(x_i,f_odhad,'o',label="f(x) odhad z mereni pro nbin="+str(nbins)+" intervalu")
legend()

### Two graphs

N=1000 # velikost výběru
x=random.uniform(-10,10,N)
x=random.randn(N)*1000-300
x=random.logistic(3, 10, N)
x=random.standard_t(3, N)
x=random.poisson(1, N)*.01+1000
nbins=int(round(1+3.3*log(N)))
figure(figsize=(20,5))
subplot(121);plot(x,'-o');grid();xlabel('index vzorku dat');ylabel("x")
subplot(122)

# First number is how much rows and second how much columns
# Third number is which graph is it - first, second etc.

a = hist(x,nbins,normed=True)
grid(); ylabel("$\\approx f(x)$"); xlabel("x")
plt.tight_layout() # Pokud část jednoho překrývá druhý
plt.subplots_adjust(top=0.88)# po tight_layoutu je potřeba název umístit výš
subplots_adjust(wspace=.3) # Větší rozestupy mezi grafy

### Another way to do more plots

fig = plt.figure()

ax1 = plt.subplot(311)

ax2 = plt.subplot(312, sharex=ax1)  # Share axis
plt.ylabel("Error")
# Or rather  ax.set_ylabel('common ylabel')   - also for x

setp(ax2.get_xticklabels(), visible=False)  # To hide numbers on xaxis
ax3 = plt.subplot(313)
plt.ylabel("Weights")

# If interactive add   ax1.clear()
y_ref_pl, = ax1.plot(t, y_ref, label="Skutečnost"); plt.xlabel('t')
y_pl, = ax1.plot(t, y, label="Identifikovaná")
plt.legend(loc="upper right")

e_pl, = ax2.plot(t, e)

w_all_pl = ax3.plot(t, w_all)
plt.ylabel("Hodnoty vah")

### More plots with one legend

plt.figure(figsize=(12,7))
plt.subplot(3, 1, 1)
plt.plot(t, yy, label='Predikce'); plt.xlabel('t')
plt.plot(t, data, label='Skutečnost'); plt.xlabel('t')
plt.legend(loc="upper right")  # prop={'size': 6}   to change size
plt.ylabel("u4 predikované")

# Or

line_up, = plt.plot([1,2,3], label='Line 2')
line_down, = plt.plot([3,2,1], label='Line 1')
plt.legend(handles=[line_up, line_down])

### More plots with for cycle

for i in range(1,7):
plt.subplot(3, 2, i)
plt.plot(oknomean[i]);plt.grid(); plt.xlabel('t')
plt.ylabel("u{}".format(i))
plt.suptitle("Klouzavý průměr okna", fontsize=20)
plt.tight_layout()
plt.subplots_adjust(top=0.88)
plt.show()

### Scatterplot

import numpy as np
import matplotlib.pyplot as plt
N = 60
g1 = (0.6 + 0.6 * np.random.rand(N), np.random.rand(N))
g2 = (0.4+0.3 * np.random.rand(N), 0.5*np.random.rand(N))
g3 = (0.3*np.random.rand(N),0.3*np.random.rand(N))
data = (g1, g2, g3)
colors = ("red", "green", "blue")
groups = ("coffee", "tea", "water")
fig = plt.figure()
ax = fig.add_subplot(1, 1, 1, axisbg="1.0")
for data, color, group in zip(data, colors, groups):
x, y = data
ax.scatter(x, y, alpha=0.8, c=color, edgecolors='none', s=30, label=group)
plt.title('Matplot scatter plot')
plt.legend(loc=2)
plt.show()

### Text plot

import matplotlib.pyplot as plt
fig = [plt.figure](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.figure.html#matplotlib.pyplot.figure "View documentation for matplotlib.pyplot.figure")(figsize=(5, 1.5))

text = fig.text(0.5, 0.5, 'Hello path effects world!\nThis is the normal '
    'path effect.\nPretty dull, huh?',
    ha='center', va='center', size=20)

[plt.show](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.show.html#matplotlib.pyplot.show "View documentation for matplotlib.pyplot.show")()

### Log axis

plt.loglog(x, y)  # Both axis are log

'''
```

### Web

#### Requests - API - GET, POST

    rqsts = '''
    import getpass
    import requests
    username = input('Username: ')
    Username: hroncok
    password = getpass.getpass()   # Do not use...
    Password:
    r = requests.get('https://api.github.com/user', auth=(username, password))
    r.status_code
    r.headers['content-type']  # 'application/json; charset=utf8'
    r.encoding  # 'utf-8'
    r.text  # '{"login":"hroncok"...'
    r.json()  # {'avatar_url': 'https://avatars.githubusercontent.com/u/2401856?v=3', ...}

    # Session

    session = requests.Session()
    session.get('http://httpbin.org/cookies/set/mipyt/best')  #<Response [200]>
    r = session.get('http://httpbin.org/cookies')
    r.json()  # {'cookies': {'mipyt': 'best'}}
    session.headers.update({'x-test': 'true'})
    r = session.get('http://httpbin.org/headers', headers={'x-test2': 'true'})
    r.json()  # {'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate', 'Connection': 'close', 'Cookie': 'mipyt=best', 'Host': 'httpbin.org', 'User-Agent': 'python-requests/2.19.1', 'X-Test': 'true', 'X-Test2': 'true'}}
    '''

#### Beautiful soup - web scrapping

    # Stuff for soup
    soup = """
    sample_html =  '''
    <html>
        <head>
            <title>Test</title>
        </head>
        <body>
            <h1>Heading!</h1>
            <p class="major_content">Some content.</p>
            <p class="minor_content">The important information is, that the key number is 23..</p>
        </body>
    </html>
    '''

    from bs4 import BeautifulSoup

    # path to data
    path = "data/example1.html"

    # template for printing the output
    sentence = "{} {} is {} years old."

    # load data
    with open(path, 'r') as datafile:
        sample_html = datafile.read()

    # create tree
    soup = BeautifulSoup(sample_html, "html.parser")

    # get title and print it
    title = soup.find("title")
    print(title.text, "\n")

    # select all rows in table
    table = soup.find("table",  {"id": "main_table"})
    table_rows = table.findAll("tr")

    # iterate over table and print results
    for row in table_rows:
        first_name = row.find("td", {"class": "first_name"})
        last_name = row.find("td", {"class": "last_name"})
        age = row.find("td", {"class": "age"})
        if first_name and last_name and age:
            print(sentence.format(first_name.text, last_name.text, age.text))

    # Print attributes
    print(table.attrs)
    {'id': 'main_table'}

    ### Parse sentence
    import re
    print(re.search('the key number is (.*).', sample_html).group(1))  # 23

    ## Create simplest http server

    import SimpleHTTPServer
    import SocketServer

    PORT = 8000
    ADDRESS = "127.0.0.1"

    Handler = SimpleHTTPServer.SimpleHTTPRequestHandler

    httpd = SocketServer.TCPServer((ADDRESS, PORT), Handler)

    print ("Serving at port", PORT)
    httpd.serve_forever()
    """

### Images, pictures

    pctrs = '''
    from PIL import Image
    from resizeimage import resizeimage

    ## Show image

    ax1.imshow(img)

    ## Resize

    img = Image.open(im)
    img = resizeimage.resize_contain(img, [100,100]) # nebo
    img = img.resize((1250,890),Image.ANTIALIAS) # antialias

    ## Save

    img.save(soubor[0]+'_resized_'+soubor, img.format)
    img.close()

    ## Convert to black and white

    img = img.convert('L') #Převede na černobílý

    ## Convert image to matrix

    img=np.array(img)
    '''

### Mathematics, statistics, linear algebra

!! Matrix and liear algebra operations discussed in Numpy aray section !!

```python
### Display nice math

from IPython.display import display, Math, Latex
display(Math(r'F(k) = \int_{-\infty}^{\infty} f(x) e^{2\pi i k} dx'))

### Square root ###

y = 9**(1/2)  # 3

### Round ###

A = round(5.76543, 2)

### Natural log - e ###

from math import e
a = e  # 2.71

# Or

import numpy as np
a = np.exp(1)

### Modulo ###

a = 7 % 3 # => 1

### Power (x on y) ###

a = 2**4 # => 16

### Describe - Statistical values of list ###

import pandas as pd

df = pd.Series([1, 4, 6, 88])
df.describe()

    #    count     4.000000
    #    mean     24.750000
    #    std      42.216703
    #    min       1.000000
    #    25%       3.250000
    #    50%       5.000000
    #    75%      26.500000
    #    max      88.000000
    #    dtype: float64

### Statistical values of array

from scipy import stats
a = np.array([1, 3, 5, 77])
stats = stats.describe(a)

## Random
### Random numbers in normal distribution

import numpy as np
ran = np.random.randn(3)

q = np.random.randn(3, 4)

    #    [[ 1.33262386 -0.88922967 -0.07056098 0.27340112]
    #     [ 1.00664965 -0.68443807 0.43801295 -0.35874714]
    #     [ -0.19289416 -0.42746963 -1.80435223 0.02751727]]

### Random number everytime the same

np.random.seed(5)

w = np.random.randn(3)  # [ 0.44122749 -0.33087015  2.43077119]
w = np.random.randn(3)  # [ 0.44122749 -0.33087015  2.43077119]

### Random numbers in normal distribution matrix shape

s = np.random.normal(2, 6, 1000)  # Mu, Sigma,

## Points generation

t = np.linspace(0,20,1000)  # Generate numbers with the same interval (beginning, end, number)

t = np.arange(1, 3, 1)  # 1, 2
t = np.arange(3)  # 0, 1, 2

### Mean

x = np.array([[10,20,30], [40,50,60]])
np.mean(x)

### Standard deviation

std = np.std(x)

# Or

from statistics import stdev
sample = [1, 2, 3, 4, 5]
std = stdev(sample)

### Bins

bins = 10
binss = np.histogram(x, bins)  # binss[0] hodnoty binss[1]
    # original = x = np.array([[10,20,30], [40,50,60]])
    # binss[0] = counts - [1 0 1 0 1 0 1 0 1 1]
    # binss[1] = values [10. 15. 20. 25. 30. 35. 40. 45. 50. 55. 60.]

### Cumulative sum

y = np.cumsum(x)

### Derivation

sample = [1, 2, 8, 4, 5]
z = np.diff(y)
    # [ 1  6 -4  1]

### Latex dislay

# from IPython.display import display, Math, Latex
# display(Math(r'F(k) = \int_{-\infty}^{\infty} f(x) e^{2\pi i k} dx'))


########################################
####### Symbolic python - sympy #######
#######################################

import sympy as sp

#  Nice display
sp.init_printing()

# Or
# Or use sp.pprint()

### Display Latex

sp.latex(x**2)

### Create symbols

s = sp.Symbol("s")
x, y, z = sp.symbols('x,y,z')
d, e, f = sp.symbols('d:f')
inf = sp.oo

### Create one member (fraction) from more members

ans = sp.together(1/x + 1/y + 1/z)
    #   x*y + x*z + y*z
    #   ---------------
    #        x*y*z

# Imaginary numbers
x = 1 + 1 * sp.I

### Expand

ans = ((x+y)**2).expand()
    #    x**2 + 2*x*y + y**2

### Substitute

expr = sp.cos(x)
expr.subs(x, 0) # 1

### Compute string

str_expr = "x**2 + 3*x - 1/2"
expr = sp.sympify(str_expr)

### Simplify equation

simplified = sp.simplify((x**3 + x**2 - x - 1)/(x**2 + 2*x + 1))
    #    𝑥−1

### Evaluate expression

expr = sp.sqrt(8)
expr.evalf() # 2.82842712474619

### Use numerical evaluation

import numpy
a = numpy.arange(10)
expr = sp.sin(x)
f = sp.lambdify(x, expr, "numpy")
res = f(a)
    # [ 0.          0.84147098  0.90929743  0.14112001 -0.7568025  -0.95892427
    #  -0.2794155   0.6569866   0.98935825  0.41211849]

    ### Combine with numpy

y_vec = numpy.array([N(((x + pi)**2).subs(x, xx)) for xx in x_vec])  # But lambdify is faster

### Solve equation

solve(x**2 - 1, x)  # [-1, 1]
solve([x + y - 1, x - y - 1], [x,y])  # {x:1,y:0}

# Exponential
from mpmath import e
y = sp.exp(x)

# Matrices

m11, m12, m21, m22 = symbols("m11, m12, m21, m22")
b1, b2 = symbols("b1, b2")

A = Matrix([[m11, m12],[m21, m22]])
    #   [ m11    m21
    #     m12    m22 ]

b = Matrix([[b1], [b2]])
    #   [ b1
    #     b2 ]

# Matrix operations

sqr_mat = A**2
mat_mul = A*b
det = A.det()
inv = A.inv()

### Fast fourier transform

from numpy.fft import fft, fftfreq, ifft
import numpy as np

t = np.arange(0, 10, 0.1)
y = np.sin(t)

ffty = fft(y)

real_ffty = ffty.real
imag_ffty = ffty.imag

freqs = fftfreq(N, dt)  # Frequentions assigned to values - 0, 0.1, 0.2...

### Sympy plotting

from sympy.plotting import plot
p1 = plot(sp.exp(t))


######################################
####### Differential equations #######
######################################

import sympy as sp
f = sp.Function('f')
x = sp.Symbol('x')
eq = f(x).diff(x, x) + f(x)
res = sp.dsolve(eq, f(x))

###########################
####### Integration #######
###########################

### Unbounded integral

import sympy as sp
from sympy import exp as e
x = sp.Symbol("x")
f = e(-x)
disp("f=",f)
intf = sp.integrate(f)
disp("\int " + dfrac(f)+"dx=",intf) # po \int musim byt mezera

### Bounded integral

intf=sp.integrate(f,(x,0,1))
disp("\int_0^1" + dfrac(f)+" dx=",intf.evalf())

###########################
####### Correlation #######
###########################

### Correlation matrix - values

import pandas as pd
df = pd.DataFrame([[1, 2, 3], [4, 5, 9], [7, 8, 78]], columns=['one', 'two', 'three'])
x_cor = df.corr()

    #               one      two    three
    #    one    1.00000  1.00000  0.89977
    #    two    1.00000  1.00000  0.89977
    #    three  0.89977  0.89977  1.00000

### Correlation matrix - plot

# a = pd.plotting.scatter_matrix(df, figsize=(16,12), alpha=0.3)

### Correlation (Pearson corr. matrix)

import matplotlib.pyplot as plt
scoreTable = df.corr(method='pearson')
df.corr(method='pearson').style.format("{:.2}").background_gradient(cmap=plt.get_cmap('coolwarm'), axis=1)

### Correlation coefficent

ar = np.array([1, 2, 4, 5, 9])
ar2 = np.array([7, 8, 78, 34, 2])
a = np.corrcoef(ar, ar2)

    #    [[ 1.        -0.0475504]
    #     [-0.0475504  1.       ]]

##########################
####### Statistics #######
##########################

### Create combinations

from scipy.special import binom
a = binom(5,2)

### Test of normal distribution

from statsmodels.stats.stattools import jarque_bera
residuals = [1, 3, 5, 2, 4]
score, pvalue, _, _ = jarque_bera(residuals)
if pvalue < 0.10:
    print ('We have reason to suspect the residuals are not normally distributed.')
else:
    print ('The residuals seem normally distributed.')

# Machine learning

#################################################
####### Standardization and normalization #######
#################################################

### Standardization: mean = 0 and std = 1

from sklearn import preprocessing
import numpy as np
X_train = np.array([[ 1., -1.,  2.],
                    [ 2.,  0.,  0.],
                    [ 0.,  1., -1.]])

scaler = preprocessing.StandardScaler()

fitted_scaler = scaler.fit(X_train)
scaled = fitted_scaler.transform(X_train)

    #    [[ 0.  ..., -1.22...,  1.33...],
    #     [ 1.22...,  0.  ..., -0.26...],
    #     [-1.22...,  1.22..., -1.06...]]

# inverse transformation
back = fitted_scaler.inverse_transform(scaled)

### Normalization (-1,1)
# Just replace one row from standard scaler
scaler = preprocessing.MinMaxScaler(feature_range=(0, 1))

### The same way you can use preprocessing.RobustScaler, that is not as much influenced by outliers !!
```

### Signal processing and controll

```python
## Signal

import scipy.signal as sig
y = sg.sawtooth(2*pi/100*t,0.5)  # Create sawtooth signal

### Chirp signal
t = np.linspace(0, 10, 5001)
w = sig.chirp(t, f0=6, f1=1, t1=10, method='linear') # (t, f0, t1, f1)  # Changing frequency

## Hanning window

w = hanning(N)

## Tuckey window

w = sig.tukey(N)

## Fourier transform

dt = 1./1024
t = np.arange(0,2,dt)
N = len(t)
y = sig.sawtooth(2*np.pi/T1*t,0.5)

freq = np.fft.fftfreq(N,dt)  # Return frequencies
ffty = np.fft.fft(y)
rffty = np.real(ffty)  # Real part
iffty = np.imag(ffty)  # Imaginary part

## Inverse Fourier transform

y_back = np.fft.ifft(ffty)

## Power spectral density

PSD = (abs(ffty)**2)/N

## Hilbert transform - Envelope, phase, frequency

analytic_signal = hilbert(signal)
amplitude_envelope = np.abs(analytic_signal)
instantaneous_phase = np.unwrap(np.angle(analytic_signal))
instantaneous_frequency = (np.diff(instantaneous_phase) / (2.0*np.pi) * fs)

### Filtering

import scipy.signal as sig
from scipy.signal import butter,filtfilt

t = np.linspace(0, 10, 501)
w = sig.chirp(t, f0=6, f1=0.1, t1=10, method='linear') # (t, f0, t1, f1)

Wn = 0.1  # zkuste pozorovat efekt nasobici konstanty na cinnost filtru
filter_order = 2
b, a = butter(filter_order, Wn, 'high', analog=False)  #Matlab-style filter design

x_dem = sig.lfilter(b, a, w)

# Or
x_dem = filtfilt(b, a, w)  # Apply  filtr (forward and backward)

# You can use scipy or you can use controll

####################
##### Controll #####
####################

## State-space representation

A = [[0. , 1.], [-1., -1.]]
B = [[0.], [1.]]
C = [1. , 0.]
D = 0.

import control as ct

sys = ct.ss(A , B, C, D)


## Transfer function

g = ct.tf(1 ,[1 ,1, 1])  # Or ct.tf(sys)

    #        1
    #   -----------
    #   s^2 + s + 1

sys = ct.ss(g)  # Convert back from transfer to state space

## Convert from continuous to discrete

g = ct.tf(1, [1, 1, 1])
gd = ct.c2d (g, 0.01)

    #   4.983e-05 z + 4.967e-05
    #   -----------------------
    #     z^2 - 1.99 z + 0.99

## Interconnect systems

### Parallel


    #      2 s + 3
    #    -------------
    #    s^2 + 3 s + 2


#################
##### Scipy #####
#################

# You have to use float here. Not working for int...

from scipy import signal

sys = signal.StateSpace(A, B, C, D)

### Step response

t1, y1 = signal.step(sys)

################
### Simulate ###
################

t = np.linspace(0, 100, 101)
u = np.zeros(len(t))
u[10:50] = 1.0;  u[50:] = 2.0

t3, y3, x3 = signal.lsim(sys, u, t)

import numpy as np
from scipy.integrate import odeint
import matplotlib.pyplot as plt

# function that returns dy/dt
def model(y,t):
    k = 0.3
    dydt = -k * y
    return dydt

# initial condition
y0 = 5

# time points
t = np.linspace(0,20)

# solve ODE
y = odeint(model,y0,t)

# plot results
plt.plot(t,y)
plt.xlabel('time')
plt.ylabel('y(t)')
plt.show()

### Simulate with time dependent input

def fdxdt(x,t,u,Omega,eta,b0,b1):    # x=[x1 x2 ... xn]
        dx1dt=-Omega0**2*x[1]-b0*u
        dx2dt=-2*eta*Omega0*x[1]-b1*u+x[0]
        return(dx1dt,dx2dt)

    dt=.21  #[sec]
    t=arange(0,50,dt) ; N=len(t)  # delka dat
    Npul=int(N/2)   #konverze na integer

    u=sin(2*pi/10*t) ; u[Npul+1:]=sign(u[Npul+1:])

    u=u*1

    figure(figsize=(12,4));grid()
    plot(t,u,'-*',label="u(t)");xlabel("t");legend();title("$Vstup \ u(t) \ se \ meni  \ se \
            \ vzorkovaci \ periodou \ \Delta t $="+str(dt) +" [sec] \n")
    show()

    from scipy.integrate import odeint

    Omega0=10  ;  eta=.1  ;   b0=Omega0**2  ;  b1=0

    #===============================

    y=zeros(N)
    x10=0 ; x20=0  # poc. podm

    x0=[x10,x20]

    for i in range(0,N-1):
        tt=[t[i],t[i+1]]  # [t1 t2]
        x=odeint(fdxdt,x0,t,(u[i],Omega0,eta,b0,b1)) #returns x=[ [x1(t1) x2(t1)] [x1(t2) x2(t2)]]
    #    x=odeint(fdxdt,x0,tt,args=(u[i],)) # <-- pokud je jen jeden extra argument, musi se tak
        y[i+1]=-x[1,1]
        x0=x[1,:]  # jako nove poc. podm pro dalsi integraci

    figure(figsize=(14,6))
    grid()
    plot(t,y,"-*",label="y(t)...simulace scipy.integrate.odeint"),xlabel("t [sec]"),legend()
    show()
```

### Database

#### pyodbc, sqlalchemy

**Read**

```python
server = 'SERVER={};'.format(server)
database = 'DATABASE={};'.format(database)
sql_params = r'DRIVER={ODBC Driver 13 for SQL Server};' + server + database + 'Trusted_Connection=yes;'

sql_conn = pyodbc.connect(sql_params)
df = pd.read_sql(query, sql_conn)
```

**Write**

from sqlalchemy import create_engine
import urllib

params = urllib.parse.quote_plus(r'DRIVER={driver};SERVER={server};DATABASE={database};Trusted_Connection=yes'.format(driver=r'{SQL Server}', server=server, database=database))
conn_str = 'mssql+pyodbc:///?odbc_connect={}'.format(params)

engine = create_engine(conn_str)
dataframe_to_sql.to_sql(name='FactProduction', con=engine, schema='Stage', if_exists='append', index=False)

### GUI

**VUE and EEl (Electron JS like library)**

You can use mypythontools pyvueeel for building an app with graphical interface. It contains working examples.

### Misc
#### Tables

    from prettytable import PrettyTable
    models_table = PrettyTable()
    models_table = PrettyTable().field_names = ["City name", "Area", "Population", "Annual Rainfall"]
    models_table = PrettyTable().add_row(["Adelaide", 1295, 1158259, 600.5])
    print (models_table)

### Jupyter, IPython
```
jupyter kernelspec list  # List available kernels
jupyter kernelspec uninstall nazev  # Uninstall kernel

# Install jupyter extensions
pip install jupyter_contrib_nbextensions && jupyter contrib nbextension install

jupyter nbextension enable --py rise  # Enable extension

```

**Run with ipython from python**
```
# Test if running on jupyter

if 'ipykernel' in sys.modules:
    print('jup')

# Or

# if hasattr(builtins, '__IPYTHON__'):
#     print('IPython')
# else:
#     print('Nope')

## If jupyter and run from normal python

try:
    __IPYTHON__

    from IPython.terminal.embed import InteractiveShellEmbed
    ipshell = InteractiveShellEmbed()
    ipshell.dummy_mode = True
    ipshell.magic("%load_ext autoreload")
    ipshell.magic("%autoreload")

except NameError:
    print('No Jupyter')

## Run ipython in normal python

from IPython.terminal.embed import InteractiveShellEmbed

ipshell = InteractiveShellEmbed()
ipshell.dummy_mode = True
ipshell.magic("%timeit abs(-42)");
```

#### Magic
**Autoreload**

Reload all modules imported with %aimport every time before executing the Python code typed, so it is not necessary to reload kernel everytime some other file changed or new library wa installed
  
    %load_ext autoreload
    %autoreload  # Reload all modules (except those excluded by %aimport) automatically now.
    %autoreload 0  # Disable automatic reloading.
    %autoreload 1  # Reload all modules imported with %aimport every time before executing the Python code typed.
    %autoreload 2  # Reload all modules (except those excluded by %aimport) every time before executing the Python code typed.
    %aimport  # List modules which are to be automatically imported or not to be imported.
    %aimport foo  # Import module ‘foo’ and mark it to be autoreloaded for %autoreload 1
    %aimport -foo  # Mark module ‘foo’ to not be autoreloaded.

#### Misc
**Youtube**

    # from IPython.display import YouTubeVideo
    # YouTubeVideo('7VeUPuFGJHk')

**Show all images from folder**

    import os
    from IPython.display import display, Image
    names = [f for f in os.listdir('../images/ml_demonstrations/') if f.endswith('.png')]
    for name in names[:5]:
        display(Image('../images/ml_demonstrations/' + name, width=100))

**Jupyter themes**

    pip install jupyterthemes
    jt -t monokai -T

    # Deactivate
    jt -r

**Matplotlib widget interactive backend**

    %matplotlib widget
    from matplotlib import pyplot as plt
    fig, ax = plt.subplots()
    ax.plot([1,2,3], [4,3,2])
    # fig.show() is not necessary

    # If not working in jupyterlab, use in terminal
    #     jupyter labextension install @jupyter-widgets/jupyterlab-manager

    # Instead of magic you can use

    import matplotlib
    matplotlib.use('module://ipympl.backend_nbagg')

**Dark background of matplotlib for dark themes**

    # ! pip install jupyterthemes

    In ~/.ipython/profile_default/startup  create startup.py file with

    import matplotlib.pyplot as plt
    from jupyterthemes import jtplot
    jtplot.style(theme='monokai', context='notebook', ticks=True, grid=False)

    # Then in extensions use theme-darcula
    # And in to ~\AppcmdData\Local\Programs\Python\Python37\Lib\site-packages\jupyterthemes\styles\monokai.less
    # Change on
    /* jtplot figure style */
    @axisFace:              #2b2b2b;
    @figureFace:            #2b2b2b;

## Building app (executables) - Pyinstaller

You can create binaries with pyinstaller, so other user can run an app even with no python installed.

You don't even need to install pyinstaller and you can use mypythontools build module (you can use VS Code Task to build an app with single click)

[Documentation](https://mypythontools.readthedocs.io/mypythontools.build.html) for how to do it.

Build bootloader locally to avoid false positive antivirus alert (in tutorial).

## Performance

### Profiling

#### Line profiling

Simplest and probably best way is to use jupyter, extensions and magic.
Simple example is in project-starter test profiling.ipynb [here](https://github.com/Malachov/mypythontools/tree/master/content/project-starter/tests)

If you want to use just python, you can

    import line_profiler

    def hoj():
        a = 2
        b = [1,2,3]

        return a * b

    lp = line_profiler.LineProfiler()
    lp_wrapper = lp(hoj)()
    lp.print_stats()

    # If inner function

    from line_profiler import LineProfiler
    import random

    def do_other_stuff(numbers):
        s = sum(numbers)

    def do_stuff(numbers):
        do_other_stuff(numbers)
        l = [numbers[i]/43 for i in range(len(numbers))]
        m = ['hello'+str(numbers[i]) for i in range(len(numbers))]

    numbers = [random.randint(1,100) for i in range(1000)]
    lp = LineProfiler()
    lp.add_function(do_other_stuff)   # add additional function to profile
    lp_wrapper = lp(do_stuff)
    lp_wrapper(numbers)
    lp.print_stats()

**Other profiling option**

    # !  pip install snakeviz
    # !  python -m cProfile -o program.prof my_program.py
    # !  snakeviz program.prof

**Show memory profile to file**

    from memory_profiler import profile

    def fu(a):
        print('content')

    memory_profile = open('memory_profiler.log','w+')
    profile_wrapped_function = profile(fu, stream=memory_profile)
    profile_wrapped_function()

    memory_profile.seek(0)
    print(memory_profile.read())  # Or save to variable
    memory_profile.close()

### Max execution time function

```python
import time, sys

def watchdog(timeout, code, *args, **kwargs):
    "Time-limited execution."
    def tracer(frame, event, arg, start=time.time()):
        "Helper."
        now = time.time()
        if now > start + timeout:
            raise WatchdogTimeoutError(start, now)
        return tracer if event == "call" else None

    old_tracer = sys.gettrace()
    try:
        sys.settrace(tracer)
        code(*args, **kwargs)
    finally:
        sys.settrace(old_tracer)

def func(a):
    return 1 + a

watchdog(5, func, 0.1) # First element is time limit - Last argument 0.1 are *args and **kwargs

# Or
mx_ex = '''
from contextlib import contextmanager
import threading
import _thread

class TimeoutException(Exception):
    def __init__(self, msg=''):
        self.msg = msg

@contextmanager
def time_limit(seconds, msg=''):
    timer = threading.Timer(seconds, lambda: _thread.interrupt_main())
    timer.start()
    try:
        yield
    except KeyboardInterrupt:
        raise TimeoutException("Timed out for operation {}".format(msg))
    finally:
        # if the acti   on ends in specified time, timer is canceled
        timer.cancel()

import time
with time_limit(5, 'slee'):
    time.sleep(10)
'''
```

### Numba

```python
import numba as nb

@nb.jit(parallel=True)  # Or use  parallel=True
def func():
    return 1

# More options possible
@nb.njit()
def func():
    return 1

# Or

@nb.jit(nopython=True)  # Or use  parallel=True, nogil=True, cache=True
def func():
    return 1

# Or

@nb.vectorize
def function(a, b):
    return 1

# Or direct data formats

import numba as nb
@nb.jit(nb.int32(nb.int32, nb.int32))
def function(a, b):
    return a + b

### Get numba dtype from other

numba.typeof(np.empty(3))
array(float64, 1d, C)

### Evaluate chunk sizes

    i = numba.cuda.grid(1)
    while i < x.size:
        # Do something
        i += numba.cuda.grid(1)

### Cuda in Numba ###

import math
import numpy as np
from numba import cuda
import matplotlib.pyplot as plt
%matplotlib inline

len(cuda.gpus)  # 1

cuda.gpus[0].name  # b'GeForce GTX 980M'

@cuda.jit
def mandelbrot_numba(m, iterations):
    # Matrix index.
    i, j = cuda.grid(2)
    size = m.shape[0]
    # Skip threads outside the matrix.
    if i >= size or j >= size:
        return
    # Run the simulation.
    c = (-2 + 3. / size * j +
        1j * (1.5 - 3. / size * i))
    z = 0
    for n in range(iterations):
        if abs(z) <= 10:
            z = z * z + c
            m[i, j] = n
        else:
            break

    size = 40
    iterations = 5

    m = np.zeros((size, size))

    # 16x16 threads per block.
    bs = 16
    # Number of blocks in the grid.
    bpg = math.ceil(size / bs)
    # We prepare the GPU function.
    f = mandelbrot_numba[(bpg, bpg), (bs, bs)]

    f(m, iterations)

# Or

from numba import cuda
@cuda.jit(device=True)
def function(a, b):
    return a + b
```

### Dask

```python
dsk = '''
import dask

@dask.delayed
def inc(x):
    return x + 1

@dask.delayed
def double(x):
    return x + 2

@dask.delayed
def add(x, y):
    return x + y

data = [1, 2, 3, 4, 5]

output = []
for x in data:
    a = inc(x)
    b = double(x)
    c = add(a, b)
    output.append(c)

total = dask.delayed(sum)(output)
# maybe add    total.compute()
'''
```

**Dask formats**

    dsk_frmts = '''
    import pandas as pd
    import dask.array as da
    import dask.dataframe as dd
    x = da.random.random((20, 20), chunks=(10, 10))
    res = x.dot(x.T).sum()
    res.compute()

    # Dataframes implement the Pandas API

    df = pd.DataFrame([[1, 2, 3], [1, 5, 3], [5, 6, 7]], columns = ['one', 'two', 'three'])
    dfdsk = dd.from_pandas(df, npartitions=1)

    # Or dd.read_csv('adress')

    from dask_ml.linear_model import LogisticRegression  # Dask-ML implements the Scikit-Learn API
    lr = LogisticRegression()
    lr.fit(dfdsk, dfdsk)

    ## Show task structure

    .visualize()
    '''

## Garbage collector

**Force to empty memory**

    import gc
    gc.collect()


## Miscellaneous

## Misc

### Snippets - examples

**Measure time**

```python
"""
import timeit
import functools

def func(*args):
    a = 1
    return a
t = timeit.Timer('''
import numpy as np
a = 12
''', 'gc.enable()')
print(t.timeit(3))  # 3 - measure three times

# Or you can use
t = timeit.timeit(func)

# Measure time of function with inputs
t = timeit.Timer(functools.partial(func, (1, 2)))
print (t.timeit(20))
"""
```

**Show bytecode**

    import dis
    dis.dis("dict()")

        #              0 LOAD_NAME                0 (dict)
        #              2 CALL_FUNCTION            0
        #              4 RETURN_VALUE

**Encoding JSON with Python**

    import json

    data =  {
                'a':  0,
                'b':  9.6,
                'c':  "Hello World",
                'd':  {
                        'a':  4
                }
    }

    json_data = json.dumps(data)

    # Notice how the keys are not sorted by default, you would have to add the sort_keys=True

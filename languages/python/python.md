# Python reference

## Table of Contents

- [Python reference](#python-reference)
  - [Table of Contents](#table-of-contents)
  - [Dedicated modules](#dedicated-modules)
  - [General](#general)
    - [Common terminal commands](#common-terminal-commands)
  - [Environment management](#environment-management)
    - [uv](#uv)
      - [pyproject.toml configuration](#pyprojecttoml-configuration)
    - [Python version management](#python-version-management)
      - [Install particular version](#install-particular-version)
      - [Select default global version](#select-default-global-version)
    - [Virtual environment (venv)](#virtual-environment-venv)
    - [Package management](#package-management)
    - [Requirements.txt](#requirementstxt)
  - [Style guide (linting, formatting)](#style-guide-linting-formatting)
  - [Documentation - docstrings](#documentation---docstrings)
      - [Google](#google)
      - [Numpy](#numpy)
      - [reStructured text](#restructured-text)
  - [Project scaffolding - Cookiecutter](#project-scaffolding---cookiecutter)
  - [Syntax](#syntax)
    - [Multi line code](#multi-line-code)
    - [Comments](#comments)
    - [Logical conditions (and, or, not...)](#logical-conditions-and-or-not)
      - [Not](#not)
      - [Greater than, lower than](#greater-than-lower-than)
      - [And, or](#and-or)
    - [Typing - Type hints](#typing---type-hints)
    - [Variables](#variables)
    - [Builtin Data types](#builtin-data-types)
      - [Type of variable as condition](#type-of-variable-as-condition)
      - [None](#none)
      - [String](#string)
        - [Format - deprecated](#format---deprecated)
        - [f-strings - Correct way to format](#f-strings---correct-way-to-format)
        - [Eval - String to code](#eval---string-to-code)
        - [Join - concatenate](#join---concatenate)
      - [List](#list)
      - [Tuple](#tuple)
      - [Dictionary](#dictionary)
        - [Two dictionaries intersection](#two-dictionaries-intersection)
      - [Deque](#deque)
      - [Set](#set)
      - [Decimal](#decimal)
    - [Iterators](#iterators)
    - [Conditions (if, else...)](#conditions-if-else)
    - [Loops](#loops)
      - [For](#for)
      - [While](#while)
    - [Functions](#functions)
    - [map, filter, reduce](#map-filter-reduce)
    - [Generators](#generators)
    - [Decorators](#decorators)
      - [Use decorator with condition](#use-decorator-with-condition)
    - [Modules](#modules)
    - [Classes](#classes)
    - [Magic methods (dunder methods)](#magic-methods-dunder-methods)
      - [Repr](#repr)
    - [Bitwise operations](#bitwise-operations)
    - [File I/O](#file-io)
      - [Common file and import snippets](#common-file-and-import-snippets)
      - [Open file](#open-file)
      - [Manipulate with files (move, copy)](#manipulate-with-files-move-copy)
    - [Try - Except](#try---except)
    - [Builtin functions](#builtin-functions)
    - [Builtin variables](#builtin-variables)
      - [\_\_name\_\_](#__name__)
    - [Builtin modules](#builtin-modules)
      - [Sys, io, os quick snippets](#sys-io-os-quick-snippets)
      - [Pathlib - Work with paths](#pathlib---work-with-paths)
      - [Warnings](#warnings)
      - [re - Regular expressions](#re---regular-expressions)
      - [Subprocess - Run shell commands](#subprocess---run-shell-commands)
      - [Pickle](#pickle)
      - [Time and datetime](#time-and-datetime)
      - [Argparse - Command Line Interface (cli)](#argparse---command-line-interface-cli)
    - [Asynchronicity and parallelism](#asynchronicity-and-parallelism)
      - [IO bound, CPU bound](#io-bound-cpu-bound)
      - [Concurrent futures](#concurrent-futures)
      - [Multiprocessing](#multiprocessing)
      - [Asyncio](#asyncio)
    - [3rd party libraries](#3rd-party-libraries)
      - [Documentation](#documentation)
        - [MkDocs](#mkdocs)
        - [Sphinx - Create documentation](#sphinx---create-documentation)
      - [Tests](#tests)
        - [Pytest](#pytest)
      - [Plots, graphs](#plots-graphs)
        - [Plotly](#plotly)
        - [Matplotlib](#matplotlib)
      - [Web](#web)
        - [HTTPX - API - GET, POST (sync + async)](#httpx---api---get-post-sync--async)
        - [Beautiful soup - web scraping](#beautiful-soup---web-scraping)
      - [Images, pictures](#images-pictures)
      - [Database](#database)
        - [pyodbc, sqlalchemy](#pyodbc-sqlalchemy)
      - [Misc](#misc)
        - [Tables](#tables)
    - [GUI](#gui)
      - [Tkinter](#tkinter)
      - [VUE and EEl (Electron JS like library)](#vue-and-eel-electron-js-like-library)
  - [Building app - executables](#building-app---executables)
    - [Pyinstaller](#pyinstaller)
  - [Performance](#performance)
    - [Profiling](#profiling)
      - [Line profiling](#line-profiling)
      - [Other profiling option](#other-profiling-option)
      - [Show memory profile to file](#show-memory-profile-to-file)
    - [Max execution time function](#max-execution-time-function)
    - [Numba](#numba)
    - [Dask](#dask)
      - [Dask formats](#dask-formats)
  - [Garbage collector](#garbage-collector)
      - [Force to empty memory](#force-to-empty-memory)
  - [Miscellaneous](#miscellaneous)
  - [Misc](#misc-1)
    - [Snippets - examples](#snippets---examples)
      - [Measure time](#measure-time)
      - [Show bytecode](#show-bytecode)
      - [Encoding JSON with Python](#encoding-json-with-python)


## Dedicated modules

There are dedicated knowledge files for

- Data science (e.g. pandas and numpy)
- Machine learning (e.g. scikit-learn, pytorch)


## General

### Common terminal commands

```shell
# Show where Python is installed
# Windows:
where python
# Linux:
which python

# Use `python` and `pip` aliases for Python 3 (add to ~/.bashrc)
alias python=python3
alias pip=pip3
```

## Environment management

In this document, environment management covers Python version management, package management, and virtual environments.

Prefer `uv` when possible. Alternative methods are kept for backward compatibility.

### uv

`uv` is a modern Python toolchain manager. It can install specific Python versions, create virtual environments, manage dependencies quickly, and produce reproducible lock files.

It can replace tools like `virtualenv` and `pip` in most workflows. For many commands, you can use the same mental model as `pip`.

Particular commands will be shown in particular sections.

```shell
# If you use private index-url, logs in
uv auth

# Updates uv
uv self update

# Manage python versions (install, uninstall)
uv python list
uv python install 3.14
uv python pin 3.14

# Shows which interpreter (and venv) is currently used with uv
uv python find
```

If you have more python sources in a project, you should use UV workspaces. This means, that you create one pyproject.toml in your root as well as pyproject.toml for each source. You have one central .venv in your root.

```python
# Create virtual environment (flags: -p 3.13)
uv venv

# Adds package to pyproject.toml, installs it to venv and updates lock
uv add numpy

# If using workspaces
uv add --package app httpx2

# Install libraries to current venv. Beware that you should usually rather use uv add over uv pip
uv pip install numpy
uv pip remove numpy

# Install all libs from requirements.txt or pyproject.toml
uv pip install -r path-to-file

# Install 

# Install current library
uv pip install .

# Editable install (use local files in dynamic way - Good for custom libraries)
uv pip install -e path-to-lib

# It installs packages including dev and default groups(much faster than pip), provides true dependency locking (including transitive dependencies), and automatically selects the correct venv.
uv sync

# Possible kwargs:
#  --all-extras
#  --extra XXX
#  --all-groups
#  --group XXX

# For getting production venv with
uv sync --no-dev --no-default-groups

# If you work in monorepo and having more pyproject.toml files, you can use workspaces.
uv sync --all-packages
uv sync --package app

# Updates lock with newer versions. You still need to use sync afterwards
uv lock --upgrade

# Push library to index-url
uv publish

# Ensure up to date venv (creates it if needed)
uv run command
```

#### pyproject.toml configuration

```toml
[tool.uv]
default-groups = ["dev"]


[[tool.uv.index]]
name = "index-name"
url = "https://index-name/path"
default = true

[tool.uv.workspace]
members = ["services/*"]

[tool.uv.sources]
app = { workspace = true }
```

If index name is artifactyory, then use these ENVs

UV_INDEX_ARTIFACTORY_USERNAME=xxx
UV_INDEX_ARTIFACTORY_PASSWORD=xxx


### Python version management

#### Install particular version

On Windows, use the official installer. On Linux:

    sudo apt-get install python3.13-dev

#### Select default global version

**PATH precedence:** The first `python` executable found in `PATH` is used. Put your preferred interpreter or venv path first.

**update-alternatives:**

```shell
sudo apt-get install update-alternatives
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.8 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.6 2
sudo update-alternatives --config python
```

### Virtual environment (venv)

A virtual environment is an isolated Python interpreter with its own installed packages. It is usually created as `.venv` in the repository root and added to `.gitignore`.

```shell
# Create new virtual environment (recommended)
python -m venv venv

# Activate virtual environment (Windows)
venv\Scripts\activate.bat

# Activate virtual environment (Linux/macOS)
source venv/bin/activate
```

### Package management

```shell
# Install library with pip
pip install library_name

# Install library with conda
conda install library_name

# If conda channel resolution fails
conda install -c anaconda library_name

# Show installed / outdated libraries
pip list
pip list --outdated

# If pip install fails due to SSL/trust issues
pip install ipykernel --upgrade pip --trusted-host pypi.org
```

### Requirements.txt

`requirements.txt` lists project dependencies and allows one-shot installation. For new projects, prefer `pyproject.toml` with a lock file.

```shell
# Install all dependencies from requirements file
pip install -r /path/to/requirements.txt

# Generate requirements from imports in a project
pipreqs --encoding=utf8 C:\VSCODE\Diplomka

# Snapshot currently installed packages (environment-dependent)
pip freeze > requirements.txt
```

You can find much more about libraries in `Modules` section.

## Style guide (linting, formatting)

**pylint** - show you where problems are

**Black** - Strict auto formatting. Setup longer default line length for better user experience

Example for how to use it in VS Code - Add to settings.json:

```
"editor.formatOnSave": true,
"editor.defaultFormatter": "ms-python.black-formatter",
"black-formatter.args": ["--line-length", "110"],
```

If you are not sure how to format code, you can try [pep 8](https://www.python.org/dev/peps/pep-0008/) or [google style guide](https://google.github.io/styleguide/pyguide.html)

## Documentation - docstrings

Common docstring styles: ReST, NumPy, and Google.

#### Google

[Sphinx example](https://www.sphinx-doc.org/en/master/usage/extensions/example_google.html)

```python
"""One sentance overview.

Long description...

Examples:
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

#### Numpy

[Sphinx example](https://www.sphinx-doc.org/en/master/usage/extensions/example_numpy.html)

#### reStructured text

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

## Project scaffolding - Cookiecutter

Cookiecutter clones a template, replaces placeholders with your values, and can run project hooks.

    cookiecutter local-repo-or-url

Use --checkout flag with branch name if you don't want main.

## Syntax

### Multi line code

```python
# This is one string
long_string = (
    "Lorem ipsum long "
    "Lorem ipsum long"
    "Lorem ipsum long."
)
```

```python
poi = {
    "1": 3,
    "2": 4,
    "6": 8,
}
```

### Comments

```python
# One-line comment

# Multi-line comments are usually multiple one-line comments.
# Triple-quoted strings are primarily docstrings.
"""Use triple-quoted strings for module, class, and function documentation."""
```

### Logical conditions (and, or, not...)

#### Not

    not True # => False
    'a' is not 1
    'a' != 1

#### Greater than, lower than

    >=

#### And, or

```python
0 and 2 # => 0
-5 or 0 # => -5
0 == False # => True
if 2 > 1 and 5 > 6:
    pass
```

    # Logical conditions can be combined

    1 < 2 < 3 # => True

### Typing - Type hints

```python
from typing import Union

# Union type is defined by |

def sentence_has_animal(sentence: None | str) -> bool:
    if not sentence:
        return False
    return "animal" in sentence


def invert_dict(d: dict[str, str]) -> dict[str, str]:
    return {v: k for k, v in d.items()}

# list[int]: list of optional number of integers
def sum_list(numbers: list[int]) -> int:
    return sum(numbers)

# tuple[str, int]: tuple with a string and an int (fixed count)
def describe_person(person: tuple[str, int]) -> str:
    name, age = person
    return f"{name} is {age} years old."

# set[float]: set of floats
def average(values: set[float]) -> float:
    return sum(values) / len(values)
```

### Variables

```python
y = int(2.8)

x = 1
x += 1 # Short form of x = x + 1. Note: x++ does not exist in Python.

# Int divided by int

3 / 2  # = 1.5
3 // 2  # = 1

# Declaration of more variables at once

a = b = c = 1  # If i change one, it doesn't affect the others

a,b,c = 1,2,"john"

# Swap variables values

e = 2; d = 4
e, d = d, e # d is now 5, e is now 4

# Bulk create variables

name = ['mike', 'john', 'steve']
age = [20, 32, 19]

for x,y in zip(name, age):
    globals()[x] = y  # mike = 20 ...

uname = ['u{}'.format(n) for n in range(7)] # [u1, u2, u3...]

# Check if variable exist

if 'myVar' in locals():
    pass

# Import variables from other modules

# Creat config.py, There for example

x = 1

# In main then use

config.x = 2 ....

# Find name of variable, list or dictionary

var = 3
my_var_name = [k for k,v in globals().items() if v == var][0]
```

### Builtin Data types

```python
a = type(var)  # Return the runtime type
```

#### Type of variable as condition

```python
if isinstance(var, str):
    pass

import numpy as np
y = np.array([1])
isinstance(y, (np.ndarray, np.generic))

if type(a) is list:  # exact type check
    pass
```

#### None

```python
None  # None singleton object

# Prefer identity comparison
None is None  # True

# Falsy values
bool(0)   # False
bool("")  # False
bool([])  # False
bool({})  # False
```

#### String

```python

a = "This is string."
a = 'This is also string.'

# In python 3

print('strings are now utf-8 \u03BCnico\u0394e!')  # strings are now utf-8 μnicoΔe!

# Strings can use + but don't use

a = "Hello " + "world!" # => "Hello world!"

# Can be concatenated without '+'

a = "Hello " "world!" # => "Hello world!"

# String is list of symbols

a = "This is list"[0] # => 'T'
```

##### Format - deprecated

```python
# Old

a = '%s  %s' % ('one', 'two')

# Newer

a = '{} {}'.format('one', 'two')

# Format can be used multiple times

a = "{0} {1} rockets flew over {0} {1} roofs".format("three hundred thirty-three", "silver")

# You can use named arguments

a = "{name} had {meal}".format(name="Frank", meal="goulash") # => "Frank had goulash"
```

##### f-strings - Correct way to format

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

##### Eval - String to code

```python
# Avoid eval/exec for untrusted input.
# Prefer ast.literal_eval for parsing Python literals safely.
import ast

value = ast.literal_eval("[1, 2, 3]")
```

##### Join - concatenate

```python
words = ["this", "is", "a", "list", "of", "strings"]
" ".join(words)  # "this is a list of strings"
```

#### List

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

# Create with comprehension
u_list = [f"u{n}" for n in range(1, 6)]

# Access and slices
lst = [1, 2, 3, 4]
lst[0]      # 1
lst[-1]     # 4
lst[1:3]    # [2, 3]
lst[::2]    # [1, 3]

# Common operations
len(lst)
sum(lst)
min(lst)
max(lst)
1 in lst

# Copy before mutation
list_a = [1, 2, 3]
list_b = list_a.copy()
list_b[2] = 5

# Deduplicate and reverse
deduplicated = list(set([1, 2, 3, 1, 2]))
reversed_list = lst[::-1]

# Zip and elementwise operations
list_1 = [1, 2, 3]
list_2 = [2, 3, 4]
pairs = list(zip(list_1, list_2))
summed = [a + b for a, b in zip(list_1, list_2)]

# Filtering and conditional comprehensions
evens = [x for x in range(6) if x % 2 == 0]
defaults = [x if x else 2 for x in [0, 1, 0, 3]]

# Nested list access
nested = [[1, 2, 3], [11, 12, 13], [21, 22, 23]]
nested[1][1]                # 12
first_column = list(zip(*nested))[0]

# Count occurrences
from collections import Counter
Counter(["a", "b", "c", "a", "b", "b"])
```

#### Tuple

Tuple is like a list, but immutable.

    tuple = (1, 2, 3)
    tuple[0]  # => 1
    # tuple[0] = 3  # Raise TypeError

    a, = 5  # (5)

#### Dictionary

```python
empty_dic = {}
dic = {"one": 1, "two": 2, "three": 3}

# Dict comprehension
dic_variable = {key:value for (key,value) in dic.items()}

# Add value
dic['four'] = 4  # If key is already there it's updated

# Add more values at once
dic.update({"four": 4, "five": 5})


# Create dictionary from two lists
name = ['mike', 'john', 'steve']
age = [20, 32, 19]
dic=dict(zip(name, age))

# Assign multiple keys to one value
my_dict = dict.fromkeys(['a', 'b', 'c'], 10)

# Miscellaneous
thisdict = {'b':1, 'c':2, 'd':3}
del thisdict['b']  # delete list
'e' in thisdict  # returns False
thisdict.items()  # returns [('a', 1), ('c', 'eggs')]

# All keys
dic.keys()

# You need list sometimes not iterables
list(dic.keys())

# Last key in dictionary
max(dic)

# For cycle for all keys
for s in dic:
    print(s)

# Maximum value and its index
stats = {'a':1000, 'b':3000, 'c': 100}
maxname = max(stats, key=stats.get)
maxvalue = stats[maxname]

# Values
list(dic.values()) # => [3, 2, 1]
"one" in dic # => True if key is in dictionary
dic.get("four") # => None - don't raise error if key not in dic
dic.setdefault("five", 5) # dic["five"] default 5

# Find key from value
list(stats.keys())[list(stats.values()).index(100)]

# For cycle for dictionaries
for k in stats: # Iterates over all keys
    print(k)

for k, v in stats.items(): # Iterates over all keys and values
    print(k,v)

# Join two dictionaries
c = {**dic, **stats}

# Enumerate in dictionaries
for i, (j, k) in enumerate(dic.items()):
    pass

# Dictionary as arguments into function
def rep(*nonamed, **named):
    return nonamed, named

t = (47,11)
d = {'x':'extract','y':'yes'}
rep(*t, **d) # It's the same as f(47, 11, x=extract, y=yes)

# Nested dictionaries - for examples name of functions and it's parameters
def rep2(*inp, **inp2):
    return rep

models = {"AR (Autoregression)": rep, "Linear neural unit": rep2}
modelsparameters = {"AR (Autoregression)": {"predicts":  2}, "Linear neural unit": {"predicts": 4}}
modelscomplet = zip(models.keys(), models.values(),  modelsparameters.values())
modelsresults = []

for i, j, k in modelscomplet:
    modelsresults.append({i: j(1, **k)})

# Nested dictionaries - Find minimum

top = 1000000
for key, value in modelsparameters.items():
    for inkey, invalue in value.items():
        if invalue < top:
            best_model_name = key
            best_data = inkey
            top = invalue

# Dictionary comprehension

{x: x**2 for x in range(1, 5)} # => {1: 1, 2: 4, 3: 9, 4: 16}
{pismeno for pismeno in "abeceda"} # => {"d", "a", "c", "e", "b"}
```

##### Two dictionaries intersection

    d1 = {'a': 1, 'b': 2}
    d2 = {'b': 2, 'c': 3}

    d1.keys() & d2.keys()  # {'b'}

```python
# Or with set
a = { 'x' : 1, 'y' : 2, 'z' : 3 }
b = { 'u' : 1, 'v' : 2, 'w' : 3, 'x'  : 1, 'y': 2 }
set( a.keys() ) & set( b.keys() )  # Output set(['y', 'x'])
set( a.items() ) & set( b.items())  # Output set([('y', 2), ('x', 1)])
```

#### Deque

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

#### Set

A set is unordered and stores unique values only.

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

#### Decimal

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

### Iterators

```python
iterable = [1, 2, 3]
iterator = iter(iterable)

# Next value
next(iterator)  # => "one"
```

### Conditions (if, else...)

```python
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

# Ternary operator
variable = 3
state = "nice" if variable else "not nice"

# Other shortened syntax
a = 4
b = 3
x = 77
y = 66
result = (y, x)[a > b]  # y if [True], x if [False]
```

### Loops

#### For

```python
# Basic loops
for animal in ["dog", "cat", "mouse"]:
    print(animal)
for i in range(4):
    print(i)
for i in range(4, 8):
    print(i)
colors  =  ["red",  "green",  "blue",  "purple"]
for  i  in  range(len(colors)):
    print(colors[i])

# Return indexes and elements
ints = [8, 23, 45, 12, 78]
for idx, val in enumerate(ints):
    print(idx, val)

# Break out of nested loop
for x in range(10):
    for y in range(10):
        print(x * y)
        if x*y > 50:
            break
    else:
        continue  # only executed if the inner loop did NOT break
    break  # only executed if the inner loop DID break

# Generate keyed values without exec
cats = {f"cat_{k}": k * 2 for k in range(5)}

# Comprehensions and nested collections
[x*5 for x in range(5)] #[0, 5, 10, 15, 20]
[x for x in range(5) if x%2 == 0]  #[0, 2, 4]
[a if a else  2  for a in  [0,1,0,3]]

list1 = [1, 2, 3]
list2 = [3, 4, 5]
[a + b for a, b in zip(list1, list2)]

a = [[1,2,3],[2,3,4],[5,6,7]]
print(a[1][1])
```

#### While

```python
x = 0
while x < 4:
    print(x)
    x += 1

# Infinite loop pattern
while True:
    listen_forever()
```

### Functions

```python
# Basic function definition and calls
def add(x, y):  # Create new function with def
    return x + y  # Return values with return

add(5, 6)  # Call the function with parameters, return 11
add(y=6, x=5)  # Named arguments

def return_arguments(*argumenty):  # Variable number of arguments
    return argumenty

return_arguments(1, 2, 3)  # => (1, 2, 3)

def return_named_arguments(**pojmenovane_argumenty):  # Varaiable number of named arguments
    return pojmenovane_argumenty

return_named_arguments(who="is afraid", must_not="go to the forest")  # {"who": "is afraid", "must_not": "go to the forest"}

def return_all(*args, **kwargs):  # You can use combination
    print(args, kwargs)  # Return all parameters

# Jump out of function - return
def print_var():
    a = 8
    if a > 5:
        return
    print_var(2)

# Default parameter
def funkce(y, lags=50): #If we use lags in call, it will be overwritten
    return 1

# Unknown number of parameters - *args, **kwargs
def print_all(*args, **kwargs):
    print(args, kwargs)

print_all(1, 2, a=3, b=4) # Use: (1, 2) {"a": 3, "b": 4}
tuple = (1, 2, 3, 4)
dic = {"a": 3, "b": 4}
print_all(tuple)  # Is like print_all((1, 2, 3, 4)). One parameter - tuple
print_all(*tuple)  # Is like print_all(1, 2, 3, 4)
print_all(**dic)  # Is like print_all(a=3, b=4)
print_all(*tuple, **dic)  # Is like print_all(1, 2, 3, 4, a=3, b=4)

# Global variables
x = 5
def setx(cislo):  # Local variable override global
    x = cislo  # => 43
    print(x)  # => 43

def setglobalx():
    global x
    print(x)  # => 5
    x = 6  # Set global x to 6, so x = 6 also outside the function
print(x)  # => 6

# Functions are objects
def adder(pricitane_cislo):
    def add(x):
        return x + pricitane_cislo
    return add
add_10 = adder(10)
add_10(3)  # => 13

# callable
if callable(a):
    pass

# Function in list generator
[add_10(i) for i in [1, 2, 3]]  # => [11, 12, 13]
[x for x in [3, 4, 5, 6, 7] if x > 5]  # => [6, 7]

# Parse function arguments
import inspect
frame = inspect.currentframe()
args, _, _, values = inspect.getargvalues(frame)
```

### map, filter, reduce

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

### Generators

Generators are functions, that instead return have yield

    def multiplier_2(sequention):
        for i in sequention:
            yield 2 * i

Generator generate values one after one, when it\s needed. Instead of been generated all at once
Example of generator is range(10000)

### Decorators

Dekorators are functions, that wrap other functions, by that
it can change it's behaviour.

```python
def repeated(puvodni_funkce):
    def repeat_function(*args, **kwargs):
        for i in range(3):
            puvodni_funkce(*args, **kwargs)
    return repeat_function
```

    @repeated
    def pozdrav(Name):
        print("Bye {}!".format(Name))

    pozdrav("Dan")  # Return 3x: Bye Dan!

#### Use decorator with condition

    from memory_profiler import profile
    use_decorator = False

    def empty_decorator(fu):
        return fu

    if use_decorator == False :
        profile = empty_decorator

    @profile
    def fu():
        print('content')

### Modules

A Python module is a `.py` file. You can import standard modules, third-party packages, and your own modules.

```python
from math import sqrt

help(sqrt)  # module/function help
dir(sqrt)   # inspect available attributes
```

### Classes

```python
class Human:
    """Basic class example."""

    species = "H. sapiens"  # Class variable shared by all instances

    def __init__(self, name):
        self.name = name

    def say(self, message):
        return f"{self.name}: {message}"

    @classmethod
    def get_species(cls):
        return cls.species

    @staticmethod
    def clear_throat():
        return "*ehm*"


class Date:
    """Example with classmethod constructor and validation."""

    def __init__(self, day=1, month=1, year=1970):
        self.day = day
        self.month = month
        self.year = year

    @classmethod
    def from_string(cls, date_as_string):
        day, month, year = map(int, date_as_string.split("-"))
        if not cls.is_valid_date(day, month, year):
            raise ValueError("Invalid date")
        return cls(day, month, year)

    @staticmethod
    def is_valid_date(day, month, year):
        return 1 <= day <= 31 and 1 <= month <= 12 and 1 <= year <= 3999


# Create objects
david = Human(name="David")
alice = Human("Alice")
print(david.say("hello"))
print(alice.say("hi"))

date2 = Date.from_string("11-09-2012")
print(date2.day, date2.month, date2.year)

# Inspect class namespace
print(Human.__dict__)

# Enumerate instances currently tracked by GC
import gc

for obj in gc.get_objects():
    if isinstance(obj, Human):
        print(obj.name)
```

### Magic methods (dunder methods)

Dunder (double-underscore) methods let custom classes integrate with Python built-ins and operators.

```python
class Polynomial:
    def __init__(self, *coefficients):
        # Highest degree first, e.g. Polynomial(2, 0, 1) == 2x^2 + 1
        self.coefficients = coefficients

    def __call__(self, x):
        result = 0
        degree = len(self.coefficients) - 1
        for i, coeff in enumerate(self.coefficients):
            result += coeff * (x ** (degree - i))
        return result


class Vector:
    def __init__(self, *values):
        self.values = values or (0, 0)

    def __add__(self, other):
        added = tuple(a + b for a, b in zip(self.values, other.values))
        return Vector(*added)


p = Polynomial(1, -0.5, 0.75, 2)
print(p(2))

v1 = Vector(1, 2)
v2 = Vector(10, 13)
print((v1 + v2).values)  # (11, 15)
```

Common dunder methods:

- `__init__` for initialization
- `__repr__` for developer-friendly representation
- `__str__` for user-facing string representation
- `__call__` for callable instances
- `__add__`, `__sub__`, `__mul__` for operators
- `__lt__`, `__eq__` for comparisons

#### Repr

```python
class Pizza:
    def __init__(self, ingredients):
        self.ingredients = ingredients

    def __repr__(self):
        return f"Pizza({self.ingredients!r})"


print(Pizza(["cheese", "mushroom"]))
```

### Bitwise operations

Unsigned integers can hold bigger values, but cannot be negative!!!

```python
# Convert number to bits

binn = bin(123)  # 0b1111011

# Or

four_bytes = 16.to_bytes(4, byteorder='big', signed=True)
print(four_bytes)

# Convert with keep bit structure

bin_number = f'{3:08b}'  # 00000011
bin_number = f'{3:#08b}'  # 0b000011

# Bit length

a.bit_length()

# Convert back to int again

int('11111111', 2)  # 255

# Or i = int.from_bytes(some_bytes, byteorder='big')

# Create empty bytes
empty_bytes = bytes(4)

# And, or etc...

a = 60            # 60 = 0011 1100
b = 13            # 13 = 0000 1101
c = 0

# Bit and
c = a & b;        # 12 = 0000 1100

# Bit or
c = a | b;        # 61 = 0011 1101

# bitwise exclusive or
c = a ^ b;        # 49 = 0011 0001

# Flip bits
c = ~a;           # -61 = 1100 0011

# Bit shift (4 bits)
c = a << 2;       # 240 = 1111 0000
c = a >> 2;       # 15 = 0000 1111

# Formating
bin = "{0:b}".format(i) # binary: 11111111
hex = "{0:x}".format(i) # hexadecimal: ff
oct = "{0:o}".format(i) # octal: 377

```

### File I/O

#### Common file and import snippets

```python
from math import sqrt  # Import function from another module
from pathlib import Path
from numpy import loadtxt
import glob
import os

# Find script address (__file__ is not available in some notebook contexts)
# script_dir = os.path.dirname(__file__)

# Import variables from another file
# import file        # then use: file.value
# from file import * # import names directly

# Check whether file/directory exists
my_file = Path("/path/to/file")
if my_file.is_file():
    pass
if my_file.is_dir():
    pass
if my_file.exists():
    pass

# Create a module from a folder:
# 1) add __init__.py
# 2) inside it, use relative imports like: from .autoregLNU import autoregLNU

# List and filter files
all_files = os.listdir("test/")
txt_files = [x for x in all_files if x.endswith(".txt")]
tif_counter = len(glob.glob1(".", "*.tif"))

# Load files with suffix and wildcard patterns
imgs = glob.glob("*.png")
for name in glob.glob("dir/file?.txt"):
    print(name)
for name in glob.glob("dir/*[0-9].*"):
    print(name)

# Add matching names to a list
script_dir = Path(".")
data_names_list = list(glob.glob((script_dir / "*.dat").as_posix()))

# Import txt data
# x = loadtxt("realna_data_klapky.txt")
```

#### Open file

```python
# Open file in current directory
f = open("test.txt", "w+")

# File modes:
# 'r' read (default)
# 'w' write (create/truncate)
# 'x' exclusive create (fails if exists)
# 'a' append (create if missing)
# 't' text mode (default)
# 'b' binary mode
# '+' read and write

# Open by full path
f = open("C:/Python33/README.txt")
f.close()

# Use context manager so file closes automatically
with open("test.txt", "w") as f:
    pass
f.write("my first file\n")
f.write("This file\n\n")
f.write("contains three lines\n")

f = open("test.txt", "r", encoding="utf-8")
f.read(4)  # Read the first 4 data - 'This'
f.read(4)  # Read the next 4 data - ' is '
f.read()  # Read in the rest till end of file - 'my first file\nThis file\ncontains three lines\n'
f.read()  # Further reading returns empty string

# After editing text, jump with cursor to the beginning before read
f.seek(0)
f.read()  # Return all
```

#### Manipulate with files (move, copy)

```python
import os
import shutil
import send2trash

# Copy file
shutil.copy("C:\\spam.txt", "C:\\delicious")

# Copy folder with all files
shutil.copytree("C:\\bacon", "C:\\bacon_backup")

# Move file
shutil.move("C:\\bacon.txt", "C:\\eggs")

# Remove to trash bin
send2trash.send2trash("bacon.txt")

# Remove file and folder
os.unlink(path)
shutil.rmtree(path)

# File tree walk
for folderName, subfolders, filenames in os.walk("C:\\delicious"):
    print("The current folder is " + folderName)
    for subfolder in subfolders:
        print("SUBFOLDER OF " + folderName + ": " + subfolder)
    for filename in filenames:
        print("FILE INSIDE " + folderName + ": " + filename)
```

### Try - Except

```python
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

# Try for all errors and print them
try:
    linux_interaction()
except Exception as error:
    print(error)

# Raise exception
# if somethingbad:
#   raise Exception('x should not exceed 5. The value of x was: {}')  # Better concrete error. E.g. ValueError...

# Raise in except block
try:
    1/0
except Exception:
    raise

# Assert - require something or error
x = 5
assert (x > 4), 'What happened'

# Print error details in except block
import traceback
try:
    1/0
except Exception as e:
    print(traceback.format_exc())
```

### Builtin functions

```python
# print: combine strings and variables
a = "hi"
print("as", a, "fegg")

# range
x = range(3 + 1)  # 0, 1, 2, 3

# reversed iteration
for i in reversed(range(5)):
    print(i)  # 4, 3, 2, 1
```

### Builtin variables

#### \_\_name\_\_

If file is imported, code inside the condition will not execute. If the file itself is run, code will execute.

    if __name__ == "__main__":
        pass

### Builtin modules

#### Sys, io, os quick snippets

```python
import io
import os
import sys
import warnings

# sys: object size and custom import path
x = 2
sys.getsizeof(x)
sys.path.insert(0, "/path/to/your/package_or_module")

# io + warnings: capture printed output
stdout = sys.stdout
sys.stdout = io.StringIO()
warnings.warn("asdefwefwg")
print("aho")
output = sys.stdout.getvalue()
sys.stdout = stdout
print(output)

# os: working directory and environment variables
os.chdir(r"C:\Users\...")
env_var = os.environ["ENV_VAR"]
```

#### Pathlib - Work with paths

```python

# Get path of running file
from pathlib import Path
import inspect
import os

# Get script path
file_path = Path(__file__)

# If it must also work in Jupyter, you cannot use __file__
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

# Find full address

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

Deprecated historical examples (kept for legacy reference):

from os import path
import os

file_path = path.relpath("data/data.txt")

# Absolute path

# os.path.abspath(\__file__)  # Not working in jupyter

# Add path to files and modules

import os
data_folder = "folder/nextfolder/"
file_to_open = data_folder + "data.txt"

# For example if jupyter do not see some files

script_dir0 = os.path.abspath('') # C:\VScode

# Next

# script_path = os.path.abspath(\__file__)  # i.e. /path/to/dir/foobar.py  (not working in Jupyter)
script_path = 'C://prog'
script_dir = os.path.split(script_path)[0]  #i.e. /path/to/dir/
rel_path =  "2091/data.txt"
abs_file_path = os.path.join(script_dir, rel_path)  # Result is relative '/path/to/dir/2091/data.txt'
filename = os.path.join(script_path,  '../same.txt')  # We can use name of file

dir_path = os.path.dirname(os.path.realpath(__file__))  # We can use address of file... but not in jupyter

# You can also do

# script_dir = os.path.dirname(\__file__)
rel_path = "test_ data/realna_data_klapky.txt"
abs_file_path = os.path.join(script_dir, rel_path)
'''
```

#### Warnings

```python
import warnings

warnings.warn("Example warning message")

# Promote matching warnings to errors
warnings.filterwarnings("error", message=r"[\s\S]*HessianInversionWarning*")

# Show deprecations and hide matching warnings
warnings.filterwarnings("always", category=DeprecationWarning)
warnings.filterwarnings("ignore")

# Other actions: "default", "once", "error", "ignore", "always", "module"
```

#### re - Regular expressions

```
[]     A set of characters     "[a-m]"
\     Signals a special sequence (can also be used to escape special characters)     "\d"
.     Any character (except newline character)     "he..o"
^     Starts with     "^hello"
$     Ends with     "world$"

*     Zero or more occurrences     "aix*"
+     One or more occurrences     "aix+"
{}     Exactly the specified number of occurrences     "al{2}"
|     Either or     "falls|stays"

\A     Returns a match if the specified characters are at the beginning of the string     "\AThe"
\b     Returns a match where the specified characters are at the beginning or at the end of a word     r"\bain"
r"ain\b"

\B     Returns a match where the specified characters are present, but NOT at the beginning (or at the end) of a word     r"\Bain"
r"ain\B"


\d     Returns a match where the string contains digits (numbers from 0-9)     "\d"
\D     Returns a match where the string DOES NOT contain digits     "\D"
\s     Returns a match where the string contains a white space character     "\s"
\S     Returns a match where the string DOES NOT contain a white space character     "\S"
\w     Returns a match where the string contains any word characters (characters from a to Z, digits from 0-9, and the underscore _ character)     "\w"
\W     Returns a match where the string DOES NOT contain any word characters     "\W"
\Z     Returns a match if the specified characters are at the end of the string     "Spain\Z"

[arn]     Returns a match where one of the specified characters (a, r, or n) are present
[a-n]     Returns a match for any lower case character, alphabetically between a and n
[^arn]     Returns a match for any character EXCEPT a, r, and n
[0123]     Returns a match where any of the specified digits (0, 1, 2, or 3) are present
[0-9]     Returns a match for any digit between 0 and 9
[0-5][0-9]     Returns a match for any two-digit numbers from 00 and 59
[a-zA-Z]     Returns a match for any character alphabetically between a and z, lower case OR upper case
[+]     In sets, +, *, ., |, (), $,{} has no special meaning, so [+] means: return a match for any + character in the string
```


#### Subprocess - Run shell commands

```python
import subprocess

# Subprocess is preferred over os.system
subprocess.run(["ls"], check=False)

# Capture output
p1 = subprocess.run(["ls", "-la"], capture_output=True, text=True, check=False)
print(p1.stdout)

# More general process execution
status = subprocess.Popen("mycmd myarg", shell=True).wait()
```

#### Pickle

```python
import pickle

d = {"a": 1, "b": 2}

# Save file in binary format
with open(r"someobject.pickle", "wb") as output_file:
    pickle.dump(d, output_file)

# Load file from binary format
with open(r"someobject.pickle", "rb") as input_file:
    e = pickle.load(input_file)
```

Result is `e = {"a": 1, "b": 2}`

#### Time and datetime

```python
import datetime as dt
import time

# Print today's date
date = dt.datetime.now().strftime("%d-%m-%Y")

# Measure elapsed time
start = time.time()
a = 6
end = time.time()
print(end - start)

# Wait for some time
time.sleep(1)
```

#### Argparse - Command Line Interface (cli)

If you want to call a script from terminal like `python myscript.py` and want to add some optional arguments (one letter -a , -b etc. or word with two dashes --var)

Nowadays argparse is better and more often used than sys.argv, getopt

    import argparse

```python
parser = argparse.ArgumentParser(description='A tutorial of argparse!')
parser.add_argument('-l','--list', default=1, type=int, help="This is the 'l' variable")
parser.add_argument("--education",
                    choices=["highschool", "college", "university", "other"],
                    required=True, type=str, help="Your name") # If type list, just append action='append'
```

    args = parser.parse_args()

    # If error with jupyter, then !!!
    par_args = parser.parse_known_args()
    myarg = par_args[0].myarg

    ed = args.education

```python
if ed == "college" or ed == "university":
    print("Has degree")
elif ed == "highschool":
print("Finished Highschool")
else:
    print("Does not have degree")
```

There are also 3rd party libraries for this as `click`

### Asynchronicity and parallelism

Async code mean, that GIL bounded python code still uses just one CPU, but when it waits for network or file system, it can switch context and jump to another task, so it doesn't block.

Parallel code means, that it runs on multiple CPUs.

#### IO bound, CPU bound

If you want to make app faster, you need to think where is the bottleneck. It can be one CPU limitation (CPU bound) or blocking one task by waiting for something (IO bound)

**IO bound example**. I have 50 URLs and I need to get 50 responses. I call it in for loop, send one request, wait 200 ms and then call another. You need **async** here.

**CPU bound example**. I evaluate ML model and it takes 1s. It runs on 1 CPU. On 16 cores, it would take much less. You need **parallel** here.

#### Concurrent futures

Use `concurrent.futures` for simple parallel execution APIs.

ThreadPoolExecutor vs ProcessPoolExecutor:

- `ThreadPoolExecutor`: Best for blocking IO tasks (HTTP with sync libs, file operations, waiting on external systems). It has lower startup/communication overhead.
- `ProcessPoolExecutor`: Best for CPU-heavy tasks (math, transforms, data crunching) because it bypasses the GIL. It has higher overhead because data is serialized between processes. Functions for it must be picklable and defined at module level.

```python
from concurrent.futures import ProcessPoolExecutor, ThreadPoolExecutor
from time import sleep


def io_task(x: int) -> int:
    sleep(0.3)  # Simulate blocking IO
    return x * 10


def cpu_task(n: int) -> int:
    # Simulate CPU-heavy work
    total = 0
    for i in range(3_000_000):
        total += (i * n) % 97
    return total


if __name__ == "__main__":
    inputs = [1, 2, 3, 4]

    # 1) IO-bound: prefer threads
    with ThreadPoolExecutor(max_workers=4) as pool:
        io_results = list(pool.map(io_task, inputs))
    print("ThreadPoolExecutor (IO):", io_results)

    # 2) CPU-bound: prefer processes
    with ProcessPoolExecutor(max_workers=4) as pool:
        cpu_results = list(pool.map(cpu_task, inputs))
    print("ProcessPoolExecutor (CPU):", cpu_results)
```

Rule of thumb:

- Waiting most of the time: use `ThreadPoolExecutor`.
- Computing most of the time: use `ProcessPoolExecutor`.
- If you want to have async processing of functions, that are async, use `asyncio` instead. Use executors mainly for legacy blocking code.

#### Multiprocessing

Use `multiprocessing` when you need explicit process control (instead of futures API).

```python
from multiprocessing import Pool, cpu_count


def square(n: int) -> int:
    return n * n


if __name__ == "__main__":
    numbers = [1, 2, 3, 4, 5, 6]
    workers = min(4, cpu_count())

    with Pool(processes=workers) as pool:
        results = pool.map(square, numbers)

    print("Workers:", workers)
    print("Results:", results)
```

Notes:

- Keep worker function at module top level (required for process spawning).
- Always protect entrypoint with `if __name__ == "__main__":`.
- For simple task submission API, `ProcessPoolExecutor` is often more convenient.

#### Asyncio

Use `asyncio` for high-concurrency IO when libraries provide async APIs.

```python
import asyncio


async def fetch_fake(delay_s: float, name: str) -> str:
    await asyncio.sleep(delay_s)  # Simulate network wait
    return f"done:{name}"


async def main() -> None:
    tasks = [
        fetch_fake(0.8, "a"),
        fetch_fake(0.4, "b"),
        fetch_fake(0.6, "c"),
    ]
    results = await asyncio.gather(*tasks)
    print(results)


if __name__ == "__main__":
    asyncio.run(main())
```

TaskGroup (Python 3.11+) is a structured-concurrency alternative to `gather`:

```python
import asyncio


async def worker(name: str, delay_s: float) -> str:
    await asyncio.sleep(delay_s)
    return f"done:{name}"


async def main() -> None:
    tasks: list[asyncio.Task[str]] = []

    async with asyncio.TaskGroup() as tg:
        tasks.append(tg.create_task(worker("a", 0.8)))
        tasks.append(tg.create_task(worker("b", 0.4)))
        tasks.append(tg.create_task(worker("c", 0.6)))

    # Reaching this point means all tasks finished successfully.
    results = [task.result() for task in tasks]
    print(results)


if __name__ == "__main__":
    asyncio.run(main())
```

Limit concurrency when needed:

```python
import asyncio


sem = asyncio.Semaphore(10)


async def bounded_task(i: int) -> int:
    async with sem:
        await asyncio.sleep(0.1)
        return i
```

### 3rd party libraries

#### Documentation

##### MkDocs

It is way how to turn markdown files into documentation

There is also material for MkDocs, which create webpage with React Material inspired look.

Beware, that MkDocs is not maintained for a long time and it may be problematic to incude it into techstack. There are new alternatives that are drop-in replacement.

##### Sphinx - Create documentation

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

#### Tests

##### Pytest

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

#### Plots, graphs

##### Plotly

```python
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


# Plotly timeseries

py.plotly.iplot([{
    'x': data_for_predicts_csv_trimmed.index,
    'y': data_for_predicts_csv_trimmed[col],
    'name': col
}  for col in data_for_predicts_csv_trimmed.columns])

# Or the same with cufflinks
cf.go_offline()
df.iplot(kind='scatter', filename='cufflinks/cf-simple-line')

# From matplotlib to plotly

plot_mpl(fig)

# Save picture

import plotly.io as pio
static_image_bytes = pio.to_image(fig, format='png')
```

##### Matplotlib

```python
# Simple plot

import matplotlib.pyplot as plt
plt.plot(data)

# Load txt and plot it

from numpy import loadtxt # For one column txt
x = loadtxt('realna_data_klapky.txt')
figure(figsize=(20,5))
plot(x,'-o',label="x");xlabel('i');ylabel('x [sec]');grid();title("realna_data_klapky.txt")
legend();show()
plt.savefig('fig1.png', dpi =  300) # saves the plot

# Jupyter plot one after one

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

# Histogram

figure(figsize=(20,5))
nbins=int(round(1+3.3*log(N))) # Sturge's rule, but depend on data
xlabel("x");ylabel("number_of_bins"),title("number_of_bins  - values x in intervals");grid()
a=hist(x,bins=nbins) # returns counts and bin edges: a[0]=counts, a[1]=bin_edges
print("number_of_bins=", a[0])
print("bin_edges =", a[1])

# Curves and points

x=arange(-600,100) #
mu=m1;sigma=sqrt(s2);print("mu=", mu, "sigma=", sigma)
f=1/(sigma*sqrt(2*pi))*exp(-(((x-mu)/sigma)**2)/2)
figure(figsize=(20,5))
plot(x,f,'g',label="f(x) spojita");grid()
plot(x_i,f_odhad,'o',label="f(x) odhad z mereni pro nbin="+str(nbins)+" intervalu")
legend()

# Two graphs

N=1000 # sample size
x=random.uniform(-10,10,N)
x=random.randn(N)*1000-300
x=random.logistic(3, 10, N)
x=random.standard_t(3, N)
x=random.poisson(1, N)*.01+1000
nbins=int(round(1+3.3*log(N)))
figure(figsize=(20,5))
subplot(121);plot(x,'-o');grid();xlabel('sample index');ylabel("x")
subplot(122)

# First number is how much rows and second how much columns
# Third number is which graph is it - first, second etc.

a = hist(x, nbins, density=True)
grid(); ylabel("$\\approx f(x)$"); xlabel("x")
plt.tight_layout() # Avoid overlap between subplots
plt.subplots_adjust(top=0.88)# Move title higher after tight_layout
subplots_adjust(wspace=.3) # Wider spacing between subplots

# Another way to do more plots

fig = plt.figure()

ax1 = plt.subplot(311)

ax2 = plt.subplot(312, sharex=ax1)  # Share axis
plt.ylabel("Error")
# Or rather  ax.set_ylabel('common ylabel')   - also for x

setp(ax2.get_xticklabels(), visible=False)  # To hide numbers on xaxis
ax3 = plt.subplot(313)
plt.ylabel("Weights")

# If interactive add   ax1.clear()
y_ref_pl, = ax1.plot(t, y_ref, label="Ground truth"); plt.xlabel('t')
y_pl, = ax1.plot(t, y, label="Identified")
plt.legend(loc="upper right")

e_pl, = ax2.plot(t, e)

w_all_pl = ax3.plot(t, w_all)
plt.ylabel("Weight values")

# More plots with one legend

plt.figure(figsize=(12,7))
plt.subplot(3, 1, 1)
plt.plot(t, yy, label='Prediction'); plt.xlabel('t')
plt.plot(t, data, label='Ground truth'); plt.xlabel('t')
plt.legend(loc="upper right")  # prop={'size': 6}   to change size
plt.ylabel("u4 predicted")

# Or

line_up, = plt.plot([1,2,3], label='Line 2')
line_down, = plt.plot([3,2,1], label='Line 1')
plt.legend(handles=[line_up, line_down])

# More plots with for cycle

for i in range(1,7):
plt.subplot(3, 2, i)
plt.plot(oknomean[i]);plt.grid(); plt.xlabel('t')
plt.ylabel("u{}".format(i))
plt.suptitle("Moving average window", fontsize=20)
plt.tight_layout()
plt.subplots_adjust(top=0.88)
plt.show()

# Scatterplot

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

# Text plot

import matplotlib.pyplot as plt
fig = [plt.figure](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.figure.html#matplotlib.pyplot.figure "View documentation for matplotlib.pyplot.figure")(figsize=(5, 1.5))

text = fig.text(0.5, 0.5, 'Hello path effects world!\nThis is the normal '
    'path effect.\nPretty dull, huh?',
    ha='center', va='center', size=20)

[plt.show](https://matplotlib.org/api/_as_gen/matplotlib.pyplot.show.html#matplotlib.pyplot.show "View documentation for matplotlib.pyplot.show")()

# Log axis

plt.loglog(x, y)  # Both axis are log
```

#### Web

##### HTTPX - API - GET, POST (sync + async)

Httpx2 is continuation on httpx as it has not been updated for some time.

```python
import asyncio
import httpx2

# Sync API close to requests style (no base_url)
response = httpx2.get("https://httpbin.org/get", params={"page": 1}, timeout=10.0)
response.raise_for_status()
print(response.status_code)
print(response.json()["args"])  # {'page': '1'}

# Session-like usage (close to requests.Session)
client = httpx2.Client(headers={"x-test": "true"}, timeout=10.0)
try:
    client.get("https://httpbin.org/cookies/set/mipyt/best")
    cookies_response = client.get("https://httpbin.org/cookies")
    print(cookies_response.json())  # {'cookies': {'mipyt': 'best'}}
finally:
    client.close()

# Optional: same sync client pattern with base_url
with httpx2.Client(base_url="https://httpbin.org", timeout=10.0) as client:
    headers_response = client.get("/headers", headers={"x-test2": "true"})
    headers_response.raise_for_status()
    print(headers_response.json()["headers"])


async def main() -> None:
    # Async client with cleanup
    async with httpx2.AsyncClient(
        base_url="https://httpbin.org",
        headers={"x-test": "true"},
        timeout=10.0,
        follow_redirects=True
    ) as client:
        response = await client.get("/headers", headers={"x-test2": "true"})
        response.raise_for_status()
        print(response.json()["headers"])


asyncio.run(main())

# Prefer one global client with manual cleaning on shutdown
```

##### Beautiful soup - web scraping

```python
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

#### Parse sentence
import re
print(re.search('the key number is (.*).', sample_html).group(1))  # 23

## Create simplest http server

import http.server
import socketserver

PORT = 8000
ADDRESS = "127.0.0.1"

Handler = http.server.SimpleHTTPRequestHandler

httpd = socketserver.TCPServer((ADDRESS, PORT), Handler)

print ("Serving at port", PORT)
httpd.serve_forever()
```

#### Images, pictures

```python
from PIL import Image
from resizeimage import resizeimage

## Show image

ax1.imshow(img)

## Resize

img = Image.open(im)
img = resizeimage.resize_contain(img, [100,100]) # or
img = img.resize((1250,890),Image.ANTIALIAS) # antialias

## Save

img.save(soubor[0]+'_resized_'+soubor, img.format)
img.close()

## Convert to black and white

img = img.convert('L') # Converts image to grayscale

## Convert image to matrix

img=np.array(img)
```


#### Database

##### pyodbc, sqlalchemy

```python
from sqlalchemy import create_engine
import urllib

# Read
server = 'SERVER={};'.format(server)
database = 'DATABASE={};'.format(database)
sql_params = r'DRIVER={ODBC Driver 13 for SQL Server};' + server + database + 'Trusted_Connection=yes;'

# Write
sql_conn = pyodbc.connect(sql_params)
df = pd.read_sql(query, sql_conn)

params = urllib.parse.quote_plus(r'DRIVER={driver};SERVER={server};DATABASE={database};Trusted_Connection=yes'.format(driver=r'{SQL Server}', server=server, database=database))
conn_str = 'mssql+pyodbc:///?odbc_connect={}'.format(params)

engine = create_engine(conn_str)
dataframe_to_sql.to_sql(name='FactProduction', con=engine, schema='Stage', if_exists='append', index=False)
```

#### Misc

##### Tables

```python
from prettytable import PrettyTable
models_table = PrettyTable()
models_table = PrettyTable().field_names = ["City name", "Area", "Population", "Annual Rainfall"]
models_table = PrettyTable().add_row(["Adelaide", 1295, 1158259, 600.5])
print (models_table)
```

### GUI

Possible options for GUI are

- Tkinter (builtin python GUI toolkit)
- WxWidgets
- Kivy (for phone apps)

In many cases, integrating a Python backend with a JavaScript frontend (Vue, React, Svelte) or Electron can be a more maintainable option.

JS look way much better than python alternatives.

#### Tkinter

TODO: open a system window to select a file.

#### VUE and EEl (Electron JS like library)

You can use mypythontools pyvueeel for building an app with graphical interface. It contains working examples.

Beware, that it is not maintained and it should never be used for productive systems!

## Building app - executables

### Pyinstaller

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

```python
numbers = [random.randint(1,100) for i in range(1000)]
lp = LineProfiler()
lp.add_function(do_other_stuff)   # add additional function to profile
lp_wrapper = lp(do_stuff)
lp_wrapper(numbers)
lp.print_stats()
```

#### Other profiling option

    # !  pip install snakeviz
    # !  python -m cProfile -o program.prof my_program.py
    # !  snakeviz program.prof

#### Show memory profile to file

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

# Get numba dtype from other

numba.typeof(np.empty(3))
array(float64, 1d, C)

# Evaluate chunk sizes

    i = numba.cuda.grid(1)
    while i < x.size:
        # Do something
        i += numba.cuda.grid(1)

# Cuda in Numba ###

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

#### Dask formats

```python
dsk_frmts = '''
import pandas as pd
import dask.array as da
import dask.dataframe as dd
x = da.random.random((20, 20), chunks=(10, 10))
res = x.dot(x.T).sum()
res.compute()
```

    # Dataframes implement the Pandas API

    df = pd.DataFrame([[1, 2, 3], [1, 5, 3], [5, 6, 7]], columns = ['one', 'two', 'three'])
    dfdsk = dd.from_pandas(df, npartitions=1)

    # Or dd.read_csv('address')

    from dask_ml.linear_model import LogisticRegression  # Dask-ML implements the Scikit-Learn API
    lr = LogisticRegression()
    lr.fit(dfdsk, dfdsk)

    ## Show task structure

    .visualize()
    '''

## Garbage collector

#### Force to empty memory

    import gc
    gc.collect()

## Miscellaneous

## Misc

### Snippets - examples

#### Measure time

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

#### Show bytecode

    import dis
    dis.dis("dict()")

        #              0 LOAD_NAME                0 (dict)
        #              2 CALL_FUNCTION            0
        #              4 RETURN_VALUE

#### Encoding JSON with Python

    import json

```python
data =  {
            'a':  0,
            'b':  9.6,
            'c':  "Hello World",
            'd':  {
                    'a':  4
            }
}
```

    json_data = json.dumps(data)

    # Notice how the keys are not sorted by default, you would have to add the sort_keys=True

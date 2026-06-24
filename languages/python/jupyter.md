# Jupyter Notebook and IPython

## Table of Contents

- [Jupyter Notebook and IPython](#jupyter-notebook-and-ipython)
  - [Table of Contents](#table-of-contents)
  - [Essentials](#essentials)
  - [Detect Notebook Environment](#detect-notebook-environment)
  - [Embed IPython in a Python Script](#embed-ipython-in-a-python-script)
  - [Magic Commands](#magic-commands)
    - [Autoreload](#autoreload)
  - [Display Helpers](#display-helpers)
    - [YouTube](#youtube)
    - [Show Images from Folder](#show-images-from-folder)
  - [Theming](#theming)
  - [Matplotlib in Jupyter](#matplotlib-in-jupyter)
  - [Dark Background for Matplotlib](#dark-background-for-matplotlib)

## Essentials

```bash
jupyter kernelspec list                     # List available kernels
jupyter kernelspec uninstall <kernel-name> # Uninstall a kernel

# Install classic notebook extensions
pip install jupyter_contrib_nbextensions
jupyter contrib nbextension install

# Example: enable RISE slideshow extension
jupyter nbextension enable --py rise
```

## Detect Notebook Environment

```python
import sys

if "ipykernel" in sys.modules:
    print("Running in Jupyter")
```

```python
# Alternative check
try:
    __IPYTHON__
    print("Running in IPython/Jupyter")
except NameError:
    print("Running in standard Python")
```

## Embed IPython in a Python Script

```python
from IPython.terminal.embed import InteractiveShellEmbed

ipshell = InteractiveShellEmbed()
ipshell.dummy_mode = True
ipshell.magic("%timeit abs(-42)")
```

## Magic Commands

### Autoreload

Reload modules automatically so you do not need to restart the kernel after every code change.

```python
%load_ext autoreload
%autoreload 0  # Disable automatic reloading
%autoreload 1  # Reload only modules imported with %aimport
%autoreload 2  # Reload all modules except those excluded with %aimport

%aimport       # List include/exclude modules
%aimport foo   # Include module foo for %autoreload 1
%aimport -foo  # Exclude module foo
```

## Display Helpers

### YouTube

```python
# from IPython.display import YouTubeVideo
# YouTubeVideo("7VeUPuFGJHk")
```

### Show Images from Folder

```python
import os
from IPython.display import display, Image

names = [f for f in os.listdir("../images/ml_demonstrations/") if f.endswith(".png")]
for name in names[:5]:
    display(Image("../images/ml_demonstrations/" + name, width=100))
```

## Theming

```bash
pip install jupyterthemes
jt -t monokai -T

# Reset theme
jt -r
```

## Matplotlib in Jupyter

```python
%matplotlib widget
from matplotlib import pyplot as plt

fig, ax = plt.subplots()
ax.plot([1, 2, 3], [4, 3, 2])
```

```python
# Alternative backend setup
import matplotlib
matplotlib.use("module://ipympl.backend_nbagg")
```

If widgets are not working in JupyterLab, install the widget extension in your environment.

## Dark Background for Matplotlib

Create `~/.ipython/profile_default/startup/startup.py`:

```python
import matplotlib.pyplot as plt
from jupyterthemes import jtplot

jtplot.style(theme="monokai", context="notebook", ticks=True, grid=False)
```

Advanced theme tweaks can also be applied by editing your Jupyter theme style files.

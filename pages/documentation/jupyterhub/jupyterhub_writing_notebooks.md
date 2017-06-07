
# Writing Notebooks

The Jupyter Notebook is an interactive environment for writing and running code. The notebook is capable of running code in a wide range of languages. However, each notebook is associated with a single kernel. This notebook is associated with the Python kernel, therefore runs Python code. You can change the kernel that a notebook is associated with by selecting `Kernel>Change kernel` from the menu bar.

In addition to code, Notebooks support Markdown and LaTeX for including text and mathematical equations/expressions.

Notebooks are written as collection of "Cells", which can be of two types:

1.  Code cells
2.  Markdown cells (including LaTeX)

### Code cells

Code cells contain a single line or several lines of code. Code cells can be differentiated from Markdown cells by the `In [ ]` indicator on their left:

<img src="images/edit_mode.png" style="height:50px; width:900px">

Code cells can be "executed" by typing `Shift+Enter`. When a code cell is executed by the kernel, any output or errors resulting from running these lines of code is presented below the cell. The first code cell below produces output, while the second produces an error:


```python
a = 11
print(a)
```

    11



```python
print(b)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-6-4851c8fca996> in <module>()
    ----> 1 print(b)
    

    NameError: name 'b' is not defined


There are two other keyboard shortcuts for running code:

* `Alt-Enter` runs the current cell and inserts a new one below.
* `Ctrl-Enter` run the current cell and enters command mode in the same cell.

### Markdown cells

Markdown cells contain a single line or several lines of *Markdown*. A cell can be changed from a code cell to a Markdown cell by:

1.  Selecting or navigating to the cell,
2.  Changing to command mode (press Esc)
3.  Pressing the `m` key

Conversely, to change a cell type from Makrdown to code, press the `y` key instead.

Markdown cells can be differentiated from code cells by the absence of the `In [ ]` indicator on the left:

<img src="images/markdown-cell.png" style="height:50px; width:900px">

When a markdown cell is executed by typing `Shift+Enter`, the resulting **rendered** text is produced. For example, in the above Markdown cell, the text between the asterisks (`*`) is rendered in italics. The output of the above cell is:

This is a *mardown cell*

Markdown is a simple, and extremely powerful markup language. Markdown makes it easy to quickly produe high-quality documents containing elements like stylized text, headings, tables, figures, etc., Markdown also happens to be a superset of HTML, which means that HTML is valid inside Markdown cells. The full specification of Markdown is given here:

<http://daringfireball.net/projects/markdown/>

Code is run in a separate process called the Kernel.  The Kernel can be interrupted or restarted.  Try running the following cell and then hit the "interrupt kernel" button in the toolbar above.

### Managing the kernel


```python
import time
time.sleep(10)
```

If the Kernel dies you will be prompted to restart it. Below is some Python code that crashes the kernel. If this cell is executed, the kernel has to be restarted for the notebook to be usable again.


```python
import sys
from ctypes import CDLL

# This will crash a Linux or Mac system
# by producing a segmentation faul or "segfault"
# equivalent calls can be made on Windows

# Uncomment these lines if you would like to see the segfault.
# After the segfault occurs, you will have to restart the Kernel.

# dll = 'dylib' if sys.platform == 'darwin' else 'so.6'
# libc = CDLL("libc.%s" % dll) 
# libc.time(-1)  # BOOM!!
```

The kernel maintains the state of a notebook's computations. This means that any variables or functions you define in a cell are available to other cells. Restarting the kernel also resets this "state" of the notebook.

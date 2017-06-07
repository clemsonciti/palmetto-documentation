---
title: About JupyterHub
keywords: [jupyterhub,jupyter,Python,R,MATLAB]
sidebar: documentation_sidebar
permalink: jupyterhub_notebooks.html
---

## Notebook Basics

### Starting up notebooks

To create a new notebook, click on the "New" button on the top right of the Files tab of the dashboard.
The different options listed in this menu are the available **kernels** for writing notebooks.
The kernel is what decides which programming language to use to run notebook commands,
and each notebook is associated with a single kernel.

<img src="{{site.baseurl}}/images/jupyterhub-kernel-options.png" width="202px">

When you select a kernel, the notebook user interface (UI) is launched with the
corresponding kernel running in the background.
On the top right of the notebook window, you will see messages about the kernel:

<img src="{{site.baseurl}}/images/jupyterhub-kernel-starting.png" style="width:500px">

<img src="{{site.baseurl}}/images/jupyterhub-kernel-ready.png" style="width:390px">

By default, notebooks are named "Untitled" (with an `.ipynb` extension).
You can click on the name of the notebook on the top left of the notebook UI to change its name.

### Uploading notebooks

Notebooks and files can be uploaded from your local machine to the Palmetto by using the "Upload" button on the Files tab of the dashboard.

### Shutting down notebooks

The file browser shows green "Running" text and a green notebook icon next to running notebooks (as seen below).
Notebooks remain running until you explicitly shut them down; closing the notebook's page is not sufficient.

<img src="{{site.baseurl}}/images/jupyterhub-dashboard-files-tab-run.png" style="width:777px">

To shutdown, delete, duplicate, or rename a notebook check the checkbox next to it and an array of controls will appear at the top of the notebook list (as seen below).  You can also use the same operations on directories and files when applicable.

<img src="{{site.baseurl}}/images/jupyterhub-dashboard-files-tab-btns.png" style="width:301px">

To see all of your running notebooks along with their directories, click on the "Running" tab:

<img src="{{site.baseurl}}/images/jupyterhub-dashboard-running-tab.png" style="width:786px">

This view provides a convenient way to track notebooks that you start as you navigate the file system in a long running notebook server.

### Overview of the Notebook UI

If you create a new notebook or open an existing one, you will be taken to the notebook user interface.
This UI allows you to run code and author notebook documents interactively.
The notebook UI has the following main areas:

* Menu
* Toolbar
* Notebook area and cells

The notebook has an interactive tour of these elements that can be started in the "Help:User Interface Tour" menu item.

### Modal editor

The Jupyter Notebook has a "modal" user interface.
This means that the keyboard does different things depending on which "mode" the Notebook is in.
There are two modes: *edit* mode and *command* mode.

#### Edit mode

Edit mode is indicated by a green cell border and a prompt showing in the editor area:

<img src="{{site.baseurl}}/images/jupyterhub-edit-mode.png" style="width:500px">

When a cell is in edit mode, you can type into the cell, like a normal text editor.

<div class="alert alert-success">
Enter edit mode by pressing `Enter` or using the mouse to click on a cell's editor area.
</div>

#### Command mode

Command mode is indicated by a grey cell border with a blue left margin:

<img src="{{site.baseurl}}/images/jupyterhub-command-mode.png" style="width:500px">

When you are in command mode, you are able to edit the notebook as a whole, but not type into individual cells. Most importantly, in command mode, the keyboard is mapped to a set of shortcuts that let you perform notebook and cell actions efficiently. For example, if you are in command mode and you press `c`, you will copy the current cell. If you then press `v`, you will paste the copied cell below the current cell.

<div class="alert alert-warning">
Be aware of which mode you are in at all times. Don't try to type into a cell in command mode; unexpected things will happen!
</div>

<div class="alert alert-success">
Enter command mode by pressing `Esc` or using the mouse to click *outside* a cell's editor area.
</div>

### Mouse navigation

All navigation and actions in the Notebook are available using the mouse through the menubar and toolbar, which are both above the main Notebook area:

<img src="images/jupyterhub-menubar-toolbar.png" width="786px">

The first idea of mouse based navigation is that **cells can be selected by clicking on them.** The currently selected cell gets a grey or green border depending on whether the notebook is in edit or command mode. If you click inside a cell's editor area, you will enter edit mode. If you click on the prompt or output area of a cell you will enter command mode.

If you are running this notebook in a live session (not on http://nbviewer.jupyter.org) try selecting different cells and going between edit and command mode. Try typing into a cell.

The second idea of mouse based navigation is that **cell actions usually apply to the currently selected cell**. Thus if you want to run the code in a cell, you would select it and click the "run cell" button in the toolbar or the "Cell:Run" menu item. Similarly, to copy a cell you would select it and click the "copy cell" button in the toolbar or the "Edit:Copy" menu item. With this simple pattern, you should be able to do most everything you need with the mouse.

Markdown and heading cells have one other state that can be modified with the mouse. These cells can either be rendered or unrendered. When they are rendered, you will see a nice formatted representation of the cell's contents. When they are unrendered, you will see the raw text source of the cell. To render the selected cell with the mouse, click the "run cell" button in the toolbar or the "Cell:Run" menu item. To unrender the selected cell, double click on the cell.

### Keyboard Navigation

The modal user interface of the Jupyter Notebook has been optimized for efficient keyboard usage. This is made possible by having two different sets of keyboard shortcuts: one set that is active in edit mode and another in command mode.

The most important keyboard shortcuts are `Enter`, which enters edit mode, and `Esc`, which enters command mode.

In edit mode, most of the keyboard is dedicated to typing into the cell's editor. Thus, in edit mode there are relatively few shortcuts.  In command mode, the entire keyboard is available for shortcuts, so there are many more.  The `Help`->`Keyboard Shortcuts` dialog lists the available shortcuts.

We recommend learning the command mode shortcuts in the following rough order:

| Key                  | Action                                         |
|----------------------|------------------------------------------------|
| `enter`              | Enter edit mode from command mode              |
| `shift-enter`        | Execute cell                                   |
| `up/k`, `down/j`     | Move to cell above or below                    |
| `s`                  | Save notebook                                  |
| `y`                  | Change cell type to code cell                  |
| `m`                  | Change cell type to Markdown cell              |
| `1-6`                | Change cell type to heading (levels 1-6)       |
| `a`                  | Create cell above current cell                 |
| `b`                  | Create cell below current cell                 |
| `x`, `c`, `v`        | Cut, copy, paste cell                          |
| `dd`                 | Delete cell                                    |
| `z`                  | Undo cell deletion                             |
| `ii`, `00`           | Interrupt kernel, restart kernel               |


## Code Cells

Code cells allow you to enter and run code.
Run a code cell using `Shift-Enter` or pressing the <button class='btn btn-default btn-xs'><i class="icon-step-forward fa fa-step-forward"></i></button> button
in the toolbar above. For example, in a Python notebook, you can do:


```python
a = 10
```


```python
print(a)
```

    10

There are two other keyboard shortcuts for running code:

* `Alt-Enter` runs the current cell and inserts a new one below.
* `Ctrl-Enter` run the current cell and enters command mode.

### Managing the Kernel

Code is run in a separate process called the Kernel.  The Kernel can be interrupted or restarted.  Try running the following cell and then hit the <button class='btn btn-default btn-xs'><i class='icon-stop fa fa-stop'></i></button> button in the toolbar above.


```python
import time
time.sleep(10)
```

If the Kernel dies you will be prompted to restart it. Here we call the low-level system libc.time routine with the wrong argument via
ctypes to segfault the Python interpreter:

```python
import sys
from ctypes import CDLL
# This will crash a Linux or Mac system
# equivalent calls can be made on Windows

# Uncomment these lines if you would like to see the segfault

# dll = 'dylib' if sys.platform == 'darwin' else 'so.6'
# libc = CDLL("libc.%s" % dll)
# libc.time(-1)  # BOOM!!
```

### Cell menu

The "Cell" menu has a number of menu items for running code in different ways. These includes:

* Run and Select Below
* Run and Insert Below
* Run All
* Run All Above
* Run All Below

### Restarting the kernels

The kernel maintains the state of a notebook's computations. You can reset this state by restarting the kernel. This is done by clicking on the <button class='btn btn-default btn-xs'><i class='fa fa-repeat icon-repeat'></i></button> in the toolbar above.

## Markdown Cells

Text can be added to Jupyter Notebooks using Markdown cells.  Markdown is a popular markup language that is a superset of HTML. Its specification can be found here:

<http://daringfireball.net/projects/markdown/>

The notebook corresponding to this web page can be downloaded [here](https://github.com/jupyter/notebook/blob/master/docs/source/examples/Notebook/Working%20With%20Markdown%20Cells.ipynb).

### Markdown basics

You can make text *italic* or **bold**.

You can build nested itemized or enumerated lists:

* One
    - Sublist
        - This
  - Sublist
        - That
        - The other thing
* Two
  - Sublist
* Three
  - Sublist

Now another list:

1. Here we go
    1. Sublist
    2. Sublist
2. There we go
3. Now this

You can add horizontal rules:

---

Here is a blockquote:

> Beautiful is better than ugly.
> Explicit is better than implicit.
> Simple is better than complex.
> Complex is better than complicated.
> Flat is better than nested.
> Sparse is better than dense.
> Readability counts.
> Special cases aren't special enough to break the rules.
> Although practicality beats purity.
> Errors should never pass silently.
> Unless explicitly silenced.
> In the face of ambiguity, refuse the temptation to guess.
> There should be one-- and preferably only one --obvious way to do it.
> Although that way may not be obvious at first unless you're Dutch.
> Now is better than never.
> Although never is often better than *right* now.
> If the implementation is hard to explain, it's a bad idea.
> If the implementation is easy to explain, it may be a good idea.
> Namespaces are one honking great idea -- let's do more of those!

And shorthand for links:

[Jupyter's website](http://jupyter.org)

### Headings

You can add headings by starting a line with one (or multiple) `#` followed by a space, as in the following example:

```
# Heading 1
# Heading 2
## Heading 2.1
## Heading 2.2
```

### Embedded code

You can embed code meant for illustration instead of execution in Python:

    def f(x):
        """a docstring"""
        return x**2

or other languages:

    if (i=0; i<n; i++) {
      printf("hello %d\n", i);
      x += 4;
    }

### LaTeX equations

Courtesy of MathJax, you can include mathematical expressions both inline: 
$e^{i\pi} + 1 = 0$  and displayed:

$$e^x=\sum_{i=0}^\infty \frac{1}{i!}x^i$$
Inline expressions can be added by surrounding the latex code with `$`:
```
$e^{i\pi} + 1 = 0$
```

Expressions on their own line are surrounded by `$$`:
```latex
$$e^x=\sum_{i=0}^\infty \frac{1}{i!}x^i$$
```

### GitHub flavored markdown

The Notebook webapp supports Github flavored markdown meaning that you can use triple backticks for code blocks:

    <pre>
    ```python
    print "Hello World"
    ```
    </pre>

    <pre>
    ```javascript
    console.log("Hello World")
    ```
    </pre>

Gives:

```python
print "Hello World"
```

```javascript
console.log("Hello World")
```

And a table like this: 

    <pre>
    ```

    | This | is   |
    |------|------|
    |   a  | table| 

    ```
    </pre>

A nice HTML Table:

| This | is   |
|------|------|
|   a  | table| 


### General HTML

Because Markdown is a superset of HTML you can even add things like HTML tables:

<table>
<tr>
<th>Header 1</th>
<th>Header 2</th>
</tr>
<tr>
<td>row 1, cell 1</td>
<td>row 1, cell 2</td>
</tr>
<tr>
<td>row 2, cell 1</td>
<td>row 2, cell 2</td>
</tr>
</table>

### Local files

If you have local files in your Notebook directory, you can refer to these files in Markdown cells directly:

    [subdirectory/]<filename>

For example:

    <img src="images/python_logo.svg" />
    <video controls src="images/animation.m4v" />

These do not embed the data into the notebook file, and require that the files exist when you are viewing the notebook.

#### Security of local files

Note that this means that the Jupyter notebook server also acts as a generic file server
for files inside the same tree as your notebooks.  Access is not granted outside the
notebook folder so you have strict control over what files are visible, but for this
reason it is highly recommended that you do not run the notebook server with a notebook
directory at a high level in your filesystem (e.g. your home directory).

When you run the notebook in a password-protected manner, local file access is restricted
to authenticated users unless read-only views are active.

The Markdown parser included in the Jupyter Notebook is MathJax-aware.  This means that you can freely mix in mathematical expressions using the [MathJax subset of Tex and LaTeX](http://docs.mathjax.org/en/latest/tex.html#tex-support).  [Some examples from the MathJax site](http://www.mathjax.org/demos/tex-samples/) are reproduced below, as well as the Markdown+TeX source.

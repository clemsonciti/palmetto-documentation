# Palmetto Cluster Documentation

This repository contains the source code for the Palmetto Cluster documentation,
live at [palmetto.clemson.edu/palmetto](https://palmetto.clemson.edu/palmetto).

<img
  src="https://user-images.githubusercontent.com/10427974/195657681-b02fd206-19d9-4f21-8100-20fd0e5fab9d.png"
  alt="Screenshot of the Palmetto Documentation site homepage"
  width="400"
/>

## Just here to read the docs?

Head over to the [documentation site](https://palmetto.clemson.edu/palmetto)
to see the published version.

## Editing the Docs

Our documentation site is built using [`mkdocs`](https://www.mkdocs.org), a
static site generator written in Python. It is recommended to install this
on your computer to preview the documentation while editing.

### Commands

When working with the documentation site, you may find these commands useful:

| Command        | Description                                                 |
| -------------- | ----------------------------------------------------------- |
| `mkdocs serve` | Start the live-reloading server to preview site in browser. |
| `mkdocs build` | Build the static documentation site for production use.     |
| `mkdocs -h`    | Print the `mkdocs` help message and exit.                   |

### Installing Dependencies

Before running any commands, you must install the necessary tools to serve the
site locally.

The [Python 3](https://www.python.org/downloads/) interpreter is required.

```sh
$ python3 --version
Python 3.8.10
```

The [`pip` package manager](https://pip.pypa.io/en/stable/getting-started/)
may need to be installed if it was not bundled with your Python installation.

```sh
$ python3 -m pip --version
pip 20.0.2 from /usr/lib/python3/dist-packages/pip (python 3.8)
```

Then, you can use `pip` to install the dependencies of this project, as declared
in `requirements.txt`:

```sh
$ python3 -m pip install -r requirements.txt
Defaulting to user installation because normal site-packages is not writeable
Collecting mkdocs
  Downloading mkdocs-1.4.0-py3-none-any.whl (3.7 MB)
     |████████████████████████████████| 3.7 MB 16.5 MB/s
Collecting markdown_include
  Downloading markdown_include-0.7.0-py3-none-any.whl (16 kB)
     |████████████████████████████████| 16 kB 25.1 MB/s
Installing collected packages: mkdocs, markdown-include
Successfully installed mkdocs-1.4.0 markdown-include-0.7.0
```

Now you should be able to run the commands mentioned above.

## Deployment

The deployment instructions can be found in the RCD space on CCIT Confluence.

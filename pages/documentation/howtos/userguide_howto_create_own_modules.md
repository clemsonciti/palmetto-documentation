---
title: How to create your own modules
keywords: [modules]
sidebar: documentation_sidebar
permalink: userguide_howto_create_own_modules.html
---

To conveniently manage packages that you install in your home directory
you can make them available as modules (for yourself)
by writing a *modulefiles*.
Modulefiles are scripts written in the Tcl scripting langauge.
Even if you don't know Tcl, it's easy to copy and customize one of the modulefiles
in `/software/modulefiles/` to suit your needs.

To create and load your own module:

* Prepare a modulefile (using one of the modulefiles in `/software/modulefiles` as a starting point)
* Place the modulefile in a directory called `privatemodules` in your home directory
* Load the special module `use.own`
* Load your own module with `module add modulefilename`

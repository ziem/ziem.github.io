---
layout: post
title: LaTeX on macOS
---

I learned the basics of LaTeX during my studies. One of our teachers was a big LaTeX fan and was encouraging students to write their diploma thesis using it. I wrote both my bachelor's and master's thesis in LaTeX.

Recently, I use LaTeX very rarely, mainly for my resume, and in this post, I would like to show you how to set it up on macOS.

1. Install [MacTeX Distribution](https://www.tug.org/mactex/) (beware: it's HUGE! ~4 GB).
2. Install [Sublime Text](https://www.sublimetext.com/) (both 3 & 4 are OK).
2. Install [Package Control](https://packagecontrol.io/):
    - open the command palette by pressing `cmd` + `shift` + `p`,
    - type "Install Package Control" and press `enter`.
3. Install [LaTeXTools](https://latextools.readthedocs.io/en/latest/) plugin:
    * open the command palette by pressing `cmd` + `shift` + `p`,
    * type "Install Package" and choose "Package Control: Install Package" then press `enter`,
    * type "LaTeXTools" and press `enter`.
4. To compile your `*.tex` file:
    * open it in the Sublime Text,
    * open the command palette by pressing `cmd` + `shift` + `p`,
    * type "Set Syntax: LaTeX" and press `enter`,
    * build your document by pressing `cmd` + `b`.

Happy writing!

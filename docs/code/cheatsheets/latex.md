---
title: Latex Tricks
toc: true
tags:
    - latex
date: 2025-05-04
dg-publish: true
---

# Latex tricks

## note

## comment

use following to comment mulilines:

```tex
\iffalse
comments
another line
\fi
```

```tex

\newcommand{\chen}[1]{{\textcolor{red}{[Chen: #1]}}}
\newcommand{\liu}[1]{{\textcolor{magenta}{[Liu: #1]}}}
\newcommand{\wang}[1]{{\textcolor{blue}{[Wang: #1]}}}

\begin{document}
\liu{test}

\chen{test}

\wang{test}
\end{document}
```

## diff

`latexdiff` to diff in pdf. tldr:

```


  Determine differences between two LaTeX files.
  More information: <https://ctan.org/pkg/latexdiff>.

  Determine changes between different versions of a LaTeX file (the resulting LaTeX file can be compiled to show differences underlined):

      latexdiff old.tex new.tex > diff.tex

  Determine changes between different versions of a LaTeX file by highlighting differences in boldface:

      latexdiff --type=BOLD old.tex new.tex > diff.tex

  Determine changes between different versions of a LaTeX file, and display minor changes in equations with both added and deleted graphics:

      latexdiff --math-markup=fine --graphics-markup=both old.tex new.tex > diff.tex

```

`git-latexdiff` compare pdf in git history

```
Usage: git latexdiff [options] OLD [NEW]
       git latexdiff [options] OLD --
       git latexdiff [options] -- OLD
Call latexdiff on two Git revisions of a file.

OLD and NEW are Git revision identifiers. NEW defaults to HEAD.
If "--" is used for NEW, then diff against the working directory.

Options:
    --help                this help message
    --help-examples       show examples of usage
    --main <file>         name of the main LaTeX, R Sweave,
                            or Emacs Org mode file.
                            The search for the only file containing 'documentclass'
                            will be attempted, if not specified.
                            For non-LaTeX files, a reasonable `prepare` command
                            will be used unless explicitly provided
    --no-view             don't display the resulting PDF file
    --latex               run latex instead of pdflatex
    --xelatex             run xelatex instead of pdflatex
    --lualatex            run lualatex instead of pdflatex
    --tectonic            run tectonic instead of pdflatex
    --bibtex, --bbl       display changes in the bibliography
                             (runs bibtex to generate *.bbl files and
                             include them in the source file using
                             latexpand --expand-bbl before computing
                             the diff)
    --biber               like --bibtex, but runs biber instead.
    --run-bibtex, -b      run bibtex as well as latex to generate the PDF file
                             (pdflatex,bibtex,pdflatex,pdflatex)
                          NOTE: --bibtex usually works better
    --run-biber           run BibLaTex-Biber as well as latex to generate the PDF file
                             (pdflatex,biber,pdflatex,pdflatex)
                          NOTE: --biber usually works better
    --view                view the resulting PDF file
                            (default if -o is not used)
    --pdf-viewer <cmd>    use <cmd> to view the PDF file (default: $PDFVIEWER)
    --no-cleanup          don't cleanup temp dir after running
    --no-flatten          don't call latexpand to flatten the document
    --cleanup MODE        Cleanup temporary files according to MODE:

                           - keeppdf (default): keep only the
                                  generated PDF file

                           - none: keep all temporary files
                                  (may eat your diskspace)

                           - all: erase all generated files.
                                  Problematic with --view when the
                                  viewer is e.g. evince, and doesn't
                                  like when the file being viewed is
                                  deleted.

    --latexmk             use latexmk
    --build-dir           use pdfs from specific build directory
    --latexopt            pass additional options to latex (e.g. -shell-escape)
    -o <file>, --output <file>
                          copy resulting PDF into <file> (usually ending with .pdf)
                          Implies "--cleanup all"
    --tmpdirprefix        where temporary directory will be created (default: /tmp).
                            Relative path will use repository root as a base
    --verbose, -v         give more verbose output
    --quiet               redirect output from subprocesses to log files
    --prepare <cmd>       run <cmd> before latexdiff (e.g. run make to generate
                             included files)
    --filter <cmd>        run <cmd> after latexdiff and before compilation
                             (e.g. to fix up latexdiff output)
    --ln-untracked        symlink uncommited files from the working directory
    --version             show git-latexdiff version.
    --subtree             checkout the tree at and below the main file
                             (enabled by default, disable with --whole-tree)
    --whole-tree          checkout the whole tree (contrast with --subtree)
    --ignore-latex-errors keep on going even if latex gives errors, so long as
                          a PDF file is produced
    --ignore-makefile     ignore the Makefile, build as though it doesn't exist
    -*                    other options are passed directly to latexdiff
    --latexpand OPT       pass option OPT to latexpand. Use multiple times like
                          --latexpand OPT1 --latexpand OPT2 to pass multiple options.
    --latexdiff-flatten   use --flatten from latexdiff instead of latexpand

```

Important options:

```
Usage: git latexdiff [options] OLD [NEW]
OLD and NEW are Git revision identifiers. NEW defaults to HEAD.
If "--" is used for NEW, then diff against the working directory.
    --latex               run latex instead of pdflatex
    --xelatex             run xelatex instead of pdflatex
    --lualatex            run lualatex instead of pdflatex
    --main <file>         name of the main LaTeX, R Sweave,
    --tmpdirprefix        where temporary directory will be created (default: /tmp).
                            Relative path will use repository root as a base
    --ignore-latex-errors keep on going even if latex gives errors, so long as
                          a PDF file is produced
```

For example, use `git latexdiff HEAD -- --tmpdirprefix tmp --main main.tex` to check differences between worktree and last commit.

---
layout: post
title: "How to Highlight Changes Between Two Versions of LaTeX Files"
date: 2025-01-29
categories: tutorials
tags: [latex]
permalink: /posts/latex-changes/
usemathjax: true
---

<style>
ul.custom-bullets {
    list-style-type: none;
    padding-left: 0;
}
ul.custom-bullets li::before {
    content: "✔️"; /* Change this to any symbol you prefer */
    padding-right: 8px;
}
ul.custom-indent {
    list-style-type: disc;
    padding-left: 20px;
}
</style>

When working with LaTeX documents, it's often necessary to compare two versions of a file to identify changes. This guide will walk you through the process of highlighting differences between two LaTeX files using `latexdiff`.

## Prerequisites

Before proceeding, ensure you have the following tools installed:

<ul class="custom-indent">
    <li><strong>LaTeX Distribution</strong>: Install a LaTeX distribution like <a href="https://www.tug.org/texlive/">TeX Live</a> (for Linux/Windows) or <a href="https://www.tug.org/mactex/">MacTeX</a> (for macOS).</li>
    <li><strong>Diff Tools</strong>: Install a diff tool such as:
        <ul class="custom-indent">
            <li><a href="https://ctan.org/pkg/latexdiff">latexdiff</a> (recommended for LaTeX-specific comparisons).</li>
        </ul>
    </li>
</ul>

## Using `latexdiff`

`latexdiff` is a Perl script specifically designed to compare two LaTeX files and produce a new LaTeX file with highlighted changes.

### Step 1: Install `latexdiff`

If you're using a LaTeX distribution like TeX Live or MacTeX, `latexdiff` is likely already installed. Check via:

```bash
latexdiff --version
```

If it's not installed, you can install it via:

```
sudo apt-get install latexdiff  # For Debian/Ubuntu
brew install latexdiff          # For macOS (using Homebrew)
```

If brew is not installed on your macOS, install via:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Step 2: Compare Two LaTeX Files

Run the following command to compare `old.tex` and `new.tex`:

```
latexdiff old.tex new.tex > diff.tex
```

This will generate a new file `diff.tex` with the following highlights:

<ul class="custom-bullets">
    <li>Additions: Marked in blue with an underline.</li>
    <li>Deletions: Marked in red with a strikethrough.</li>
</ul>

### Step 3: Compile the Output
Compile the `diff.tex` file using your LaTeX editor or command-line tool:

```
pdflatex diff.tex
```

This will produce a PDF where changes are visually highlighted.

**Customization:** You can customize the output using additional options:

Use `--type=CFONT` to highlight changes using colored text instead of underlines and strikethroughs:
  
```
latexdiff --type=CFONT old.tex new.tex > diff.tex
  ```

Use `--flatten` to expands `\input` and `\include` commands for comparison:
  
```
latexdiff --flatten old.tex new.tex > diff.tex
```

To color what is added and not show what has been deleted, use the following options:

```
latexdiff --type=CFONT --append-textcmd="textcolor{blue}" --exclude-textcmd="st" old.tex new.tex > diff.tex
```

To exclude differences in changing the image size, use the following option:

```
latexdiff --graphics-markup=0 old.tex new.tex > diff.tex
```

Example:

```
latexdiff --type=CFONT --append-textcmd="textcolor{blue}" --exclude-textcmd="st" --graphics-markup=0 old.tex new.tex > diff.tex
```


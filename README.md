# md2pdf
A pandoc-based Markdown to pdf script

* Sebastien Kramm, 2023/06
* https://github.com/skramm/md2pdf


## Rationale

This a simple (but convenient) bash script that converts a Markdown file to a (beautiful) pdf, using Pandoc's power.

Pandoc is great, but remembering all its options every time you want to do that is pretty much tedious.
So this is a simple wrapper that adds some basic options for you.
And of course, you may tweak it as required!

The idea came from the great rendering done by the  "Markdown PDF" VSCode extension (see https://github.com/yzane/vscode-markdown-pdf).
Unfortunately, this extension cannot be "batched" outside VSCode, and I wanted something running from CLI.
The goal is to produce something as neat (not yet... working on it!)

## Install & usage

Install: clone repos, then:
```
$ cd md2pdf
$ sudo ./install
```

This installs the script in a folder that should already be in your $PATH (`/usr/local/bin`).

Usage:
```
$ md2pdf myfile.md
```




## Technical details

Converting from Markdown to pdf using Pandoc needs to be done in two steps, but you have the choice of two options:
* convert Markdown to LaTeX, then from LaTeX to pdf
* convert Markdown to HTML(5), then from HTML to pdf

This scripts uses the second path, as it is much easier to tweak the appearance using CSS.
This can be done by adding the required CSS styles in the included Pandoc template.

## Requirements

bash + pandoc

## References
* https://pandoc.org/

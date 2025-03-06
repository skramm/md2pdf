# md2pdf
A pandoc-based Markdown to pdf script

* Sebastien Kramm, 2023 - 2025
* https://github.com/skramm/md2pdf

## News
* 20250306:
  * added (optional) feature: print page numbers ("page X/Y") on output pdf file.
  Requires pdfcpu (https://pdfcpu.io/)
* 20240102:
  * now use the [Commonmark](https://spec.commonmark.org/) Pandoc option, to use the Commonmark dialect (see https://commonmark.org/).
* 20241102:
  * cleanout, added some colors
* 20240112:
  * added .desktop file for easy desktop integration
* 20231113:
  * Added checking for the presence of wkhtmltopdf and pandoc
  * Added pandoc error code checking

## Rationale

This a simple bash script that converts a Markdown file to a (beautiful) pdf, using Pandoc's power.

Pandoc is great, but remembering all its options every time you want to do that is pretty much tedious.
So this is a simple wrapper that adds some basic options for you.
And you can easily tweak it as required.

The idea came from the great rendering done by the "Markdown PDF" VSCode extension<br>
(see https://github.com/yzane/vscode-markdown-pdf).
Unfortunately, it seems that this extension cannot be "batched" outside VSCode, and I wanted something running from CLI or desktop.
The goal is to produce something as neat.

## Install & usage

### Install:
clone repo, then:
```
$ cd md2pdf
$ sudo ./install
```

This installs the script in a folder that should already be in your $PATH (`/usr/local/bin`), and the additional files in `/usr/local/share/md2pdf/`.

### Usage:
```
$ md2pdf myfile.md
```
This will generate a file `myfile.pdf` in local folder.

But the script also accepts a path in filename, thus these are fine:
```
$ md2pdf "../../some file.md"
$ md2pdf location/to/some-file.md
```

For KDE/Gnome and related desktop environnments, a `.desktop` file is also provided and installed, so it is now possible to use this directly with a right-clic on a .md file.

This might require to run `$ sudo update-desktop-database` to get it available into your file explorer.


## Technical details

### 1 - Steps
Converting from Markdown to pdf using Pandoc needs to be done in two steps, but you have the choice of two options:

* convert Markdown to LaTeX, then from LaTeX to pdf
* convert Markdown to HTML(5), then from HTML to pdf using wkhtmltopdf

The first method requires a complete LaTeX environment installed on your system, which isn't that frequent for most users.
Thus, this scripts uses the second method.
Moreover, it is much easier to tweak the appearance using CSS.
This can be done by adding the required CSS styles either in the included Pandoc template, or in the included CSS stylesheet
(that are located after install in /usr/local/share/md2pdf/).

in case of trouble, you can check the standard output files:  
`/tmp/md2pdf/md2pdf_FILENAME.stdout` and `/tmp/md2pdf/md2pdf_FILENAME.stderr`

**New feature**  
With the help of pdfcpu, the output file now has the page numbers printed on bottom of page.
This is done by a post-processing step, pdfcpu takes the Pandoc output file and adds on each page the page numbers.


### 2 - Processing sets of file

You can use this to process a set of files lying in subfolders by using the `find` command:
```
find . -iname "*.md" -exec md2pdf {} \;
```

(`-iname` is used here because some people use `.MD` instead of `.md`, but the script will accept both)


### 3 - Current folder

The script changes the current folder for the one where the file stays.
This is needed so that images that could be referenced in sub-folders are correctly embedded in the output pdf file.
This is done with `pushd` and restored with `popd`.

## Requirements (aka "known to work with" )

* pandoc: 2.5
* wkhtmltopdf: 0.12.5
* pdfcpu (optional): 0.9.1

## References
* https://pandoc.org/
* https://wkhtmltopdf.org/
* https://pdfcpu.io/ - https://github.com/pdfcpu/pdfcpu

## TODO

* add a CI Github worker to generate online a demo pdf


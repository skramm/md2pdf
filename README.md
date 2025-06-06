# md2pdf
A pandoc-based Markdown to pdf script

* Sebastien Kramm, 2023 - 2025
* https://github.com/skramm/md2pdf

## News
* 20250321:
  * added ability to provide own css file
* 20250310:
  * added option to print on Landscape
* 20250306:
  * added (optional) feature: print date and page numbers ("page X/Y") on output pdf file.
  Requires pdfcpu (https://pdfcpu.io/) (see below)
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

I am a heavy LaTeX user, that I (used to) use for everything I need to write.
But recently, I tend to adopt a lot Markdown, for short notes or documents that don't require fancy formatting.
Faster to type, no bothering with compiling, handling errors and so.
And for git-hosted stuff, it gets automatically nicely converted to HTML pages.

But sometimes you only want to sent it through email, or just print it.
In that case, pdf becomes the best way to go.
And Pandoc is then the right tool for that.

Pandoc is great, but its flexibility requires a lot of options to achieve exactly your usecase, remembering all its options every time you want to do that is pretty much tedious.
So this is a simple wrapper-script that adds all the required options to convert a Markdown file to a (beautiful) pdf, using Pandoc's power

And you can easily tweak it as required.

The idea came from the great rendering done by the "Markdown PDF" VSCode extension<br>
(see https://github.com/yzane/vscode-markdown-pdf).
Unfortunately, it seems that this extension cannot be "batched" outside VSCode, and I wanted something running from CLI or desktop.
The goal is to produce something as neat.

It can be used as a command-line tool, but also from your desktop, just with a right-clic on a .md file.

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

Passing the `-L` option will generate a landscape pdf:
```
$ md2pdf -L myfile.md
```

For other tweaks of the pdf appearance, you can provide a CSS file named `md2pdf.css` in current folder.
It will then be used in replacement of the default one.

The best idea is to copy the default one (`/usr/local/share/md2pdf/md2pdf.css`) into the folder holding your .md file, then edit it.
For example, increasing font size can be done with:
```
body{font-size:120%;}
```

Refer to some
[CSS references](https://www.google.com/search?q=css+reference)
for other tweaks.

For KDE/Gnome and related desktop environnments, a `.desktop` file is also provided and installed, so it is now possible to use this directly with a right-clic on a .md file.
This might require to run `$ sudo update-desktop-database` to get it available into your file explorer.


### Optional: print date and page number

The script will automatically detect if 
pdfcpu (https://pdfcpu.io/) is installed on your system.
If so, it will be used to print current date and page number on the bottom of each page.

## Technical details

### 1 - Steps
Converting from Markdown to pdf using Pandoc needs to be done in two steps, but you have the choice of two options:

* convert Markdown to LaTeX, then from LaTeX to pdf
* convert Markdown to HTML(5), then from HTML to pdf using wkhtmltopdf

The first method requires a complete LaTeX environment installed on your system, which isn't that frequent for most users.
Thus, this scripts uses the second method.
Moreover, it is much easier to tweak the appearance using CSS.
This can be done either by editing the default CSS stylesheet
(located in `/usr/local/share/md2pdf/`) or by having in the current folder a file named `md2pdf.css` that will be used by the script.

in case of trouble, you can check the standard output files:  
`/tmp/md2pdf/md2pdf_FILENAME.stdout` and `/tmp/md2pdf/md2pdf_FILENAME.stderr`

**Page numbers**  
With the help of pdfcpu, the output file now has the page numbers printed on bottom of page.
This is done by a post-processing step, pdfcpu takes the Pandoc output file and adds on each page the page numbers.
See link below for install steps.

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

### References
* https://pandoc.org/
* https://wkhtmltopdf.org/
* https://pdfcpu.io/ - https://github.com/pdfcpu/pdfcpu

The two first ones can be easily installed with:  
`$ sudo apt install pandoc wkhtmltopdf`  
For `pdfcpu`, a binary is provided on the website.



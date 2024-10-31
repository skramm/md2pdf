# md2pdf
A pandoc-based Markdown to pdf script

* Sebastien Kramm, 2023/06
* https://github.com/skramm/md2pdf

## News

* 20241031:
  * removed pushs/popd: now works from current folder
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

For KDE/Gnome and related desktop environnments, a .desktop file is also provided and installed, so it is now possible to use this directly with a right-clic on a .md file.

This might require to run `$ sudo update-desktop-database` to get it available into your file explorer.


## Technical details

Converting from Markdown to pdf using Pandoc needs to be done in two steps, but you have the choice of two options:

* convert Markdown to LaTeX, then from LaTeX to pdf
* convert Markdown to HTML(5), then from HTML to pdf using wkhtmltopdf

The first method requires a complete LaTeX environment installed on your system, which isn't that frequent.
Thus, this scripts uses the second method.
Moreover, it is much easier to tweak the appearance using CSS.
This can be done by adding the required CSS styles either in the included Pandoc template, or in the included CSS stylesheet
(that are located after install in /usr/local/share/md2pdf/).

in case of trouble, you can check the standard output files
`/tmp/md2pdf/md2pdf_FILENAME.stdout` and `/tmp/md2pdf/md2pdf_FILENAME.stderr`

## Requirements (aka "known to work with" )

* pandoc: 2.5
* wkhtmltopdf: 0.12.5

## References
* https://pandoc.org/
* https://wkhtmltopdf.org/

## TODO

* add a CI Github worker to generate online a demo pdf


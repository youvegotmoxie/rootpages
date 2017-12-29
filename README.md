# Root Pages

Root Pages is a collection of quick and easy-to-reference tutorials, examples, and guides primarily for Linux and other UNIX-like systems. The main goal of this project is to provide a more general purpose alternative to the "info" documents for System Administrators, Engineers, and Architects. Each page is a full guide to each topic. This makes it easy to navigate and search for content.

All Root Pages are written in the reStructuredText (RST) markup language. Sphinx is used for building the documentation into ePub, HTML, PDF, and other common document formats.


## Install

Sphinx is a Python package that is required to compile Root Pages into different document formats. It is provided by most operating systems' package manager. The official installation guide can be found [here](http://www.sphinx-doc.org/en/stable/install.html).

Arch Linux:

```
$ sudo pacman -S python-sphinx
```

Debian:

```
$ sudo apt-get install python3-sphinx
```

Fedora:

```
$ sudo dnf install python3-sphinx
```


## Usage

Sphinx will save newly generated documents into the "build/" directory by default.

### Usage - Automatic


ePub:
```
$ make epub
```

HTML:
```
$ make html
```

PDF:
```
$ make latexpdf
```

Text:
```
$ make text
```


### Usage - Manual

The "sphinx-build" command is more flexible by being able to add additional command-line arguments to it. It also does not require the "make" system package to be able to process and execute the Makefile.

Specify the format that the reStructuredText should be transformed and output into, and both the source and build directories.

```
$ sphinx-build -b <OUTPUT_FORMAT> src/ build/
```


## Contributing

Root Pages is a rolling release. As new information is committed, it is shortly pushed into master after a quick review for technical writing standards and correct citation usage.

All updates should be added to the reStructuredText files in the "src/" directory of this project. These documents are currently in English and translations are always welcome!

A few quick notes about technical documentation:

* Everything should be written in the third-person narrative.
* Sentences should be easy-to-read and quick to the point.
* Chicago style citation to sources is REQUIRED.
    * If assistance is required with the syntax of new citations, check out [Citation Machine](http://www.citationmachine.net/chicago).

Recommended text editors:

* Graphical User Interface (GUI):
    * [Atom](https://atom.io/) with the [rst-snippets plugin](https://atom.io/packages/rst-snippets).
* Command-Line Interface (CLI):
    * [Vim](https://github.com/vim/vim) with the [vim-rst-tables plugin](https://github.com/nvie/vim-rst-tables).


## Legal


### License

Root Pages, and all of it's content, is provided under the GPLv3. All information should be free information. Knowledge is power and everyone deserves to learn and grow to their fullest potential.


### Plagiarism

Root Pages strives to provide proper citation to the original authors to give credit where due. If there are any reports of plagiarism, please [open a new GitHub issue](https://github.com/ekultails/rootpages/issues) for it to get addressed.

+++
title = "Recommended GitHub Atom Packages"
slug = "recommended-atom-packages"
date = "2017-02-27T22:07:28-03:00"
categories = ["general"]
author = "Daniel Fuentes"
draft = false
lang = "en"
+++

I’ve been using `GitHub’s Atom editor <https://atom.io>`_ for the past couple
of months. I don't completely love it (there are elements that I miss from my
experiences with Vim: startup speed, maturity of some core editing components,
not being able to run it in a terminal) but I'm using Atom though because the
way the editor is built (built on `Electron <http://electron.atom.io/>`_)
allows for some very cool and unique experiences.

By default Atom comes with a few built-in extensions/packages available.
However, for the purposes of development you will definitely need other
packages and the community has been quick to fill in that gap.

In this post I have picked some useful and/or "most interesting" Atom packages
I've found. Interesting in this case means that they really only could have
been done as they are in Atom or a similarly flexible editor.
<!--more-->

Atom Config
===========

First, open the Atom Preferences window (Go to
``Edit menu > Preferences > Settings``).

Use spaces instead of tabs.
---------------------------

Scroll down the Settings panel until you see the Soft Tabs option. Make sure
it’s checked. This setting will convert tabs into spaces automatically.

Set the tab length to 4 spaces
------------------------------

Set tabs length to 4 spaces and turn on showing Invisibles (spaces,
tabs, etc.). Go to ``Edit menu > Preferences > Settings`` and here we can set
Tab Length to 4 and check "Show Invisibles".

Others
------

Next step, hide **\*.pyc** files and other files included in **.gitignore**,
go to ``settings > packages``, search for ``tree-view``, go to package
settings, check 'Hide VCS Ignored Files'.

At the beginning we set tab length to 4 spaces, for me, HTML projects need 2
spaces (because HTML tends to nest very deeply and anything more than two spaces
tends to start pushing HTML off the right edge of an 80-column screen pretty
quickly). So let's set different tab size for HTML. Open your config file (go
to ``Edit menu > config``) and add in the end:

.. code-block:: coffee-script

    ".basic.html.text":
      editor:
        tabLength: 2

Now is time to install some additional plugins and themes
(``Edit > Preferences > Install``).

    Note: You can browse the packages `online <https://atom.io/packages/>`_
    or from Atom’s Preferences (``Edit > Preferences``)

UI and Themes
=============

Themes are subjective, and I’d normally avoid recommending one. However, Seti
makes Atom look gorgeous and includes a wide range of icons for file types
including CSS, LESS, JSON, grunt configuration, gulp configuration and more.

- `Seti UI <https://atom.io/themes/seti-ui>`_

General
=======

Autocomplete-plus
-----------------

Autocomplete-plus started as a community package but has now become part of the
core editor. Just type some stuff, and autocomplete-plus will automatically
show you some suggestions.

Autocomplete plus has flexible visual context options, allowing autocomplete
providers to show the type of the autocomplete option, a brief description, and
potentially a more in depth description. So users can distinguish between
completions offering a snippet, a function, a keyword, or an import.

- `Autocomplete-plus <https://github.com/atom/autocomplete-plus/>`_

Highlight-selected
------------------

When you select a keyword or variable in Sublime Text or Notepad++, all other
instances are shown. Highlight Selected brings the feature to Atom.

- `highlight-selected <https://atom.io/packages/highlight-selected>`_

Pigments
--------

Pigments is a great package for displaying as a highlight of the
hex/gba/rgba/hsl/hsla code. You may have seen hex-color previewers before, but
few match Pigments. It parses colors, understands pre-processor variables and
even executes color-changing functions.

In addition you can display the project’s palette through the Pigments. You can
look through your palette and quickly go to the definition of any individual
color.

- `Pigments <https://atom.io/packages/pigments>`_

Color Picker
------------

Usually, if you want to use a color picker you probably open Photoshop or GIMP
and use the built-in color picker there. This package lets you pick colors in
the atom editor, and it is as easy as a right-click and choosing
``Color Picker``. Alternatively it can be done by pressing ``CMD/CTRL+SHIFT+C``.

Color Picker currently reads HEX, HEXa, RGB, RGBa, HSL, HSLa, HSV, HSVa, VEC3
and VEC4 colors.

- `Color Picker <https://atom.io/packages/color-picker>`_

Minimap
-------

`Minimap <https://atom.io/packages/minimap>`_ is one of Atom’s most popular
packages, displaying a condensed preview of your code for quick navigation.
You can set the position to be on the left or right, turn on/off code
highlights, and more. Minimap even comes with some plugins to extend its
functionality, such as color highlighter.

- `Atom minimap <https://atom.io/packages/minimap>`_

- `minimap-highlight-selected:
  <https://atom.io/packages/minimap-highlight-selected>`_ Highlight-selected
  keyword (or search results) appearing on the minimap.

- `minimap-pigments: <https://atom.io/packages/minimap-pigments>`_ Show the
  pigments colours on the minimap.

Atom Beautify
-------------

Beautify will turn messy (or minify) code into neater and more readable. It has
great support for programming languages, such as HTML, CSS, JavaScript, PHP,
Python, Ruby, Java, C, C ++, C #, Objective-C, CoffeeScript, typescript, etc.

After installing this package, to run it, just right-click and choose
``Beautify editor contents``, or via ``Packages > Atom Beautify > Beautify``.

- `Atom Beautify <https://atom.io/packages/atom-beautify>`_

Linter
------

Atom Linter comes as a "base" of linting plugins for major languages,
providing the top level API for linters. After installing this main package,
you need to install the specific linter for language you work on.

- `Linter <https://atom.io/packages/linter>`_

Atom Alignment
--------------

Highlight your variables assignments, hit ``CTRL + ALT + A`` and this:

.. code-block:: coffee-script

    var a = b;
    var anotherVariable = 12;
    var awesomeModule = require('awesome-module');
    var that = this;

becomes this:

.. code-block:: coffee-script

    var a               = b;
    var anotherVariable = 12;
    var awesomeModule   = require('awesome-module');
    var that            = this;

- `Atom Alignment <https://atom.io/packages/atom-alignment>`_

Web development
===============

Emmet
-----

Emmet (formerly known as Zen Coding) is a plugin available for popular text
editors (ncluding Sublime Text, Visual Studio, Eclipse, Atom, etc.) that let
you write native HTML code without having to directly write HTML tags, instead
use Emmet’s shortcuts. For example you would type this string into your editor:

.. code-block:: html

    div#content>ul#nav>li*4>a

And then hit the "Expand Abbreviation" key (default the tab key). The code is
magically transformed into valid HTML:

.. code-block:: html

    <div id="content">
      <ul id="nav">
        <li><a href=""></a></li>
        <li><a href=""></a></li>
        <li><a href=""></a></li>
        <li><a href=""></a></li>
      </ul>
    </div>

- `Emmet <https://atom.io/packages/emmet>`_

Autoclose-html (or Less-Than-Slash)
-----------------------------------

When writing HTML, Atom doesn’t automatically match (close) your tags. For
example, type ``<div>`` one might expect the corresponding ``</div>`` tag to be
added as well but it is not supported out of the box. This package that
functionality into atom.io and personally find this very useful and makes
writing HTML much faster

- `Autoclose-html: <https://atom.io/packages/autoclose-html>`_ Close the open
  tag when ``>`` is typed.

- **Alternative** `less-than-slash: <https://atom.io/packages/less-than-slash>`_
  closing open tags when less-than, slash ``</`` is typed

Uglify
------

This package is the opposite of atom-beautify, it is designed to minify
JavaScript files.

- `Atom-uglify <https://atom.io/packages/uglify>`_

- **Alternative** `Atom-minify: <https://atom.io/packages/atom-minify>`_
  Minifies JS and CSS files

linter-csslint
--------------

CSS Lint error reports for your Atom editor

- `linter-csslint <https://atom.io/packages/linter-csslint>`_ (Require Linter)

less/sass-autocompile
---------------------

Automatically compiles LESS/SASS files on save or via shortcut.

- `less-autocompile <https://atom.io/packages/less-autocompile>`_

- `sass-autocompile <https://atom.io/packages/sass-autocompile>`_

Python
======

script
------

Run code/scripts in Atom!, based on file name, a selection of code, or by line
number. Suport Python, Ruby, Ruby on Rails, Perl, php, java, C/C++, Haskell,
Shell Script an a big etc.

- `Script <https://atom.io/packages/script>`_

linter flake8 and pydocstyle
----------------------------

Next, we’re going to install a Python Linter package, to help us detect errors
in our Python code. This package is called linter-flake8 and it’s an interface
to flake8.

- `linter-flake8 <https://atom.io/packages/linter-flake8>`_

If you installed the linter-flake8 package, you already have automatic PEP8
validation but another package is missing to validate docstrings according to
the semantics and conventions in PEP 257. This is solved with linter-pydocstyle
which can be used side-by-side with the flake8 linter.

- `linter-pydocstyle <https://atom.io/packages/linter-pydocstyle>`_

Bonus
=====

- `Expose <https://atom.io/packages/expose>`_ Is a file management tool modeled
  after Mac OSX's expose feature. With it, you can instantly display all open
  files as small thumbnails, and switch quickly between them using the keyboard.

- `Asteroids <https://atom.io/packages/asteroids>`_ Spawn an Asteroids shooter
  on any page and then blast away your code.

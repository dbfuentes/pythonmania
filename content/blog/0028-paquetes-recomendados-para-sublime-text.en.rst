+++
title = "Recommended Sublime Text Packages"
slug = "recommended-sublime-text-packages"
date = "2019-05-01T23:45:28-03:00"
categories = ["general"]
author = "Daniel Fuentes"
draft = false
lang = "en"
+++

Some time ago I made a list of `recommended packages for atom,
<https://www.pythonmania.net/en/2017/02/27/recommended-atom-packages/>`_ now I
will do the same for `Sublime Text 3 (ST3) <http://www.sublimetext.com/>`_.

Sublime Text 3 (ST3) is a lightweight, proprietary cross-platform source code
editor, editor very similar to atom (supports plugins, typically
community-built) and known for ease of use, strong community support [1]_ and
it's pretty fast (much better than atom in opening, closing, searching, etc. is
very smooth and fast since it is written in C ++ and Python for plugins).

General information
===================

1. URL: `www.sublimetext.com <http://www.sublimetext.com/>`_
2. Developer(s): Jon Skinner (former Google Engineer) and Will Bond
3. Platforms: mac OSX (10.7 or later), Windows and GNU/Linux
4. License: Proprietary software

- unlimited free trial, with pop-up remembering to buy (like winrar does).
- license costs $80 at the time of writing this post (One-time payment).

It’s an incredible editor right out of the box, but the real power comes from
the ability to enhance its functionality using Package Control and creating
custom settings.

In this post I have picked some useful and/or "most interesting" ST3 packages
I've found.
<!--more-->

Built-in Features
=================

Sublime Text 3  has some features included by default (that in other editors we
have to add them with plugins), among them the most important we have:

1. **Minimap:** display a condensed preview of your code for quick navigation.
#. **Split Layouts:** allow you to arrange your files in various split screens.
   This is useful when you are doing test driven development (Python code on
   one screen, test scripts on another) or working on the front end (HTML on
   one screen and CSS/JavaScript in another).
#. **Vintage Mode:** provides you with vi commands for use within ST3.
#. Automatic loading of the last session re-opens all files and folders you had
   open when you closed the editor the last time.
#. **Code Snippets:** giving you the ability to create common pieces of code
   with a single keyword. There are a number of default snippets. For example,
   open a new file, type  ``lorem``, and press Tab. You should get a paragraph
   of lorem ipsum text.

Note: You can also create your own snippets: ``Tools`` > ``New Snippet``. Refer
to `the documentation for help
<http://docs.sublimetext.info/en/latest/extensibility/snippets.html>`_.

SublimeText Custom Settings
===========================

You can fully configure Sublime Text using JSON-based settings files (for the
base and language-specific settings), so it’s easy to transfer or synchronize
your customized settings to another system.

General settings
****************

First, we need to create our base customized settings. To set up a base file,
in Sublime Text click ``Preferences`` > ``Settings - User``. This opens the
default preferences (on the left) and a empy JSON object to write our
personalized preferences (on the right, called
``Preferences.sublime-settings - User``).

For example this is my (mimimalist) ``Preferences.sublime-settings - User``:

.. code-block:: json

    // Settings in here override those in "Default/Preferences.sublime-settings"
    // and are overridden in turn by syntax-specific settings.
    {
        // Columns in which to display vertical rulers
        "rulers": [80],
        // Tabulation options.
        "tab_size": 4,
        "translate_tabs_to_spaces": true,
        "draw_white_space": "all",
        // Highlights.
        "draw_indent_guides": true,
        "highlight_line": true,
        // Used when saving new files
        "default_encoding": "UTF-8",
        "ensure_newline_at_eof_on_save": true,
    }

In this configuration we established the following setings:

1. The ``"rulers": [80]`` insert a vertical ruler in the column 80. For the
   oldest coding practice is to keep line width 80 chars (better for diff and
   small terminal screens).
2. The ``"tab_size": 4`` to set tabs length to 4 spaces (common in some
   languages like Python) and the ``"translate_tabs_to_spaces": true`` will
   convert tabs into spaces automatically when tab key |tab-key| is pressed.
3. The ``draw_white_space": "all"`` is to show invisibles (spaces,tabs, etc.)
   in the text. Other possible values: ``"selection"`` to draw the white space
   only in the selected text and ``"none"`` to turn off.
4. Others: ``"default_encoding": "UTF-8",``set the default coding for new
   files and ``"ensure_newline_at_eof_on_save": true`` is useful in unix
   systems. [2]_

Language specific settings
**************************

In sublime text you can set a specific configuration for each language
independently.

For language specific settings, click in Sublime Text: ``Preferences`` >
``Settings - More`` > ``Syntax Specific - User``. Then save the file using the
following format: ``LANGUAGE.sublime-settings``.

You can obviously configure your settings to your liking. However, I highly
recommend starting with settings and then making changes as you see fit.

HTML and CSS Preferences:
_________________________

At the beginning we set tab length to 4 spaces, for me HTML and CSS projects
need 2 spaces for indentation (because HTML tends to nest very deeply and
anything more than two spaces tends to start pushing HTML off the right edge of
an 80-column screen pretty quickly).

So let's set different tab size for HTML. In Sublime Text go to:
``Preferences`` > ``Settings - More`` > ``Syntax Specific - User`` to create an
empty configuration. Copy the following configuration to the empty file:

.. code-block:: json

    {
        // tabs and whitespace
        "draw_white_space": "all",
        "tab_size": 2
        // Automatically close HTML and XML tags when </ is entered
        "auto_close_tags": true,
    }

Then save the file as ``HTML.sublime-settings`` (go to ``file`` > ``save as``
and rename the file as ``HTML.sublime-settings`` Save and close it).

For the CSS we are also going to leave the tab length to 2 spaces, for that
go to: ``Preferences`` > ``Settings - More`` > ``Syntax Specific - User`` to
create an empty configuration. Copy the following configuration in the empy
file:

.. code-block:: json

    {
        // tabs and whitespace
        "draw_white_space": "all",
        "tab_size": 2
    }

Then save the file as ``CSS.sublime-settings`` (go to ``file`` > ``save as``
and rename the file, Save and close it).

Python Preferences:
___________________

This is my Python-specific settings. In Sublime Text go to:
``Preferences`` > ``Settings - More`` > ``Syntax Specific - User`` to create an
empty configuration. Copy the following configuration to the empty file:

.. code-block:: json

    {
        // tabs and whitespace
        "draw_white_space": "all",
        "auto_indent": true,
        "smart_indent": true,
        "tab_size": 4,
        "trim_automatic_white_space": true,
        "use_tab_stops": true,
        "word_wrap": true,
        "wrap_width": 80
    }

Save the file as ``Python.sublime-settings`` (go to ``file`` > ``save as``
and rename the file, Save and close it).

A good reference for settings can be found at the `Sublime Text Unofficial
Documentation. <http://sublime-text-unofficial-documentation.readthedocs.org/en/latest/reference/settings.html>`_

Package Control
===============

Now is time to install some additional plugins and themes but for that first we
have to install the package manager called Package Control. Once you have it
installed, you can use it to install, remove, and upgrade other ST3 packages.

Installing Package Control
**************************

To install the Package Control you have two alternatives:

1. If you got a recent build [3]_ of Sublime text 3, go to ``Tools`` >
   ``Install Package Control``
2. If you do not have the previous option (or you are using an old version of
   sublime text) you simply have to open the Sublime Text console (menu
   ``View`` > ``Show Console``), go to the `installation page on their website,
   <https://packagecontrol.io/installation>`_ and copy some strange Python code,
   paste the code into the console, press ``Enter`` |enter-key| and... Presto!
   You can now install any package easily from within Sublime.

After you install it, you’ll be able to get packages right from Sublime Text.
Forget about manually searching and installing stuff!

Package Control Usage
*********************

1. Open the Command Palette: Press ** Ctrl+Shift+P ** (Windows or GNU/Linux) or
   **Cmd (|cdm-mac-key|)+Shift+P** (Mac OS X).
2. Type "Package Control" and select "Package Control: Install Package".
3. A list of available packages will display in the Palette. Double-click on
   the Package name to start installing this package.

UI and Themes
=============

Themes are subjective, and I’d normally avoid recommending one. However, in
Atom I liked the Seti theme and sublime text has a port of this theme. Others
Themes that I personally like are:

- `Seti UI (Port for ST3). <https://packagecontrol.io/packages/Seti_UI>`_
- `Soda Dark Theme <https://packagecontrol.io/packages/Theme%20-%20Soda>`_
- `Ayu Theme <https://packagecontrol.io/packages/ayu>`_
- `Dracula Theme <https://packagecontrol.io/packages/Dracula%20Color%20Scheme>`_
- `Flatland Theme. <https://packagecontrol.io/packages/Theme%20-%20Flatland>`_

After installing a theme (using the Package Control), make sure to update your
base settings through Sublime Text ``Preferences`` > ``Settings - User`` and
add the theme lines in your user settings, for example:

.. code-block:: json

    {
      "theme": "ayu-light.sublime-theme",
      "color_scheme": "Packages/ayu/ayu-light.sublime-color-scheme"
    }


General Packages
================

Like Atom, Sublime has a lot of packages and themes! For me the essentials are:


SideBar Enhancements
********************

SideBarEnhancements extends the number of menu options in the sidebar, adding
some key features to the sidebar like open in browser, copy name, copy path,
etc...speeding up your overall workflow. These kind of features should really
be there by default in any text editor.

- `SideBarEnhancements <https://packagecontrol.io/packages/SideBarEnhancements>`_

Bracket HighLighter
*******************

This plugin gives a great visual hint to where is a tag or bracket ending.
Helps a lot, especially in debugging by highlighting the scope

- `Bracket HighLighter <https://packagecontrol.io/packages/BracketHighlighter>`_

Color HighLighter
*****************

HighLighter is a package for displaying as a highlight of the hex, gba, rgba,
hsl, hsla, etc. code. with their real color. When you click on that particular
code it fills it with color.

In addition y comes with it’s own color picker, just press ctrl +Shift + C and
pick your colour.

- `Color HighLighter (project is abandoned?)
  <https://packagecontrol.io/packages/Color%20Highlighter>`_

- **Alternative:** `Color Highlight
  <https://packagecontrol.io/packages/Color%20Highlight>`_

Code​ Formatter
***************

Code​ Formatter will turn messy (or minify) code into neater and more readable.
It has support for programming languages, such as HTML, CSS, JavaScript, JSON,
PHP, Python and VBScript.

- `CodeFormatter <https://packagecontrol.io/packages/CodeFormatter>`_

Linter
******

Sublime Linter is a framework "base" for ST3 linters plugins for major
languages, providing the top level API for linters. After installing this main
package, you need to install the specific linter for language you work on.

- `Sublime Linter <https://packagecontrol.io/packages/SublimeLinter>`_

Web development Packages
========================

Emmet
*****

Emmet (formerly known as Zen Coding) is a plugin available for popular text
editors (ncluding Sublime Text, Visual Studio, Eclipse, Atom, etc.) that let
you write native HTML code without having to directly write HTML tags, instead
use Emmet’s shortcuts. For example you would type this string into your editor:

.. code-block:: html

    div#content>ul#nav>li*4>a

And then the hit the expand Abbreviation" key (default the Ctrl+e). The code is
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

- `Emmet <https://packagecontrol.io/packages/Emmet>`_

LiveReload
**********

A page reloading plugin for sublime text 3.

- `LiveReload <https://packagecontrol.io/packages/LiveReload>`_

AutoPrefixer
************

Just run this and it will automatically add add every CSS prefix.
Simple and blazing fast!

- `AutoPrefixer <https://packagecontrol.io/packages/Autoprefixer>`_

Minify
******

Minify for Sublime Text allows you to quickly minify and/or beautify CSS,
JavaScript, JSON, HTML and SVG files

- `Minify <https://packagecontrol.io/packages/Minify>`_

- **Alternative** `Minifier: <https://packagecontrol.io/packages/Minifier>`_
  Minifies JS and CSS files

linters CSS and js
******************

CSS and js Lint error reports for your editor (require Sublime Linter)

- `linter css <https://github.com/SublimeLinter/SublimeLinter-csslint>`_
- `linter JSHint <https://packagecontrol.io/packages/JSHint>`_

Python Packages
===============

linter flake8 and pydocstyle
****************************

Next, we’re going to install a Python Linter package, to help us detect errors
in our Python code. This package is called linter-flake8 and it’s an interface
to flake8 (Simply speaking flake8 is "the wrapper which verifies `pep8
<https://github.com/SublimeLinter/SublimeLinter-pep8>`_, `pyflakes
<https://github.com/SublimeLinter/SublimeLinter-pyflakes>`_ and circular
complexity").

- `linter-flake8 <https://packagecontrol.io/packages/SublimeLinter-flake8>`_

If you installed the linter-flake8 package, you already have automatic PEP8
validation but another package is missing to validate docstrings according to
the semantics and conventions in PEP 257. This is solved with linter-pydocstyle
which can be used side-by-side with the flake8 linter.

- `linter-pydocstyle <https://packagecontrol.io/packages/SublimeLinter-pydocstyle>`_

Another interesting package alternative is Pylint which is a tool to verify
modules and packages used for multiple files to finish.

- `linter-pylint <hhttps://github.com/SublimeLinter/SublimeLinter-pylint>`_

Anaconda
********

Another option is Anaconda. It adds a number of IDE-like features to ST3
including the following:

- **Autocompletion:** works by default, but there are a number of configuration
  options.
- **Find Usage:** quickly searches where a variable, function, or class has
  been used in a specific file.
- **Goto Definitions:** finds and displays the definition of any variable,
  function, or class throughout your entire project.
- **Code linting**: uses either PyLint or PyFlakes with PEP 8. I personally use
  a different linting package (mentioned above) so I disable linting altogether
  in the user-defined Anaconda settings file (``Anaconda.sublime-setting``),
  via the file menu: ``Preferences`` > ``Package Settings`` > ``Anaconda`` >
  ``Settings - User`` adding the line ``{"anaconda_linting": false}`` to the
  file, ``Anaconda.sublime-setting``
- **McCabe code complexity checker:** runs the `McCabe complexity checker tool
  <http://en.wikipedia.org/wiki/Cyclomatic_complexity>`_ within a specific file.
- **Show Documentation:** shows the docstring for functions or classes (if
  defined, of course).

Bonus Packages
==============

- `requirementstxt <https://packagecontrol.io/packages/requirementstxt>`_
  provides autocompletion and syntax highlighting as well as a nice version
  management system for your requirements.txt files.
- All `official linter plugins for
  SublimeLinter <https://github.com/SublimeLinter>`_
- `Markdown Preview <https://packagecontrol.io/packages/Markdown%20Preview>`_
  is used for previewing and building markdown files.

**Footnotes:**

.. [1] an example of this is the `unofficial documentation for the Sublime
   Text <docs.sublimetext.info/>`_ that is better than the official.

.. [2] According to POSIX, every text file consists of a series of lines, each
   of which ends with a newline character (``\n``), including the last one.

   Some programs have problems processing the last line of a file if it isn't
   newline terminated. For example GCC warns about it not because it can't
   process the file, but because it has to as part of the standard.

   Reference: `The GCC/GNU mail archive.
   <http://gcc.gnu.org/ml/gcc/2003-11/msg01568.html>`_ and how the POSIX
   standard `defines a line (see 3.206 Line).
   <http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap03.html#tag_03_206>`_

.. [3] SublimeText 3 `Build 3124 o higher.
   <https://www.sublimetext.com/blog/articles/sublime-text-3-build-3124>`_

.. |tab-key| unicode:: U+21B9
.. |cdm-mac-key| unicode:: U+2318
.. |enter-key| unicode:: U+21B5

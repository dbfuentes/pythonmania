+++
title = "Explanation of the underscore in Python"
slug = "underscore-in-python"
date = "2017-03-05T16:43:28-03:00"
categories = ["examples"]
tags = ["python2", "python3"]
author = "Daniel Fuentes"
draft = false
lang = "en"
+++

The underscore (``_``) is special in Python and this post will explain the
about when and how use the underscore ``_`` and help you understand it.

<!--more-->

There are the following cases for using the underscore in Python:

1. To use in Internationalization (i18n) or Localization (l10n).
#. Storing the value of last expression in interpreter.
#. Separate the digits of a number.
#. For ignoring the specific values.
#. Give special meanings (and functions) to name of variables or functions.

Let's explain each case:

1) To use in Internationalization (i18n) or Localization (l10n)
===============================================================

By convention (from the `programming language
C <https://en.wikipedia.org/wiki/C_%28programming_language%29>`_)
the *underscore* (``_``) variable isused to mark the translatable strings for
Internationalization (i18n) or Localization (l10n).

The built-in python library `gettext which is for i18n/l10n uses this
convention <https://docs.python.org/3/library/gettext.html>`_ and Django (a
Python web framework) which supports i18n/l10n also introduces and uses this
convention.

For example (built-in Python library):

.. code-block:: python

    import gettext

    gettext.textdomain("myapplication")
    gettext.bindtextdomain("myapplication", "path/to/my/language/directory")

    _ = gettext.gettext

    print(_("This is a translatable string"))

2) Storing the value of last expression in interpreter
======================================================

The python interpreter stores the last expression value to the special variable
called ``_``. For example used the interpretant in interactive mode:

.. code-block:: python

    Python 2.7.13 (default, Jan 19 2017, 14:48:08)
    [GCC 6.3.0 20170118] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> 5
    5
    >>> _
    5
    >>> _ * 3
    15
    >>> _
    15
    >>> _ + 7
    22
    >>> _
    22

Note:
  This feature has been used in standard CPython interpreter and you could use
  it in other Python interpreters too.

3) Separate the digits of a number
==================================

It is used for separating digits of numbers using underscore for readability.

Note:
  This feature was added in Python 3.6.

.. code-block:: python

    x_dec = 6_543_210
    y_hex = 0x_1234_abcd

    print(x_dec) # 6543210
    print(y_hex) # 305441741

4) Ignoring the specific values
===============================

The ``_`` is used as a throw-away name. If you do not need the specific values
or the values are not used, just assign the values to the underscore and they
will be ignored. For example:

.. code-block:: python

    # Ignore a value when unpacking
    x, _, y = (1, 2, 3)
    print(x) # x = 1
    print(y) # y = 3

    # Extended Unpacking: Ignore multiple values. Available in only Python 3.x
    x, *_, y = (1, 2, 3, 4, 5)
    print(x) # x = 1
    print(y) # y = 5

    # As a throw-away name:
    # 1- you may not be interested in the actual value of a loop counter
    n = 42
    for _ in range(n):
        do_something()
    # 2- Ignore a value of specific location
    for _, val in list_of_tuple:
        do_something()

5) Give special meanings to a name of variables or functions
============================================================

The underscore be most used in "naming" variables or functions. `The
PEP8 <https://www.python.org/dev/peps/pep-0008/#descriptive-naming-styles/>`_
which is Python convention guideline introduces the following 4 naming cases:

- A single underscore After a name (e.g. ``spam_``)
- A single underscore Before a name (e.g. ``_spam``)
- A double underscore Before a Name (e.g. ``__spam``)
- A double underscore Before and After a Name (e.g. ``__spam__``)

A single underscore After a name (e.g. ``spam_``)
-------------------------------------------------

This convention could be used only for avoiding conflict with Python keywords
or built-ins. For Example:

.. code-block:: python

    # Avoid conflict with "class" keyword
    Tkinter.Toplevel(master, class_='ClassName')

A single underscore Before a name (e.g. ``_spam``)
--------------------------------------------------

A single underscore (``_``) before a name is used to specify that the name is
to be treated as **"private"** or **"internal"** variables, functions, methods
or classes. This is a kind of convention so that the next person using your
code knows that a name starting with ``_`` is for internal use and it should
be considered an implementation detail and subject to change without notice.

For Example:

.. code-block:: python

    _internal_version = "1.0" # private variable

    def _get_double(x): # private method
        _factor = 2 # private variable
        return x * _factor


Anything with this convention are ignored in ``from module/package import *``
(unless the module's/package's ``__all__`` list explicitly contains them).

However, Python does **not supports** truly `private variables
<https://docs.python.org/3.6/tutorial/classes.html#tut-private>`_, so we can
not force somethings private and  it still is possible to access or modify a
variable that is considered private. This can even be useful in special
circumstances, such as in the debugger.

A double underscore Before a Name (e.g. ``__spam``)
---------------------------------------------------

The double underscore (``__``) when naming a class attribute, invokes name
mangling to the interpreter. Python will mangle the attribute names of a class
(modify the variables or function names with some rules, not use as it is) to
avoid conflicts (clashes) of attribute names defined by subclasses.

As the `python documentation points
out <https://docs.python.org/3.6/tutorial/classes.html#tut-private>`_,
any identifier of the form ``__spam`` is textually replaced with
``_classname__spam``, where classname is the current class name with leading
underscore(s) stripped.

If we take the following example (In the interpreter):

.. code-block:: python

    Python 2.7.13 (default, Jan 19 2017, 14:48:08)
    [GCC 6.3.0 20170118] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> class A(object):
    ...   def _internal_use(self):
    ...      pass
    ...   def __method_name(self): # for mangling
    ...      pass
    ...
    >>> dir(Test())
    ['_A__method_name', '__class__', ... , '_internal_use']

The ``_internal_use`` doesn't change, but the ``__method_name`` is mangled to
``_ClassName__method_name`` (``_A__method_name``). Now, if you create a
subclass of A, named B then you can't easily override A's ``__method_name``:

.. code-block:: python

    >>> class B(A):
    ...   def __method_name(self): # for mangling
    ...      pass
    ...
    >>> dir(B())
    ['_A__method_name', '_B__method_name', '__class__', ... , '_internal_use']

Note:
  The intended behaviour here is almost equivalent to ``final`` methods in Java.

A double underscore Before and After a Name (e.g. ``__spam__``)
---------------------------------------------------------------

This convention is used for `special method
names <https://docs.python.org/3.6/reference/datamodel.html#specialnames>`_
used by Python (so-called "magic method", for example: ``__init__``,
``__len__``, etc.). These methods provides special syntactic features or does
special things, for example ``__file__`` indicates the location of Python file.

This is just a convention, a way for the Python system to use names that won't
conflict with user-defined names. You then typically override these methods
and define the desired behaviour for when Python calls them. For example, you
often override the ``__init__`` method when writing a class.

.. code-block:: python

    >>> class C(object):
    ...   def __init__(self, a): # override the __init__ method
    ...     self.a = a
    ...   def __mine__(self): # custom special method
    ...     pass
    ...
    >>> dir(C(1))
    [ '__getattribute__', '__hash__', '__init__', '__mine__', ... , 'a']

+++
title = "Comparison in Python: == vs. is"
slug = "comparison-in-python"
date = "2019-01-01T23:59:58-04:00"
categories = ["examples"]
tags = ["python2", "python3"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

When making comparisons of variables you can use both ``==`` and ``is``,
although they do not work in the same way. In general, we have:

- You use ``==`` to compare values. It is used when you want to know if two
  variables have the same value.
- You use  ``is`` to compare objects. It is used when you want to know if two
  variables (literally) refer to the same object.

For example:

<!--more-->

.. code-block:: python

    >>> a = 19999990
    >>> b = 19999989 +1
    >>> a is b
    False
    >>> a == b
    True
    >>> c = 500
    >>> d = 500
    >>> c is d
    False
    >>> c == d
    True

In this case ``a`` is not the same object as ``b`` but they have the same value.

**Note:** Because of the way the CPython reference implementation works, you'll
get inconsistent and unexpected results if you mistakenly use ``is`` to compare
for equality on integers. The reference implementation of Python caches integer
objects in the range (-5 to 256 inclusive) as singleton instances for 
performance reasons.

Here's an example demonstrating this:

.. code-block:: python

    >>> e = 250
    >>> f = 250
    >>> e is f
    True
    >>> e == f
    True
    >>> g = 257
    >>> h = 257
    >>> g is h
    False
    >>> g == h
    True
    >>>

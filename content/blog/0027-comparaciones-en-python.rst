+++
title = "Comparaciones en Python: == contra is"
slug = "comparaciones-en-python"
date = "2019-01-01T23:55:03-04:00"
categories = ["ejemplos"]
tags = ["python2", "python3"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Al realizar comparaciones de variables se pueden utilizar tanto ``==`` como
``is``, aunque no funcionan de la misma manera. De manera general se tiene:

- Se usa ``==`` para comparar valores. Se usa cuando se desea saber si dos
  variables tienen el mismo valor.
- Se usa ``is`` para comparar objetos. Se usa cuando se desea saber si dos
  variables se refieren literalmente al mismo objeto.

Por ejemplo:

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

En este caso ``a`` no es el mismo objeto que ``b`` pero tienen el mismo valor.

**Nota:** Debido a la forma en que funciona la implementación de referencia de
CPython en enteros pequeños se pueden obtener resultados inconsistentes e
inesperados si se usa ``is``. Esto se debe a que la implementación de
referencia de CPython cachea los enteros entre **-5** a **256** (ambos
incluidos) como una sola instancia para mejorar el rendimiento (provocando
resultados erroneos).

Por ello si probamos con alguno de estos números ocurre lo siguiente:

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

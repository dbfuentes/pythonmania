+++
title = "Yield en python"
slug = "yield-en-python"
date = "2009-02-10T11:49:00-04:00"
categories = ["ejemplos"]
tags = ["python2"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Mientras miraba un poco un ejemplo, vi que se usaba el comando yield en
un par de oportunidades (que se usan para crear generadores).
Básicamente los generadores se escriben funciones normales, pero usan la
sentencia yield en vez de un return dentro de un bucle. Yield funciona
de manera similar al return, pero la gracia de usar el yield es que
conserva la iteración del bucle para la siguiente vez que se le invoque,
esto queda mas claro con un ejemplo, así que abrimos el interprete en
modo interactivo para hacer el siguiente ejemplo:

<!--more-->

.. code-block:: python

    >>> def contador(max):
    ...   n=0
    ...     while n < max:
    ...       yield n
    ...       n +=1
    ...
    >>> gen=counter(3)
    >>> gen.next()
    0
    >>> gen.next()
    1
    >>> gen.next()
    2
    >>> gen.next()
    3
    >>> gen.next()
    Traceback (most recent call last):
      File "<stdin>", line 1, in ?
    StopIteration
    >>>

Al invocar el "contador" que definimos, este devuelve un objeto de
tipo Generator. Este objeto tiene el método next() que permite ir
obteniendo valores del bucle cuando se necesitan y cuando los
generadores terminan su ejecución, hacen saltar la excepción
StopIteration automáticamente.

Además, podemos hacer varias cosas, como por ejemplo usar el yield
dentro de un bucle infinito (en el siguiente ejemplo seria el while
True) de esta manera cuando termina de iterar vuelve a empezar. En este
caso voy a hacer que recorra los elementos de un lista.

.. code-block:: python

    >>> def recorre(list):
    ...   while True:
    ...     for n in list:
    ...       yield n
    ...
    >>> gen=recorre([1, "a", 2, "hola"])
    >>> gen.next()
    1
    >>> gen.next()
    'a'
    >>> gen.next()
    2
    >>> gen.next()
    'hola'
    >>> gen.next()
    1
    >>> gen.next()
    'a'
    >>>

Y podríamos continuar así de manera indefinida (evidentemente la lista
puede contener cualquier cosa). La utilidad de esto es que se pueden
usar iterables fuera de un bucle de una manera sencilla.

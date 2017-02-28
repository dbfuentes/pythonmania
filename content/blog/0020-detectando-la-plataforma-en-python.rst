+++
title = "Detectando la plataforma en python"
slug = "detectando-la-plataforma-en-python"
description = "Explicación de como detectar la plataforma (OS) en que se esta ejecutando un script de Python"
date = "2013-04-06T12:55:00-04:00"
categories = ["ejemplos"]
tags = ["python2", "python3", "sys"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

A mas de alguno le puede servir esto, así que lo posteo. Usando las
`"pilas
incluidas" <http://es.wikipedia.org/wiki/Python#Biblioteca_est.C3.A1ndar>`_
de Python determinaremos mediante sys cual es la plataforma en que se
esta ejecutando el script.

Así que manos a la obra, este código mostrara un mensaje diferente
dependiendo de la plataforma en que se ejecute:

<!--more-->

.. code-block:: python

    #!/usr/bin/python

    import sys

    plataforma = sys.platform

    if plataforma == "linux":
        print("hola tux")
    elif plataforma == "darwin":
        # Estos es la firma de los Mac
        print("la manzana mordida")
    elif plataforma == "win32":
        print("el wintendo")
    else:
        # Para otros sistemas y variantes de Unix
        print("Soy diferente")

Eso si hay un pequeño problema, en versiones de Python anteriores a 3.3
a la respuesta "linux" se le agregaba un sufijo numérico indicando la
versión mayor de kernel, así el resultado podía ser "linux2" o "linux3"
dependiendo del kernel usado (`ver este reporte de
bug <http://bugs.python.org/issue12326>`_), cosa que no sucede con
Python 3.3 o superiores que responde con un "linux" sin ningún numero.

Para arreglarlo en nuestro ejemplo, es mejor comprobando si comienza por
"linux" en vez de si se produce una coincidencia exacta. para esto
podemos usar el método startswith (que es aplicable a una cadena de
caracteres) el cual devuelve True si la cadena comienza por los
caracteres facilitados como parámetro.

así cambiamos la linea 7 de:

.. code-block:: python

    if plataforma == "linux":

por lo siguiente:

.. code-block:: python

    if plataforma.startswith("linux"):

y la detección se realiza correctamente.

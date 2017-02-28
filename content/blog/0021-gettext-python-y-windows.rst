+++
title = "Gettext, Python y Windows"
slug = "gettext-python-y-windows"
description = "Usando Gettext con Python en Windows."
date = "2013-04-19T19:19:00-04:00"
categories = ["ejemplos"]
tags = ["python2", "python3", "gettext"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Hace mucho tiempo escribí `un post sobre como escribir aplicaciones en
python que soporten/muestren varios lenguajes usando gettext
<https://www.pythonmania.net/es/2008/09/10/traducir-aplicaciones-en-python/>`_
(incluso después hice otro `tutorial para crear una aplicación gráfica
multilenguaje
<https://www.pythonmania.net/es/2009/03/15/traducir-una-aplicacion-pygtk-glade/>`_
que se aprovechaba de esto).

Básicamente lo que hace es ocupar el modulo gettext, el cual lee el
lenguaje que utiliza el sistema y a partir de este carga el
correspondiente archivo de traducción (\*.mo) cambiando las cadenas de
texto al idioma del sistema.

Esto funciona de maravilla en sistemas POSIX (linux, la manzana, etc.)
pero trae un pequeño problema en windows, ya que gettext usa por defecto
las `variables de entorno del
sistema <http://es.wikipedia.org/wiki/Variable_de_entorno>`_ (como
LANGUAGE, LC\_MESSAGES, LC\_ALL o LANG) para determinar el idioma que se
utiliza en el equipo, pero estas variables de entorno por no están
presentes en el típico equipo con Windows porque tanto el OS (Windows)
como sus aplicaciones nativas obtienen esa información directamente del
registro (y por lo tanto no están definidas por defecto).

<!--more-->

Es más lo podemos comprobar fácilmente si abrimos el interprete
interactivo en Windows:

.. code-block:: python

    >>>import os
    >>>print(os.getenv('LANG'))
    None

Que regresa un None, ya que por defecto esa variable no existe en
Windows (obviamente si se ejecuta en GNU/Linux o cualquier otro tipo
POSIX, en vez de None regresaría el idioma por defecto). También lo
podríamos comprobar directamente en la `linea de comandos de Windows
usando
SET <http://es.wikipedia.org/wiki/Variable_de_entorno#MS-DOS_y_Windows>`_

Ahora esto tiene una solución bastante simple, usando el modulo locate
(de la biblioteca estándar de Python) se puede obtener el lenguaje y
codificación que usa Windows por defecto en el equipo y con esa
información podemos crear de manera temporal la variable del sistema que
necesita gettext.

Es más, se puede utilizar sys para detectar el sistema usado (`como lo
explique por aquí
<https://www.pythonmania.net/es/2013/04/06/detectando-la-plataforma-en-python/>`_)
y en caso de que sea Windows se crea la variable faltante de forma
temporal (la que necesita gettext para funcionar), el código para
hacerlo seria algo como esto:

.. code-block:: python

    import os
    import sys

    if sys.platform.startswith('win'):
        import locale
        if os.getenv('LANG') is None:
            lang, enc = locale.getdefaultlocale()
            os.environ['LANG'] = lang

Nota: solo lo probé en Windows 7, así que no se si funcionara en Windows
más antiguos (aunque debería funcionar)

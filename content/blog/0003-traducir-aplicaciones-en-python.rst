+++
title = "Traducir aplicaciones escritas en python"
slug = "traducir-aplicaciones-en-python"
date = "2008-09-10T20:19:00-04:00"
categories = ["tutoriales"]
tags = ["python2", "gettext"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Otra de las entradas de mi antiguo blog (batibit.com), esta vez se trata de
una explicación para traducir aplicaciones (crear aplicaciones
multilingües) en python.

**Necesitamos:**

-  gettext

<!--more-->

La Explicación:
===============

Primero hay que tener un programa que vamos a traducir, para el ejemplo
voy a usar el simple y clásico:

.. code-block:: python

    print ("Hello")

Luego hay que modificar un poco el programa para indicarle que textos se
deben traducir, esto se hace al importar el modulo gettext en python, lo
que dejaría el programa como el que sigue:

.. code-block:: python

    import gettext

    gettext.textdomain("programita")
    gettext.bindtextdomain("programita", "/mo")

    print gettext.gettext("Hello")

Esto parece complicado, pero no lo es, lo que se hizo es lo siguiente:

Se uso las funciones "textdomain" y "bindtextdomain" para indicarle
donde se encuentran las traducciones del programa (y el nombre de este).
En este caso como es un ejemplo tienen que cambiar "programita" por el
nombre del programa que se esta traduciendo, y "/mo" por el directorio
donde van a estar las traducón:

Crear la plantilla de traducción (.pot)
---------------------------------------

Lo primero que crearemos va a ser la plantilla general del programa que
tiene la información de las cadenas a traducir, para ello abrimos una
terminal, ingresamos al directorio donde esta nuestro programa y
hacemos:

``xgettext --language=Python -o messages.pot programita.py``

Esto genera una plantilla llamada messages.pot a partir de mi código
fuente (programita.py).

Generar el fichero de traducciones (\*.po)
------------------------------------------

Ahora a partir de nuestra plantilla .pot podemos generar varios ficheros
de traducción (.po) para cada idioma. Como en este ejemplo nos interesa
crear la traducción al español, vamos a crear el fichero es.po (también
podría haber sido ficheros para cada país como es\_ES, es\_CL, es\_AR,
etc).

``msginit -i messages.pot -o es.po``

Esto generara un fichero con los datos de la traducción y alguno de
nuestros datos.

Editando el fichero de traducciones (\*.po)
-------------------------------------------

Ahora viene la parte en la que editamos el fichero es.po para ello se
puede usar algún programa para esto (yo suelo usar el poEdit aunque
existen otros como el gtranslator, etc.) o se puede editar a mano.

Usar un programa es mucho mas cómodo ya que se llena la información de
una manera mas fácil (y casi automática) que haciéndolo a mano, pero
como este es un ejemplo lo voy a hacer a mano, para ello se abre el
fichero .po con un editor de texto como gedit, vim, etc. y se tiene algo
parecido a esto (con algunos datos de quien creo el archivo):

.. code-block:: python

    # Spanish translations for PACKAGE package
    # Traducciones al español para el paquete PACKAGE.
    # Copyright (C) 2007 THE PACKAGE'S COPYRIGHT HOLDER
    # This file is distributed under the same license as the PACKAGE package.
    # NOMBRE DEL AUTOR , 2007.
    #
    msgid ""
    msgstr ""
    "Project-Id-Version: PACKAGE VERSION\n"
    "Report-Msgid-Bugs-To: \n"
    "POT-Creation-Date: 2007-11-13 13:54-0300\n"
    "PO-Revision-Date: 2007-11-13 14:03-0300\n"
    "Last-Translator: NOMBRE DEL TRADUCTOR \n"
    "Language-Team: Spanish\n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"
    "Plural-Forms: nplurals=2; plural=(n != 1);\n"

    #: programita.py:6
    msgid "Hello"
    msgstr ""

Hay que rellenar los datos de faltan, es importante que se señale
charset=UTF-8 o similar ya que de otra manera se podrían tener problemas
con los acentos y similares por la codificación.

Luego hay que indicarle la traducción de las cadenas de texto, para ello
hay que cambiar (en el caso del ejemplo):

.. code-block:: python

    #: programita.py:6
    msgid "Hello"
    msgstr ""

por:

.. code-block:: python

    #: programita.py:6
    msgid "Hello"
    msgstr "Hola"

Y así sucesivamente con el resto de las cadenas (que en este caso solo
era una).

Creación del fichero de equivalencias (\*.mo)
---------------------------------------------

Finalmente los archivos .po tienen que ser compilados en archivos
binario .mo para ser usados, para ello se hace:

``msgfmt es.po -o mo/es/LC_MESSAGES/programita.mo``

Si se fijan el archivo programita.mo lo dejamos en mo/es/LC\_MESSAGES/
esto se debe a que en un principio (en el código fuente) le señalamos
que la traducciones estaría en el directorio /mo, luego el idioma es
Español (por lo que las locales son "es" o sea /mo/es en este caso) y
porque finalmente todos los .mo deben estar en un directorio
LC\_MESSAGES dentro de las locales para que sea encontrado
correctamente.

Ahora el ejecutar nuestro programa si el idioma (las locales) del
sistema son "es" se usara nuestra traducción y si son cualquier otra
(como "en") lo escribirá en inglés. Así de simple.

Usar el antiguo fichero de traducciones (\*.po) con una nueva plantilla (\*.pot)
--------------------------------------------------------------------------------

Seria molesto usar este sistema si cada vez que se actualice el programa
tuviéramos que traducirlo todo de nuevo. Para evitar esto se puede
mezclar una nueva plantilla .pot con un fichero .po traducido
anteriormente.

Por ejemplo mejoramos nuestro programa inicial:

.. code-block:: python

    import gettext

    gettext.textdomain("hello")
    gettext.bindtextdomain("hello", "./mo")

    _ = gettext.gettext

    print _("Hello")
    print _("world")

Fijense que al hacer \_ = gettext.gettext puedo marcar las cadenas a
traducir de manera mas fácil (y se escribe menos).

Ahora creamos una nueva plantilla .pot tal como se lo explico
anteriormente:

`` xgettext programita.py -o nuevo.pot``

Esta plantilla no tiene ninguna linea traducida, así que la vamos a
combinar con el archivo .po que tradujimos anteriormente, para ello
hacemos:

``msgmerge antiguo.po nuevo.pot -o nuevo_es.po``

Donde antiguo.po es el nombre del antiguo fichero .po (el ya traducido
anteriormente) y nuevo.pot es la la plantilla del programa modificado
(la nueva versión) que no tiene ninguna cadena traducida y nuevo\_es.po
es el fichero en donde se combinan los dos anteriores y que en nuestro
ejemplo queda:

.. code-block:: python

    # Spanish translations for PACKAGE package
    # Traducciones al español para el paquete PACKAGE.
    # Copyright (C) 2007 THE PACKAGE'S COPYRIGHT HOLDER
    # This file is distributed under the same license as the PACKAGE package.
    # NOMBRE DEL AUTOR , 2007.
    #
    msgid ""
    msgstr ""
    "Project-Id-Version: PACKAGE VERSION\n"
    "Report-Msgid-Bugs-To: \n"
    "POT-Creation-Date: 2007-11-13 14:43-0300\n"
    "PO-Revision-Date: 2007-11-13 14:03-0300\n"
    "Last-Translator: NOMBRE DEL TRADUCTOR \n"
    "Language-Team: Spanish\n"
    "MIME-Version: 1.0\n"
    "Content-Type: text/plain; charset=UTF-8\n"
    "Content-Transfer-Encoding: 8bit\n"
    "Plural-Forms: nplurals=2; plural=(n != 1);\n"

    #: programita.py:6
    msgid "Hello"
    msgstr "Hola"

    #: programita.py:7
    msgid "world"
    msgstr ""

Si se fijan las cadenas ya traducidas (del la versión previa del
programa) aparecen en el nuevo fichero, por lo que solo hay que traducir
las nuevas cadenas (las que no tenia el programa anterior) tal como se
indico anteriormente.

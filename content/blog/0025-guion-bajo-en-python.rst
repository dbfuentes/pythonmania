+++
title = "Explicacion del guion bajo en Python"
slug = "guion-bajo-en-python"
date = "2017-03-05T16:34:08-03:00"
categories = ["ejemplos"]
tags = ["python2", "python3"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

El carácter de guión bajo (``_``) es especial en Python y esta entrada voy a
explicar cuándo y cómo usar el guión bajo ``_`` y ayudar a entender sus usos.

<!--more-->

En los siguientes casos se usa el guión bajo en Python:

1. Utilizarlo en Internacionalización (i18n) o Localización (l10n).
#. Almacenar el valor de la última expresión en intérprete.
#. Separar los dígitos de un número.
#. Ignorar valores específicos.
#. Dar significados especiales (y funciones) al nombre de variables o funciones.

Vamos a explicar cada caso:

1) Utilizarlo en Internacionalización (i18n) o Localización (l10n).
===================================================================

Por convención (del `lenguaje de programación
C <https://en.wikipedia.org/wiki/C_%28programming_language%29>`_)
La variable *guion bajo* (``_``) se utiliza para marcar las cadenas
traducibles para Internacionalización (i18n) o Localización (l10n).

La biblioteca de python incorporada `para i18n / l10n "gettext" utiliza esta
convención <https://docs.python.org/3/library/gettext.html>`_ y Django (un
web framework en Python) que soporta i18n / l10n, también introduce y utiliza
esta convención.

Por ejemplo (usando biblioteca de Python incorporada gettext):

.. code-block:: python

    import gettext

    gettext.textdomain("miaplicacion")
    gettext.bindtextdomain("miaplicacion", "ubicacion/del/directorio/del/lenguaje")

    _ = gettext.gettext

    print(_("Esta es una cadena traducible"))

2) Almacenar el valor de la última expresión en intérprete.
===========================================================

El intérprete python almacena el valor de la última expresión usada en una
variable especial llamada ``_``. Por ejemplo, utilizando el interpretante en
modo interactivo:

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

Nota:
  Esta característica esta incorporada en el intérprete estándar de CPython y
  se puede utilizar en otros intérpretes de Python también.

3) Separar los dígitos de un número.
====================================

Se puede utilizar el guión bajo para separar los dígitos de los números para
lograr una mayor legibilidad.

Nota:
  Esta característica se agregó en Python 3.6.

.. code-block:: python

    x_dec = 6_543_210
    y_hex = 0x_1234_abcd

    print(x_dec) # 6543210
    print(y_hex) # 305441741

4) Ignorar valores específicos.
===============================

El ``_`` se puede utilizar como un nombre desechable. Si no necesitas algunos
valores específicos o si los valores no se utilizan, sólo hay que asignar los
valores al guión bajo y estos valores serán ignorados. Por ejemplo:

.. code-block:: python

    # Ignorar un valor al desempaquetar
    x, _, y = (1, 2, 3)
    print(x) # x = 1
    print(y) # y = 3

    # Desempaque extendido: ignora varios valores. Disponible solo en Python 3.x
    x, *_, y = (1, 2, 3, 4, 5)
    print(x) # x = 1
    print(y) # y = 5

    # como un nombre desechable:
    # 1- es posible que no esten interesados en el valor de un contador de bucle
    n = 42
    for _ in range(n):
        do_something()
    # 2- Ignorar un valor de ubicacion especifica
    for _, val in list_of_tuple:
        do_something()

5) Dar significados especiales (y funciones) al nombre de variables o funciones
===============================================================================

El guión bajo es más utilizado en "nombrar" variables o funciones. Por el
`PEP8 <https://www.python.org/dev/peps/pep-0008/#descriptive-naming-styles/>`_,
que es la guía de convenciones recomendadas para Python, se indican los
siguientes 4 casos de nomenclatura:

- Un solo guión bajo Después de un nombre (por ejemplo ``spam_``)
- Un solo guión bajo Antes de un nombre (por ejemplo ``_spam``)
- Un doble guión bajo Antes de un nombre (por ejemplo ``__spam``)
- Un doble guión bajo Antes y Después de un nombre (por ejemplo ``__spam__``)

Un solo guión bajo Después de un nombre (por ejemplo ``spam_``)
---------------------------------------------------------------

Esta convención sólo se utiliza para evitar conflictos con palabras clave o
con elementos integradosde de Python. Por ejemplo:

.. code-block:: python

    # Evita el conflico con el nombre clave "class" usado para definir clases
    Tkinter.Toplevel(master, class_='ClassName')

Un solo guión bajo Antes de un nombre (por ejemplo ``_spam``)
-------------------------------------------------------------

Un carácter de guión bajo único (``_``) antes de un nombre se utiliza para
especificar que el nombre debe tratarse como una variable, función, método o
clases **"privado"** o **"interno"**. Este es un tipo de convención para
que la siguiente persona que use tú código sepa que un nombre empezando por
``_`` es para uso interno y debe ser considerado un detalle de implementación
y sujeto a cambios sin previo aviso.

Pr ejemplo:

.. code-block:: python

    _internal_version = "1.0" # variable privada

    def _get_double(x): # metodo privado
        _factor = 2 # variable privada
        return x * _factor

Cualquier cosa que siga esta convención se ignora al hacer un
``from module / package import *`` (A menos que este listado explícitamente en
el contenido del ``__all__`` del módulo / paquete).

Sin embargo, Python **no soporta** `variables verdaderamente
privadas <https://docs.python.org/3.6/tutorial/classes.html#tut-private>`_,
por lo cual no podamos forzar algo verdaderamente privado y es posible acceder
o modificar un variable que se considera privada. Esto puede incluso ser útil
en algunas circunstancias, como al depurar.

Un doble guión bajo Antes de un nombre (por ejemplo ``__spam``)
---------------------------------------------------------------

Al utilizar el doble guión bajo (``__``) al nombrar un atributo de una clase,
esto provoca que el intérprete modifique el nombre. De esta manera Python
manipulará los nombres de una clase (Modifica el nombre de las variables o
funciones con algunas reglas, para que no use como están escritas) y para que
de esta manera se eviten conflictos (choques) con nombres definidos por otras
subclases (o clases hijas).

Como la `documentación de Python
señala <https://docs.python.org/3.6/tutorial/classes.html#tut-private>`_, que
cualquier mombre de la forma ``__spam`` se sustituye por
``_NombreClase__spam``, donde *NombreClase* es el nombre de la clase actual
en donde esta el doble guión bajo.

Si tomamos el siguiente ejemplo (En el intérprete ineractivo):

.. code-block:: python

    Python 2.7.13 (default, Jan 19 2017, 14:48:08)
    [GCC 6.3.0 20170118] on linux2
    Type "help", "copyright", "credits" or "license" for more information.
    >>> class A(object):
    ...   def _internal_use(self):
    ...      pass
    ...   def __method_name(self): # para modificar
    ...      pass
    ...
    >>> dir(Test())
    ['_A__method_name', '__class__', ... , '_internal_use']

Vemos que el ``_internal_use`` no cambia, pero el ``__method_name`` es
modificado, quedando en ``_NombreClase__method_name`` (``_A__method_name``).
Ahora si se crea un clase hija (o subclases) de A, que llamaremos B, entonces
no puede sobrescribir fácilmente el ``__method_name`` de A resultando en:

.. code-block:: python

    >>> class B(A):
    ...   def __method_name(self): # para modificar
    ...      pass
    ...
    >>> dir(B())
    ['_A__method_name', '_B__method_name', '__class__', ... , '_internal_use']

Nota:
  El comportamiento visto aquí es casi equivalente al método ``final`` en Java.

Un doble guión bajo Antes y Después de un nombre (por ejemplo ``__spam__``)
---------------------------------------------------------------------------

Esta convención se utiliza para nombrar `métodos
especiales <https://docs.python.org/3.6/reference/datamodel.html#specialnames>`_
utilizado por Python (también llamados como "métodos mágicos" como por
ejemplo: ``__init__``, ``__len__``, etc.). Estos métodos proporcionan
características sintácticas especiales o hacen cosas especiales al usarlos, por
ejemplo ``__file__`` le indica la ubicación de un archivo a Python.

Esto es sólo una convención, una forma para que Python utilice nombres que no
van a entrar en conflicto con los nombres definidos por el usuario. Uno puede
sobrescribir estos métodos y definir el comportamiento deseado para cuando
Python los llama. Por ejemplo, es usual sobrescribir el método `` __init__``
al escribir una clase.

.. code-block:: python

    >>> class C(object):
    ...   def __init__(self, a): # sobrescribe el metodo __init__
    ...     self.a = a
    ...   def __mine__(self): # metodo especial personalizado
    ...     pass
    ...
    >>> dir(C(1))
    [ '__getattribute__', '__hash__', '__init__', '__mine__', ... , 'a']

+++
title = "Generador de contraseñas aleatoreas"
slug = "generador-de-contrasenas-aleatoreas"
date = "2009-05-02T21:46:00-04:00"
categories = ["ejemplos"]
tags = ["python2", "python3"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Vamos a escribir un generador de contraseñas simple (como las típicas
que te asignan al registrarse en algún sitio o al reiniciar
contraseñas), para eso solo se va a escoger valores al azar obtenidos
desde una cadena prefijada para formar una contraseña de una longitud
prefijada. Para generar/escoger los valores tenemos dos posibilidades:

<!--more-->

Ejemplo 1 : Usando valores pseudoaleatorio (módulo random)
==========================================================

Eso es exactamente, un generador de contraseñas pseudoaleatorio,
rápido y bonito, obviamente no hay nada de criptografía en este.

.. code-block:: python

    #!/usr/bin/env python

    from random import choice

    longitud = 18
    valores = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ<=>@#%&+"

    p = ""
    p = p.join([choice(valores) for i in range(longitud)])
    print(p)

Bastante simple, en **longitud** se le indica la longitud de la
contraseña (en este caso 18 letras) y en **valores** son las posibles
letras con las que se formaran la contraseña (allí cada uno escoge).

Ojo, que como mencione anteriormente el módulo random genera `valores
pseudoaleatorios <https://es.wikipedia.org/wiki/N%C3%BAmero_pseudoaleatorio>`_,
así que no es muy recomendable usarlo si se requiere una seguridad alta,
ya que en teoría un atacante podría deducir como se generaron los
números.

Para generar valores criptográficamente seguros hay que usar los módulos
`os.urandom() <http://docs.python.org/2/library/os.html#os.urandom>`_
o
`SystemRandom() <http://docs.python.org/2/library/random.html#random.SystemRandom>`_,
pero estos tienen algunos inconvenientes: No están disponibles en todos
los sistemas (ya que usan los generadores propios de cada sistema, en
GNU/Linux y otros tipo UNIX usan /dev/urandom y en Windows usan
CryptGenRandom()) y además son un poco más lentos que en módulo random a
secas.

Así usando SystemRandom() se tiene:

Ejemplo 2: Usando SystemRandom()
================================

.. code-block:: python

    #!/usr/bin/env python

    from random import SystemRandom

    longitud = 18
    valores = "0123456789abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ<=>@#%&+"

    cryptogen = SystemRandom()
    p = ""

    while longitud > 0:
        p = p + cryptogen.choice(valores)
        longitud = longitud - 1

    print(p)

Algunas consideraciones:

    **Nota 1:** Si el sistema de contraseña **NO** diferencia entre
    minúsculas y mayúsculas, habría que sacar todas las letras
    mayúsculas (o minúsculas) porque o si no habría el doble de
    posibilidades de escoger las letras vs los símbolos (por ejemplo
    para la **A** se podría escoger 2 valores que serían lo mismo “A” o
    “a”, mientras para un símbolo como el **+** solo habría 1 posible
    valor que es “+”).

    **Nota 2:** puede ser recomendable omitir el 1,l,O,0 porque a veces se
    confunden y omitir algunas letras para que por azar no se creen
    palabras ofensivas.

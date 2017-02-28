+++
title = "Control de bucles, break, continue y pass"
slug = "control-de-bucles-break-continue-y-pass"
date = "2013-04-05T11:26:00-04:00"
categories = ["ejemplos"]
tags = ["python2", "python3"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

voy a explicar como se usan y las diferencias entre break y continue
para terminar/salir de un bucle y de paso para que sirve pass. Así que
vamos con un ejemplo.

<!--more-->

Break
=====

Este funciona tal como el break de C. Break se puede usas en bucles
**for** y **while** y simplemente termina el bucle actual y continua con la
ejecución de la siguiente instrucción, por ejemplo:

.. code-block:: python

    #!/usr/bin/python

    # Primer ejemplo
    for letra in "Python":
        if letra == "h":
            break
        print ("Letra actual : " + letra)

    # Segundo ejemplo
    var = 10
    while var > 0:
        var = var -1
        if var == 5:
            break
        print ("Valor actual de la variable : " + str(var))

    print ("fin del script")

que da como resultado:

.. code-block:: python

    >>>
    Letra actual : P
    Letra actual : y
    Letra actual : t
    Valor actual de la variable : 9
    Valor actual de la variable : 8
    Valor actual de la variable : 7
    Valor actual de la variable : 6
    fin del script

Si se fijan en el primer ejemplo al llegar a la letra "h" simplemente se
termina (rompe) el ciclo (bucle) y se sigue con el segundo ejemplo. En
el segundo ejemplo la variable va disminuyendo su valor hasta que llega
a 5, en donde se termina (rompe) el ciclo (bucle), mostrando el print
final.

Continue
========

Al aparecer un Continue en Python, este regresa al comienzo del bucle,
ignorando todos los estamentos que quedan en la iteración actual del
bucle e inicia la siguiente iteración. Queda más claro con un ejemplo:

Nota: al igual que break, se puede usas en los bucles **for** y **while**

.. code-block:: python

    #!/usr/bin/python

    # segundo ejemplo
    for letra in "Python":
        if letra == "h":
            continue
        print ("Letra actual : " + letra)

    # Primer ejemplo
    var = 10
    while var > 0:
        var = var -1
        if var == 5:
            continue
        print ("Valor actual de la variable : " + str(var))

    print ("fin del script")

que da como resultado:

.. code-block:: python

    >>>
    Letra actual : P
    Letra actual : y
    Letra actual : t
    Letra actual : o
    Letra actual : n
    Valor actual de la variable : 9
    Valor actual de la variable : 8
    Valor actual de la variable : 7
    Valor actual de la variable : 6
    Valor actual de la variable : 4
    Valor actual de la variable : 3
    Valor actual de la variable : 2
    Valor actual de la variable : 1
    Valor actual de la variable : 0
    fin del script

En el primer ejemplo al llegar a la letra "h" simplemente termina esa
iteración (ignorando al print que sigue en la linea 7) y continua con la
siguientes iteraciones (letras o y n) hasta que se termina el ciclo
(bucle).

En el segundo ejemplo la variable va disminuyendo su valor hasta que
llega a 5, en donde se termina esa iteración del ciclo (bucle) y se
continua con las iteraciones que siguen hasta que se termina el bucle y
se llega al print final.

Pass
====

Por ultimo tenemos a pass, que tal como su nombre lo indica es una
operación nula, o sea que no pasa nada cuando se ejecuta. Se utiliza
cuando se requiere por sintaxis una declaración pero no se quiere
ejecutar ningún comando o código. También se utiliza en lugares donde
donde el código irá finalmente, pero no ha sido escrita todavía
(utilizándolo como un relleno temporal, hasta que se escriba el código
final).

Vamos con el ejemplo:

.. code-block:: python

    #!/usr/bin/python

    # Primer ejemplo
    for letra in "Python":
        if letra == "h":
            pass
        print ("Letra actual :" + letra)

    # Segundo ejemplo
    var = 10
    while var > 0:
        var = var -1
        if var == 5:
            pass
        print ("Valor actual de la variable :" + str(var))

    print ("fin del script")

que da como resultado:

.. code-block:: python

    >>>
    Letra actual : P
    Letra actual : y
    Letra actual : t
    Letra actual : h
    Letra actual : o
    Letra actual : n
    Valor actual de la variable : 9
    Valor actual de la variable : 8
    Valor actual de la variable : 7
    Valor actual de la variable : 6
    Valor actual de la variable : 5
    Valor actual de la variable : 4
    Valor actual de la variable : 3
    Valor actual de la variable : 2
    Valor actual de la variable : 1
    Valor actual de la variable : 0
    fin del script

En el primer ejemplo al llegar a la letra h no se ejecuta nada (se
pasa), siguiendo con la ejecución de la siguiente linea (la linea 7). Lo
mismo ocurre con el 5 en el segundo ejemplo

Nota: pass es útil cuando se ha creado un bloque de código, pero ya no
es necesario, por lo que se pueden eliminar las instrucciones dentro del
bloque remplazandolas a todas con una única sentencia pass, de modo que
se mantiene el bloque dentro del programa, pero como no hace nada, no
interfiere con otras partes del código.

Diferencia entre continue y pass
--------------------------------

La diferencia es que tal como lo indica su nombre continue termina la
iteración actual, pero continua con el ciclo, volviendo al inicio del
bucle en la siguiente iteración. En cambio pass simplemente no hace nada
y pasa a la siguiente instrucción.

Por ejemplo si abrimos el interprete:

.. code-block:: python

    >>> for x in (1, 2, 3):
        print (x)
        continue
        print (str(x) + " nuevamente")

    1
    2
    3
    >>> for x in (1, 2, 3):
        print (x)
        pass
        print (str(x) + " nuevamente")

    1
    1 nuevamente
    2
    2 nuevamente
    3
    3 nuevamente

En el primer caso (el del continue): imprime (print) el valor de x que
es 1, llega al continue, actualiza x al valor de 2, imprime el valor de
x que que es 2, llega al continue, actualiza x al valor de 3, imprime el
valor de x que que es 3, llega al continue, y termina el ciclo ya que no
hay más iteraciones (valores) para x.

En el segundo caso (el del pass): imprime (print) el valor de x que es
1, llega al pass (y pasa a la siguiente linea), imprime 1 nuevamente,
actualiza x al valor de 2, imprime el valor de x que que es 2, llega al
pass (y pasa a la siguiente linea), imprime 2 nuevamente, , actualiza x
al valor de 3, imprime el valor de x que que es 3, llega al pass (y pasa
a la siguiente linea), imprime 3 nuevamente, y termina el ciclo ya que
no hay más iteraciones (valores) para x.

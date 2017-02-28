+++
title = "Tutorial Pygame 2: creando una ventana, cargar imagenes y moverlas"
slug = "tutorial-pygame-2-ventana-e-imagenes"
date = "2010-03-25T09:54:00-04:00"
lastmod = "2016-08-15T20:28:01-04:00"
categories = ["tutoriales"]
tags = ["python2", "juegos", "pygame"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

En esta segunda parte `del
tutorial <https://www.pythonmania.net/es/2010/03/23/tutorial-pygame-introduccion/>`_
veremos los siguientes temas:

-  Como crear una ventana
-  El bucle principal de un videojuego (o como saber cuando hay que
   terminar el juego)
-  Usar una imagen como fondo
-  Mostrar una imagen y mover la imagen en la pantalla

A continuación la explicación completa

<!--more-->

Paso 1: Creando una ventana
===========================

Primero Importamos los módulos y definimos dos variables globales (por
metodología se suelen poner en mayúsculas) que son resolución de la
pantalla del juego.

Luego hay que crear la ventana, para ello dentro de la función main()
usamos *pygame.display.set\_mode* y *pygame.display.set\_caption*, que
establecen el tamaño de la ventana y el titulo de esta respectivamente:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/03/25/tutorial-pygame-2-ventana-e-imagenes/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("tutorial pygame parte 2")


    if __name__ == "__main__":
        main()

Ahora lo ejecutamos y sorpresa... se crea la ventana pero inmediatamente
se cierra, eso ocurre porque no hemos definido el bucle o loop principal
del juego, tal como lo muestra este video:

{{< youtube wASkscCttMg >}}


Paso 2: el bucle principal del juego
====================================

Los juegos generalmente ejecutan un bucle infinito (su bucle principal),
lo cual permite que el juego continúe corriendo hasta la que se se
cumpla una condición que lo termine (como por ejemplo que el jugador
pierda todas sus vidas o se rinda), mas o menos lo que muestra este
diagrama:

.. image:: https://pythonmania.files.wordpress.com/2010/03/pygame_bucle_principal.png
   :width: 210px
   :height: 490px
   :target: https://pythonmania.files.wordpress.com/2010/03/pygame_bucle_principal.png
   :alt: bucle principal

En nuestro caso el bucle principal se continuara ejecutando hasta que el
jugador cierre el ventana (o sea que se quede esperando hasta que se
haga click en le botón para cerrar la ventana). Entonces el código del
paso anterior queda de la siguiente manera:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/03/25/tutorial-pygame-2-ventana-e-imagenes/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("tutorial pygame parte 2")

        # el bucle principal del juego
        while True:
            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()


    if __name__ == "__main__":
        main()

Lo que hace es simple, una vez dentro del bucle, recorre la lista que
devuelve *pygame.event.get()* (es una lista con todos los eventos que
registra pygame, como por ejemplo cuando uno presiona una tecla), si
dentro de esa lista esta el evento pygame.QUIT (o sea cerrar la ventana)
el programa ejecuta la orden *sys.exit()* (que termina la ejecución del
programa y es parte del modulo sys, por eso lo importamos), en caso
contrario (que aun no se produzca ese evento) seguirá esperando.

    En
    `loserjuegos <http://www.losersjuegos.com.ar/traducciones/pygame>`_
    esta la documentación de referencia de pygame traducida, entre ella
    la `lista de los
    eventos <http://www.losersjuegos.com.ar/traducciones/pygame/event>`_

Paso 3: Cargando el fondo y una imagen
======================================

Primero para cargar las imágenes usamos *pygame.image.load()*, con ello
se crea un objeto que contiene la `superficie de la
imagen <http://www.losersjuegos.com.ar/traducciones/pygame/image>`_
(aun no la muestra). Luego de esto hay que indicar las posiciones de
esta imagen, para lo cual se usa *blit(imagen, (coordenada\_x,
coordenada\_y))*

Puedes pensar en la ventana de pygame `como un plano en 2
ejes <http://es.wikipedia.org/wiki/Geometr%C3%ADa_anal%C3%ADtica#Localizaci.C3.B3n_de_un_punto_en_el_plano_cartesiano>`_,
con una pequeña diferencia: el origen (coordenadas 0,0) se encuentra en
la esquina superior izquierda de la ventana. Por lo tanto en valor de
las coordenadas en el eje x (horizontal) va aumentando (y es positivo)
mientras se avanza a la derecha, mientras que en el eje y (vertical) el
valor va aumentando (y es positivo) a medida que se avanza hacia abajo
(o sea lo contrario de lo que se acostumbra, para el eje y).

Para que quede más claro, un esquema en que muestra en que posición
cargaremos las imágenes (Nota: pygame siempre usa como referencia la
esquina superior izquierda de las imágenes).

.. image:: https://pythonmania.files.wordpress.com/2010/03/pygame_coordenadas.png
   :width: 480px
   :height: 365px
   :target: https://pythonmania.files.wordpress.com/2010/03/pygame_coordenadas.png
   :alt: coordenadas pygame

Ahora que ya sabemos sobre coordenadas, vamos a insertar un fondo y una
imagen en nuestra ventana, usaremos las siguientes:

- Fondo:

.. image:: https://pythonmania.files.wordpress.com/2010/03/fondo.jpg?w=150
   :width: 150px
   :height: 112px
   :target: https://pythonmania.files.wordpress.com/2010/03/fondo.jpg
   :alt: fondo.jpg (hacer clic para agrandar)

- imagen a mostrar

.. image:: https://pythonmania.files.wordpress.com/2010/03/tux.png
   :width: 90px
   :height: 90px
   :target: https://pythonmania.files.wordpress.com/2010/03/tux.png
   :alt: tux.png (hacer clic para agrandar)

ok, Vamos con el código:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/03/25/tutorial-pygame-2-ventana-e-imagenes/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("tutorial pygame parte 2")

        # cargamos el fondo y una imagen (se crea objetos "Surface")
        fondo = pygame.image.load("fondo.jpg").convert()
        tux = pygame.image.load("tux.png").convert_alpha()

        # Indicamos la posicion de las "Surface" sobre la ventana
        screen.blit(fondo, (0, 0))
        screen.blit(tux, (550, 200))
        # se muestran lo cambios en pantalla
        pygame.display.flip()

        # el bucle principal del juego
        while True:
            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()


    if __name__ == "__main__":
        main()

Si se fijan al cargar el fondo se uso *fondo =
pygame.image.load("fondo.jpg").convert()* mientras que para la imagen de
tux se uso *tux = pygame.image.load("tux.png").convert\_alpha()*. Esto
se debe a que el fondo no necesita tener un color trasparente (un canal
alpha) ya que es la imagen que esta bajo todas las demás, en cambio el
sprite de nuestro pingüino si necesita tener un color trasparente (que
convenientemente ya esta definido en el png ;), lo hice a propósito
cuando lo cree) o de lo contrario al cargar la imagen del pingüino se
vería un feo rectángulo a su alrededor.

    Si quieren saber más sobre sprites, resoluciones y similares les
    recomiendo que lean `este articulo sobre los conceptos básicos para
    programar un
    videojuego <http://www.losersjuegos.com.ar/referencia/articulos/conceptos_basicos>`_
    (pongan ojo en la parte de los sprites).

Luego se hizo *screen.blit(fondo, (0, 0))* y *screen.blit(tux, (550,
200))*, con esto se le indica que cargue la imagen del fondo justo en la
coordenada (0,0) para que cubra toda la pantalla (cualquier otro valor
hubiera dejado el fondo desplazado) y en el caso de tux se le indica que
lo cargue a la derecha.

Paso 4: Moviendo la imagen
==========================

Ahora vamos a hacer que tux se mueva a la izquierda, para ello,
simplemente a su posición en el eje x le vamos a restar -1 en cada ciclo
del bucle principal. Para ello modificamos el código para que quede de
la siguiente manera:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/03/25/tutorial-pygame-2-ventana-e-imagenes/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("tutorial pygame parte 2")

        # cargamos el fondo y una imagen (se crea objetos "Surface")
        fondo = pygame.image.load("fondo.jpg").convert()
        tux = pygame.image.load("tux.png").convert_alpha()

        tux_pos_x = 550
        tux_pos_y = 200

        # Indicamos la posicion de las "Surface" sobre la ventana
        screen.blit(tux, (tux_pos_x, tux_pos_y))
        screen.blit(fondo, (0, 0))
        # se muestran lo cambios en pantalla
        pygame.display.flip()

        # el bucle principal del juego
        while True:
            # le restamos 1 a la coordenada x de tux
            # asi se mueve un poquito a la izquierda
            tux_pos_x = tux_pos_x - 1
            screen.blit(tux, (tux_pos_x, tux_pos_y))
            # se muestran lo cambios en pantalla
            pygame.display.flip()

            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()


    if __name__ == "__main__":
        main()

y el resultado fue un fallo gigantesco:

.. image:: https://pythonmania.files.wordpress.com/2010/03/tutorial_pygame_2_fail.jpg
   :width: 450px
   :height: 353px
   :target: https://pythonmania.files.wordpress.com/2010/03/tutorial_pygame_2_fail.jpg
   :alt: fallo gigantesco

Explicación: lo que paso es que se nos olvido borrar al pingüino de la
pantalla antes de moverlo a su nueva posición, por lo que veremos en
realidad un "rastro" del movimiento mientras dibujamos continuamente al
pingüino en nuevas posiciones. Además nunca le pusimos limite al
movimiento del pingüino, por lo que una vez que alcanzo el borde de la
pantalla, siguió moviéndose hacia la izquierda hacia el infinito y mas
allá... [1]_.

{{< youtube pLzoVb-e5vQ >}}


Paso 5 (final): Refrescando/borrando la pantalla
================================================

La solución mas fácil para borrar el "rastro" que se deja al moverse, es
simplemente redibujar toda la pantalla (fondo incluido) en cada ciclo
del juego, por lo tanto dentro del while, después de actualizar la
posición del pingüino, se redibuja tota la pantalla usando *blit()* y
*pygame.display.flip()*

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/03/25/tutorial-pygame-2-ventana-e-imagenes/
    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("tutorial pygame parte 2")

        # cargamos el fondo y una imagen (se crea objetos "Surface")
        fondo = pygame.image.load("fondo.jpg").convert()
        tux = pygame.image.load("tux.png").convert_alpha()

        tux_pos_x = 550
        tux_pos_y = 200

        # Indicamos la posicion de las "Surface" sobre la ventana
        screen.blit(tux, (tux_pos_x, tux_pos_y))
        screen.blit(fondo, (0, 0))
        # se muestran lo cambios en pantalla
        pygame.display.flip()

        # el bucle principal del juego
        while True:
            # le restamos 1 a la coordenada x de tux y comprobamos
            # que no alcance el borde de la pantalla
            tux_pos_x = tux_pos_x - 1
            if tux_pos_x < 1:
                tux_pos_x = 550

            # Redibujamos todos los elementos de la pantalla
            screen.blit(fondo, (0, 0))
            screen.blit(tux, (tux_pos_x, tux_pos_y))
            # se muestran lo cambios en pantalla
            pygame.display.flip()

            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()


    if __name__ == "__main__":
        main()

Además después de actualizar la *tux\_pos\_x* se coloco un if, de tal
manera de que si esa posición es 0 o negativa (o sea esta fuera de la
pantalla), la *tux\_pos\_x* tome un valor de 550 (550 + 90px que mide de
ancho la imagen de tux = 640 px, o sea que la imagen se muestre al borde
derecho de la pantalla).

{{< youtube kIg9W5Q9oRQ >}}


Bueno eso es todo por ahora, pueden descargar todos estos ejemplos
`desde
aquí <http://sites.google.com/site/dbfuentes/archivos/tutorial-pygame-2.zip?attredirects=0&d=1>`_
(o buscarlo en el repositorio de
`github <http://github.com/dbfuentes/tutorial-pygame>`_ ).

En el próximo tutorial explicaremos como controlar el movimiento de una
imagen (con el teclado), colisiones y reproducir algunos sonidos.
`Pueden encontrar esta tercera parte
aquí. <https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/>`_

Saludos.

.. [1] Toy story: `Buzz Lightyear <https://www.youtube.com/watch?v=ku2Eqft8fiY>`_ .

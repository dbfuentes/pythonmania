+++
title = "Tutorial pygame 1: introducción a la programacion de videojuegos"
slug = "tutorial-pygame-introduccion"
date = "2010-03-23T21:47:00-04:00"
categories = ["tutoriales"]
tags = ["python2", "juegos", "pygame"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Hace tiempo que quería escribir una entrada sobre el uso de pygame
para programar juegos. Así que ahora voy a hacerlo en una serie de
entradas sobre este tema (aun no estoy seguro de cuantas serán), por
lo que esta servirá de introducción e índice (o sea la Parte 1 de este
tutorial).

<!--more-->

Primero, ¿que es pygame?
========================

Lo primero que hay que `pygame <http://www.pygame.org/>`_ es una
biblioteca multimedia (que trabaja sobre las librerías SDL) que permiten
la creación de videojuegos en dos dimensiones de una manera sencilla.

    Las SDL (Simple DirectMedia Layer) son un conjunto de bibliotecas
    que proporcionan funciones básicas para realizar operaciones de
    dibujado 2D, gestión de efectos de sonido, música, carga y gestión
    de imágenes (y son importantes ya que pygame trabaja sobre ellas).
    Básicamente es una librería similar a OpenGL o DirectX (solo en la
    parte 2D), diseñada para ser más fácil de usar que estas otras
    (además OpenGL solo se encarga de la parte gráfica, en cambio SDL
    incluye otras cosas como son los sonidos ;-))

Tal como les dije anteriormente usando pygame vamos a programar algunos
videojuegos. No esperen programar un juego inmenso en 3D con gráficas
espectaculares y que necesite equipos potentes para ser usado. Mas bien
lo que vamos a obtener son juegos en 2D similares a los de consolas como
supernintendo, portátiles como el GBA, nintendoDS (sin la parte táctil),
etc. Para que se hagan una idea de como son los juegos programados con
pygame, algunos juegos como `frets on
fire <http://fretsonfire.sourceforge.net/>`_,
`SolarWolf <http://megadriver.wordpress.com/2010/01/18/lo-mejor-de-pygame-solarwolf/>`_
o `pydance <http://icculus.org/pyddr/>`_ usan pygame para su
desarrollo. Pueden encontrar mas ejemplos en `el sitio de
pygame <http://pygame.org/tags/>`_ .

Volviendo al tema Pygame se encarga de manejar la parte más complicada
dentro de la programación de un videojuego, o se se encarga de cargar y
mostrar las imágenes (en formatos como PNG, BMP, PCX, TGA,...), sonido
(formatos OGG, MP3, MIDI,...), vídeos, las ventanas del juego y
monitoriza los dispositivos de entrada (como mouse, teclado y joystick)
de una manera bastante sencilla. Por lo que básicamente uno se tiene que
preocupar por la programación del juego en si.

Requisitos previos (recomendados)
---------------------------------

-  Tener instalado python y pygame (obvio). Ambas están disponibles para
   varios sistemas (como Windows, GNU/Linux, Mac, etc.) así que los
   juegos creados pueden ser multiplataforma
-  Conocimiento básico de python, por lo menos como se definen funciones
   y
   `clases <http://mundogeek.net/archivos/2008/03/05/python-orientacion-a-objetos/>`__
-  Conocimientos básicos de matemáticas y física, lo que resulta útil al
   programar juegos en general (con lo que aprendiste de niño es mas que
   suficiente). Básicamente es algo de física clásica como los conceptos
   de velocidad, aceleración, etc. y de matemáticas como los conceptos
   de puntos, coordenadas en el espacio, etc.

Primeros pasos
--------------

Lo primero hay que hay que hacer es importar el modulo de pygame, lo
cual se hace simplemente como:

.. code-block:: python

    import pygame
    from pygame.locals import *

La segunda linea es opcional (pero muy recomendada), ya se utiliza para
importar una serie de constantes que tiene pygame (como las teclas del
teclado)

Ahora en general nuestro código en pygame tendrá una estructura como esta:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Importacion de los módulos
    import pygame
    from pygame.locals import *
    # y cualquier otro modulo usado

    # ----------------------------------------------
    # Constantes, como anchos y largo de pantalla, etc.
    # ----------------------------------------------

    # ----------------------------------------------
    # Clases y Funciones utilizadas (lo explicare en la siguiente parte)
    # ----------------------------------------------

    def main():
        pygame.init()
        # La clase o función principal que crea o ejecuta el juego
        # Contiene principalmente loop del juego (el alma de este)

    if __name__ == "__main__":
        main()

Si te fijas en la linea 18 se hace un *pygame.init()*, esto es para que
se inicie pygame y es necesario ejecutarlo antes de empezar a usar
pygame (por eso fue incluido al inicio de la función principal del
juego). Si no te gusta incluir el *pygame.init()* en la función del
juego, otro buen lugar es incluirlo inmediatamente después del *if
\_\_name\_\_ == "\_\_main\_\_":* (antes de llamar a la función *main()*)

¿Y ahora que?
-------------

Continua leyendo las siguientes entradas sobre el tema, en donde se
explica como crear ventanas, personajes, insertar sonidos, etc. Que son
las siguientes:

`Parte 2: Creando una ventana, cargando imágenes y moviéndolas al
interior de la
ventana <https://www.pythonmania.net/es/2010/03/25/tutorial-pygame-2-ventana-e-imagenes/>`_:
En esta segunda parte veremos:

-  Como crear una ventana.
-  el bucle principal de un videojuego (o como saber cuando hay que
   terminar el juego).
-  Usar una imagen como fondo.
-  mostrar una imagen y moverla en la pantalla.

`Parte 3: Creando un videojuego (el clasico
pong) <https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/>`_:
En esta parte veremos:

-  Crear una función para cargar imágenes.
-  Como crear sprites (los sprites son personajes, objetos, etc. dentro
   del juego).
-  sincronización en los videojuegos (¿que son frames por segundo?).
-  controlar un sprite con el teclado.
-  controlar un sprite con el mouse.
-  colisiones entre elementos (sprites).
-  Inteligencia artificial (o la falta de esta).
-  reproducción de sonidos (al cumplirse alguna condición).

`Parte 4: manejando
texto <https://www.pythonmania.net/es/2010/07/14/tutorial-pygame-4-figuras-y-texto/>`_:
El nombre lo dice todo, una explicación de como mostrar texto y figuras en
la pantalla. En esta parte veremos:

- Rellenar el fondo con un color especifico.
- Dibujar lineas y circulos en la pantalla.
- Sincronizar el movimiento, usando el tiempo.


    Referencias:

    Principalmente la documentación de pygame, que `esta traducida en
    loserjuegos <http://www.losersjuegos.com.ar/traducciones/pygame>`__
    o si quieren `una copia de esta traducción en un pdf (para consultar
    off-line) <http://pythonmania.files.wordpress.com/2010/03/pygame_esp_20090205.pdf>`_

    Si alguien quiere el código de los ejemplos, este esta en github:
    http://github.com/dbfuentes/tutorial-pygame

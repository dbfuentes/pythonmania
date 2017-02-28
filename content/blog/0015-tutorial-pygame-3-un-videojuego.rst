+++
title = "Tutorial Pygame 3: creando un videojuego (el clasico pong)"
slug = "tutorial-pygame-3-un-videojuego"
date = "2010-04-07T15:48:00-04:00"
lastmod = "2016-08-15T20:29:05-04:00"
categories = ["tutoriales"]
tags = ["python2", "juegos", "pygame"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Ok, es tiempo de continuar con `esta serie de tutoriales de pygame
<https://www.pythonmania.net/es/2010/03/23/tutorial-pygame-introduccion/>`_.
Si recuerdan en el `tutorial anterior
<https://www.pythonmania.net/es/2010/03/25/tutorial-pygame-2-ventana-e-imagenes/>`_
cargamos unas imágenes y las mostramos en pantalla, haciendo que se
movieran. Ahora vamos a avanza un poco, programando uno de los juegos
mas sencillos que pueden existir, el `clásico
pong <http://es.wikipedia.org/wiki/Pong>`_.

<!--more-->

En esta parte trataremos los siguientes puntos:

-  Crear una función para cargar imágenes
-  Como crear sprites (los sprites son personajes, objetos, etc. dentro
   del juego)
-  Sincronización de elementos en los videojuegos (¿que son frames por
   segundo?)
-  Controlar un sprite con el teclado
-  Controlar un sprite con el mouse
-  Colisiones entre elementos (sprites)
-  Inteligencia artificial (o la falta de esta)
-  reproducción de sonidos (al cumplirse alguna condición)

Para este tutorial me voy a basar en el primer juego que escribí en
pygame `hace mucho
tiempo <http://logicerror.wordpress.com/2008/03/01/pygame/>`_ (que
por cierto tenia algunos errores), el cual a su vez se basaba en un
ejemplo de `linuxjuegos
<http://www.linuxjuegos.com/programacion-de-juegos-en-python-tutorial-2/>`_
(aun más antiguo). En resumen voy reescribir el juego, simplificándolo y de
paso voy a explicar como funciona:

Paso 1: Creando lo básico (y una función para cargar imágenes)
==============================================================

Lo primero es crear la ventana y una función para cargar las imágenes.

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import os
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480
    IMG_DIR = "imagenes"

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    def load_image(nombre, dir_imagen, alpha=False):
        # Encontramos la ruta completa de la imagen
        ruta = os.path.join(dir_imagen, nombre)
        try:
            image = pygame.image.load(ruta)
        except:
            print("Error, no se puede cargar la imagen: " + ruta)
            sys.exit(1)
        # Comprobar si la imagen tiene "canal alpha" (como los png)
        if alpha is True:
            image = image.convert_alpha()
        else:
            image = image.convert()
        return image


    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("Ejemplo de un Pong Simple")

        # cargamos el fondo
        fondo = load_image("fondo.jpg", IMG_DIR, alpha=False)

        # el bucle principal del juego
        while True:
            # actualizamos la pantalla
            screen.blit(fondo, (0, 0))
            pygame.display.flip()

            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()


    if __name__ == "__main__":
        main()

Creamos una función para cargar imágenes por comodidad, así se facilita
cargar distintos tipos de imágenes a la vez que realiza los convert()
según corresponda. La función trabaja de la siguiente manera: toma tres
parámetros el nombre, el directorio donde se encuentra la imagen y si
tiene o no un canal alpha (el color transparente del los png). Luego con
esa información (directorio + nombre imagen) crea la ruta de la imagen y
intenta abrirla con un *try* que es como "espera, voy a probar abrir la
imagen… y si algo falla (*except*), avisa y termina el programa".
Después de eso revisa si la imagen tiene un cana alpha (*if alpha ==
True*), en ese caso realiza el *.convert\_alpha()*, en caso contrario
realiza un simple *.convert()* . Finalmente devuelve la superficie
("surface") que se crea al cargar la imagen.

Luego en la función principal del juego se carga la imagen del fondo y
se muestra en pantalla. Si se fijan hay una variable llamada IMG\_DIR,
esta indica el nombre del directorio donde se encuentran las imágenes.

Nota: usaremos estas imágenes en el tutorial:

.. image:: https://pythonmania.files.wordpress.com/2010/04/bola.png
   :width: 23px
   :height: 23px
   :target: https://pythonmania.files.wordpress.com/2010/04/bola.png
   :alt: bola

.. image:: https://pythonmania.files.wordpress.com/2010/04/paleta.png
   :width: 16px
   :height: 75px
   :target: https://pythonmania.files.wordpress.com/2010/04/paleta.png
   :alt: paleta

.. image:: http://pythonmania.files.wordpress.com/2010/04/fondo_tutorial3.jpg?w=300
   :width: 300px
   :height: 225px
   :target: https://pythonmania.files.wordpress.com/2010/04/fondo_tutorial3.jpg
   :alt: fondo.jpg (hacer click para agrandar)

Paso 2: Creando sprites
=======================

En el tutorial anterior ya hable algo de los sprites y les recomende
`leer esto (que explica los conceptos básicos de los videojuegos)
<http://www.losersjuegos.com.ar/referencia/articulos/conceptos_basicos>`_.
Ahora vamos a profundizar en los sprites.

Un sprite es básicamente cualquier cosa que aparezca en nuestro juego
(personajes, objetos, etc.) la cual tiene asociada informacion (como su
posición, tamaño, etc.). En pygame es costumbre hacer una clase para
cada sprite, de tal manera que la clase contenga la información
relevante de ese sprite.

En este caso vamos a crear la clase Pelota, que va a contener todos los
datos de la pelota usada en el juego. Esta primera versión de la pelota,
solo rebotará indistintamente en las cuatro murallas de nuestra
pantalla. Veamos el código:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import os
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480
    IMG_DIR = "imagenes"

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    def load_image(nombre, dir_imagen, alpha=False):
        # Encontramos la ruta completa de la imagen
        ruta = os.path.join(dir_imagen, nombre)
        try:
            image = pygame.image.load(ruta)
        except:
            print("Error, no se puede cargar la imagen: " + ruta)
            sys.exit(1)
        # Comprobar si la imagen tiene "canal alpha" (como los png)
        if alpha is True:
            image = image.convert_alpha()
        else:
            image = image.convert()
        return image


    # -----------------------------------------------
    # Creamos los sprites (clases) de los objetos del juego:


    class Pelota(pygame.sprite.Sprite):
        "La bola y su comportamiento en la pantalla"

        def __init__(self):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("bola.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = SCREEN_WIDTH / 2
            self.rect.centery = SCREEN_HEIGHT / 2
            self.speed = [3, 3]


    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("Ejemplo de un Pong Simple")

        # cargamos los objetos
        fondo = load_image("fondo.jpg", IMG_DIR, alpha=False)
        bola = Pelota()

        # el bucle principal del juego
        while True:
            # actualizamos la pantalla
            screen.blit(fondo, (0, 0))
            screen.blit(bola.image, bola.rect)
            pygame.display.flip()

            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()


    if __name__ == "__main__":
        main()

Ahora dentro de la clase *Pelota()* vemos que hereda los metodos de la
clase padre pygame.sprite.Sprite, luego en el def \_\_init\_\_(self): se
define que se hará cuando se llama al instanciar la clase. (`una explicación
mas completa por
aquí <http://mundogeek.net/archivos/2008/03/05/python-orientacion-a-objetos/>`_).
En nuestro caso se le dice primero que inicie *pygame.sprite.Sprite*
(algo un poco raro pero obligatorio), luego en *self.image* cargamos la
imagen (algo conocido, ya que lo hemos hecho antes).

    Nota: *self* se usa para referirse la clase actual en la que se
    está, en otras palabras para una clase es como referirse a si misma
    (algo común en las clases).

Con *self.image.get\_rect()* se obtiene un objeto rectangulo (*rect*)
con las dimensiones y posición de la imagen (en este caso se usa sobre
self.image, o sea la imagen de la bola) y este rectángulo se lo
asignamos a self.rect. Esto resulta bastante útil, ya que nos permite
dejar de preocuparnos por las dimensiones de la imagen por si luego
decidimos cambiarlas, además de obtener algunos extras que se señalan a
continuación:

    Ahora el `rectangulo creado con
    get\_rect() <http://www.losersjuegos.com.ar/traducciones/pygame/rect>`_
    nos permite usar varios varios atributos virtuales que se pueden
    usar para mover o alinear el rectángulo (y que usaremos a
    continuación). Entre ellos:

    -  top, left, bottom, right
    -  topleft, bottomleft, topright, bottomright
    -  midtop, midleft, midbottom, midright
    -  center, centerx, centery
    -  size, width, height
    -  w,h

Ahora con *self.rect.centerx* y *self.rect.centery* le indicamos a
pygame que el centro de nuestra imagen (centerx, centery) quede
exactamente en la mitad de la pantalla (o sea centramos la imagen).

Por ultimo en *self.speed* se establece la velocidad con que se mueva la
pelota en el eje x y en el eje y (en la siguiente parte se vera para que
lo vamos a usar).

Intermedio: un poco de teoría, sincronización en los videojuegos
----------------------------------------------------------------

Antes de continuar, hay que mencionar algo sobre la forma de sincronizar
los videojuegos, lo cual es necesario, ya que si llegamos y ejecutamos
nuestro videojuego en un computador antiguo, por ejemplo en un Pentium
II, es posible que el videojuego se vea muy lento, en cambio si lo
ejecutamos en un computador más potente (de ultima generación), el juego
se verá tan rápido que será imposible jugar.

Hay dos formas de sincronización:

**Sincronización por Framerate o Frames per Second (FPS):** En este
caso se refiere a la frecuencia con que se ejecuta el ciclo principal de
un videojuego en un segundo (mientras más alto, más fluidez).

Básicamente se va obteniendo el tiempo que ha trascurrido desde el
inicio del ciclo, se hacen las acciones del juego y cuando pasen los FPS
especificados, se actualiza/refresca la pantalla. Así se logra una
fluidez constante sin importar en que equipo se ejecute.

    Este es uno de los metodos mas extendidos (especialmente en juegos
    en 2D). Claro que este método tiene sus ventajas y desventajas:

    -  A favor: ya que limitamos la cantidad máxima de FPS que puede
       lograr el juego, este debería verse de la misma forma en
       cualquier computador en donde se corra, ya que si el equipo es
       muy potente solo funcionara a los FPS especificados (pese a que
       puede ir mas rápido).

    -  En contra: al usar este método en computadores más rápidos (que
       el pc donde se desarrollo) el juego se verá fluido, pero si lo
       ejecutamos en una máquina con un procesador mucho más antiguo
       del que usamos para desarrollarlo, lo más probable es que se vea
       bastante lento (por algo hay requerimientos mínimos).

**Sincronización por Tiempo:** En este caso se sincroniza en base al
tiempo (por lo que no importan los FPS) moviéndose de igual manera los
objetos sin importar en que equipo se ejecute el juego (ya que el
movimiento depende del tiempo transcurrido).Ya que lo que se hace es
calcular la posición de un objeto en función del tiempo transcurrido.

    Este método se usa bastante en videojuegos 3D, ya que el framerate
    varía mucho en cada ciclo.

    -  A favor: Los objetos/sprites se mueven siempre a la misma
       velocidad, sin importar cuantos FPS se alcancen (ya que su
       movimiento es en función del tiempo), por lo cual no hay que
       preocuparse de controlar el framerate.

    -  En contra: Pese a que los objetos se mueven siempre a la misma
       velocidad, en un computador más lento el desplazamiento no se
       verá fluidamente, por ejemplo en caso (extremo) de que se demore
       el juego 1 segundo en cada ciclo, cada vez que se deba mover un
       objeto este se desplazara grandes distancias (ya que el tiempo
       entre actualizaciones/ciclos en donde se refrezca la pantalla es
       grande), produciéndose un salto muy notorio.

Si en el primer método (FPS) queríamos mover un objeto 8 pixeles,
haríamos lo siguiente:

.. code-block:: python

    x = x + 8


En cambio si lo hacemos en base al tiempo tendríamos:

.. code-block:: python

    x = x + (velocidad) * (tiempo)

O sea física básica, en donde por ejemplo si se desplaza el objeto a una
velocidad de 0.008, y el ciclo demora 1 segundo en ejecutarse (1000ms),
el nuevo incremento será de:

.. code-block:: python

    x = x + 0.008 * 1000
    x = x + 8

Ok, volvamos con nuestro juego.

Parte 3: Moviendo la pelota (y creando un reloj)
================================================

Volviendo al juego (el cual va a usar el método de los FPS), vamos a
crear dentro de la clase Pelota una función update, que se encargará de
hacer avanzar la bola y que seta rebote cuando haya llegado a los
límites de la pantalla.

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import os
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480
    IMG_DIR = "imagenes"

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    def load_image(nombre, dir_imagen, alpha=False):
        # Encontramos la ruta completa de la imagen
        ruta = os.path.join(dir_imagen, nombre)
        try:
            image = pygame.image.load(ruta)
        except:
            print("Error, no se puede cargar la imagen: " + ruta)
            sys.exit(1)
        # Comprobar si la imagen tiene "canal alpha" (como los png)
        if alpha is True:
            image = image.convert_alpha()
        else:
            image = image.convert()
        return image


    # -----------------------------------------------
    # Creamos los sprites (clases) de los objetos del juego:


    class Pelota(pygame.sprite.Sprite):
        "La bola y su comportamiento en la pantalla"

        def __init__(self):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("bola.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = SCREEN_WIDTH / 2
            self.rect.centery = SCREEN_HEIGHT / 2
            self.speed = [3, 3]

        def update(self):
            if self.rect.left < 0 or self.rect.right > SCREEN_WIDTH:
                self.speed[0] = -self.speed[0]
            if self.rect.top < 0 or self.rect.bottom > SCREEN_HEIGHT:
                self.speed[1] = -self.speed[1]
            self.rect.move_ip((self.speed[0], self.speed[1]))

    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("Ejemplo de un Pong Simple")

        # cargamos los objetos
        fondo = load_image("fondo.jpg", IMG_DIR, alpha=False)
        bola = Pelota()

        clock = pygame.time.Clock()

        # el bucle principal del juego
        while True:
            clock.tick(60)
            bola.update()

            # actualizamos la pantalla
            screen.blit(fondo, (0, 0))
            screen.blit(bola.image, bola.rect)
            pygame.display.flip()

            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()


    if __name__ == "__main__":
        main()

Los dos *if* sirven para comprobar si la pelota alcanzo los bordes de la
pantalla, si esto ocurre que comience a moverse en sentido contrario
(por ejemplo si llego a topar el borde derecho, la pelota comenzara a
moverse a la izquierda, lo mismo para los otros bordes)

La función *move\_ip(x,y)* mueve de forma relativa el sprite por pantalla,
esto es, subirá o bajará *x* pixel y avanzará retrocederá *y* pixel (en
este caso utilizara la velocidad que definimos anteriormente para la
bola, moviendola 3pixeles hacia la derecha y abajo).

Ahora en la función principal del juego tenemos una linea que inicia la
clase *bola = Pelota()* y luego de esto una linea que crea un reloj que
controle el tiempo del juego *clock = pygame.time.Clock()*, el cual se
ejecuta justo antes de iniciar el bucle principal del juego.

Luego se esto en la función principal hacemos *clock.tick(60)*, lo cual
sirve para poner el reloj a un paso de 60 FPS, esto se hace para que
nunca se pase de 60 frames por segundo, así no importará si estamos
ejecutando esto en un pentium II o en una supercomputadora, la velocidad
siempre será como máximo de 60 frames por segundo.

Finalmente con *bola.update()* se actualiza la posición de la pelota y
luego se redibuja la pantalla.

    Clock.tick es bastante curioso, si se usa sin argumentos (o sea
    *clock.tick()*) devuelve el tiempo que ha pasado (en milisegundos)
    desde la ultima vez que se llamo (o sea funciona como un reloj),
    pero si se usa con un argumento, que es el framerate (por ejemplo
    *clock.tick(60)*), la función esperará el tiempo necesario para
    mantener al juego corriendo a la velocidad solicitada, o sea en el
    ejemplo el juego nunca correrá a más de 60 frames/cuadros por
    segundo (sirve para controlar el framerate).

    Eso si este método no es muy exacto, pero casi no usa CPU, si se
    quiere algo mas exacto hay que usar Clock.tick\_busy\_loop `(más
    información en la documentación de
    pygame) <http://www.losersjuegos.com.ar/traducciones/pygame/time/clock>`_

Parte 4: Creando la paleta (control con teclado)
================================================

No hay grandes cambios, lo primero que se hace es crear una clase que
contenga la paleta y en la función principal hay que revisar si se ha
pulsado alguna de las teclas indicadas (y actuar de acuerdo a ella). El
código queda:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import os
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480
    IMG_DIR = "imagenes"

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    def load_image(nombre, dir_imagen, alpha=False):
        # Encontramos la ruta completa de la imagen
        ruta = os.path.join(dir_imagen, nombre)
        try:
            image = pygame.image.load(ruta)
        except:
            print("Error, no se puede cargar la imagen: " + ruta)
            sys.exit(1)
        # Comprobar si la imagen tiene "canal alpha" (como los png)
        if alpha is True:
            image = image.convert_alpha()
        else:
            image = image.convert()
        return image


    # -----------------------------------------------
    # Creamos los sprites (clases) de los objetos del juego:


    class Pelota(pygame.sprite.Sprite):
        "La bola y su comportamiento en la pantalla"

        def __init__(self):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("bola.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = SCREEN_WIDTH / 2
            self.rect.centery = SCREEN_HEIGHT / 2
            self.speed = [3, 3]

        def update(self):
            if self.rect.left < 0 or self.rect.right > SCREEN_WIDTH:
                self.speed[0] = -self.speed[0]
            if self.rect.top < 0 or self.rect.bottom > SCREEN_HEIGHT:
                self.speed[1] = -self.speed[1]
            self.rect.move_ip((self.speed[0], self.speed[1]))


    class Paleta(pygame.sprite.Sprite):
        "Define el comportamiento de las paletas de ambos jugadores"

        def __init__(self, x):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("paleta.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = x
            self.rect.centery = SCREEN_HEIGHT / 2

        def humano(self):
            # Controlar que la paleta no salga de la pantalla
            if self.rect.bottom >= SCREEN_HEIGHT:
                self.rect.bottom = SCREEN_HEIGHT
            elif self.rect.top <= 0:
                self.rect.top = 0


    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("Ejemplo de un Pong Simple")

        # cargamos los objetos
        fondo = load_image("fondo.jpg", IMG_DIR, alpha=False)
        bola = Pelota()
        jugador1 = Paleta(40)

        clock = pygame.time.Clock()

        # el bucle principal del juego
        while True:
            clock.tick(60)

            # Actualizamos los obejos en pantalla
            jugador1.humano()
            bola.update()

            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit(0)
                elif event.type == pygame.KEYDOWN:
                    if event.key == K_UP:
                        jugador1.rect.centery -= 5
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 5
                    elif event.key == K_ESCAPE:
                        sys.exit(0)
                elif event.type == pygame.KEYUP:
                    if event.key == K_UP:
                        jugador1.rect.centery += 0
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 0

            #actualizamos la pantalla
            screen.blit(fondo, (0, 0))
            screen.blit(bola.image, bola.rect)
            screen.blit(jugador1.image, jugador1.rect)
            pygame.display.flip()


    if __name__ == "__main__":
        main()

Se define la clase paleta de manera analoga a la clase pelota, la única
diferencia es que el def \_\_init\_\_(self, x) recibe como argumento
adicional la coordenada x de la paleta (así con la misma clase se puede
cargar una paleta a la izquierda y otra a la derecha), en cuanto a la
coordenada y (en eje vertical), esta clase deja centrada la paleta
(verticalmente).

Después dentro de la clase se define un: *def humano(self)*, que
simplemente es lo que hace la paleta cuando es controlada por un jugador
(humano), en este caso lo único que hace es que si alcanza el borde
superior o inferior en el eje y (eje vertical), deje de moverse, para
que no se salga de pantalla (algo no muy grato).

En la función principal del juego, se inicia la clase *jugador1 =
Paleta(25)* para crear la paleta controlada por el jugador (el 25 es
para que la paleta quede a 25 píxeles del borde izquierdo) y un poco más
adelante se le indica que esa paleta se controlada por una persona
(humana) *jugador1.humano()*, el resto es actualizar la pantalla.

Dentro del bucle se comprueba los eventos (en la parte de *for event in
pygame.event.get():*), así cada vez que se pulsa una tecla (del teclado)
se obtiene un evento pygame.KEYDOWN y cada vez que se suelta una tecla
se produce un evento pygame.KEYUP. Esos eventos devuelven el valor de la
tecla pulsada (`se puede revisar los posibles valores en la
documentación <http://www.losersjuegos.com.ar/traducciones/pygame/key>`_)

Así que simplemente se revisa dentro del evento pygame.KEYDOWN que tecla
se pulso, si se trata de la tecla flecha hacia arriba (K\_UP) la paleta
se desplaza 5 píxeles hacia arriba y si se trata de la tecla flecha
hacia abajo (K\_DOWN) se desplaza 5 píxeles hacia abajo. En el caso de
que puse la tecla K\_ESCAPE simplemente se termina el programa

Una revisión similar se hace cuando se sueltan las teclas (pygame.KEYUP)
en donde se deja de mover la paleta. En este juego no tiene mucho
sentido hacer esto (ya que por defecto la paleta esta quieta) pero en
otros juegos puede servir, por eso lo puse (pese a no ser necesario).

    Dentro de *pygame.key* existe el método `get\_pressed()
    <http://www.losersjuegos.com.ar/traducciones/pygame/key#get_pressed>`_
    que devuelve una lista con todas las teclas pulsadas, usar esto
    puede ser mas simple que ir comprobando una por una las teclas pero
    tiene como inconveniente que:

    -  no hay forma de conocer el orden de las teclas pulsadas
    -  las pulsaciones muy rápidas de teclas pueden pasar desapercibidas

Parte 5: mejorando el control con el teclado (y el mouse)
=========================================================

Si se fijaron el código anterior tiene un problema, al mantener pulsada
una tecla (flecha hacia arriba o abajo) el programa solo mueve la paleta
una vez (solo registra una vez el evento), en vez de mantenerse moviendo
la paleta hasta que se suelte la tecla. Además falta poder controlar la
paleta con el mouse.

Para lo primero, hay que indicarle a pygame que active la repetición de
teclas, lo cual se hace con la función `pygame.key.set\_repeat()
<http://www.losersjuegos.com.ar/traducciones/pygame/key#set_repeat>`_,
esta toma dos argumentos: el primero establece el tiempo de retraso (el
número de milisegundos) que tienen que pasar para detectar/enviar el
primer evento y el segundo argumento (el intervalo) es el numero de
milisegundos que pasan entre cada envío del evento.

Para conocer el estado del mouse, hay que usar
`pygame.mouse <http://www.losersjuegos.com.ar/traducciones/pygame/mouse>`_,
para ello lo primero que haremos es que el cursor del mouse no se vea
dentro de la pantalla (o sea que solo aparezca fuera de ella) seo lo
hacemos con *pygame.mouse.set\_visible(False)*

Luego dentro de nuestro bucle, registramos la posición del mouse con
*pos\_mouse = pygame.mouse.get\_pos()*, que nos devuelve una tupla con
`las coordenadas (x, y) en donde esta el puntero del
mouse <http://www.losersjuegos.com.ar/traducciones/pygame/mouse#get_pos>`_,
las que quedan almacenadas en pos\_mouse. Luego usamos *mov\_mouse =
pygame.mouse.get\_rel()* para obtener cuanto se ha movido el mouse desde
la ultima consulta que realizo get\_rel() (`esto tambien es una tupla,
del tipo (x,y) que devueve la distancia que se ha
movido <http://www.losersjuegos.com.ar/traducciones/pygame/mouse#get_rel>`_),
en caso de que el puntero del mouse no se haya movido devlvera un (0,0).

.. code-block:: python

    elif mov_mouse[1] != 0:
       jugador1.rect.centery = pos_mouse[1]

Avanzamos un poco y en la parte en donde comprobamos los eventos, usamos
un *if mov\_mouse[1] != 0:*, con esto el deciomos que si la coordenada y
de la tupla que devuelve get\_rel() es distinta de 0, mueva la paleta a
la posición en donde esta el mouse (*jugador1.rect.centery =
pos\_mouse[1]*).

Entonces nuestro codigo queda como:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import os
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480
    IMG_DIR = "imagenes"

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    def load_image(nombre, dir_imagen, alpha=False):
        # Encontramos la ruta completa de la imagen
        ruta = os.path.join(dir_imagen, nombre)
        try:
            image = pygame.image.load(ruta)
        except:
            print("Error, no se puede cargar la imagen: " + ruta)
            sys.exit(1)
        # Comprobar si la imagen tiene "canal alpha" (como los png)
        if alpha is True:
            image = image.convert_alpha()
        else:
            image = image.convert()
        return image


    # -----------------------------------------------
    # Creamos los sprites (clases) de los objetos del juego:


    class Pelota(pygame.sprite.Sprite):
        "La bola y su comportamiento en la pantalla"

        def __init__(self):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("bola.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = SCREEN_WIDTH / 2
            self.rect.centery = SCREEN_HEIGHT / 2
            self.speed = [3, 3]

        def update(self):
            if self.rect.left < 0 or self.rect.right > SCREEN_WIDTH:
                self.speed[0] = -self.speed[0]
            if self.rect.top < 0 or self.rect.bottom > SCREEN_HEIGHT:
                self.speed[1] = -self.speed[1]
            self.rect.move_ip((self.speed[0], self.speed[1]))


    class Paleta(pygame.sprite.Sprite):
        "Define el comportamiento de las paletas de ambos jugadores"

        def __init__(self, x):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("paleta.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = x
            self.rect.centery = SCREEN_HEIGHT / 2

        def humano(self):
            # Controlar que la paleta no salga de la pantalla
            if self.rect.bottom >= SCREEN_HEIGHT:
                self.rect.bottom = SCREEN_HEIGHT
            elif self.rect.top <= 0:
                self.rect.top = 0


    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("Ejemplo de un Pong Simple")

        # cargamos los objetos
        fondo = load_image("fondo.jpg", IMG_DIR, alpha=False)
        bola = Pelota()
        jugador1 = Paleta(40)

        clock = pygame.time.Clock()
        pygame.key.set_repeat(1, 25)  # Activa repeticion de teclas
        pygame.mouse.set_visible(False)

        # el bucle principal del juego
        while True:
            clock.tick(60)
            # Obtenemos la posicon del mouse
            pos_mouse = pygame.mouse.get_pos()
            mov_mouse = pygame.mouse.get_rel()

            # Actualizamos los obejos en pantalla
            jugador1.humano()
            bola.update()

            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit(0)
                elif event.type == pygame.KEYDOWN:
                    if event.key == K_UP:
                        jugador1.rect.centery -= 5
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 5
                    elif event.key == K_ESCAPE:
                        sys.exit(0)
                elif event.type == pygame.KEYUP:
                    if event.key == K_UP:
                        jugador1.rect.centery += 0
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 0
                # Si el mouse no esta quieto mover la paleta a su posicion
                elif mov_mouse[1] != 0:
                    jugador1.rect.centery = pos_mouse[1]

            # actualizamos la pantalla
            screen.blit(fondo, (0, 0))
            screen.blit(bola.image, bola.rect)
            screen.blit(jugador1.image, jugador1.rect)
            pygame.display.flip()


    if __name__ == "__main__":
        main()

Y se ve asi:

{{< youtube sO8yskwseqQ >}}

Parte 6: Colisiones
===================

Hasta ahora al encontrarse la pelota con la paleta, simplemente esta
pasa por detras sin rebotar en la paleta, eso occure porque no hemos
especificado que tiene que pasar cuando se producen colisiones, o sea
cuando dos sprites chocan.

Para esto dentro de la clase de uno de los sprites hay que crear un
método que compruebe si este sprite ha chocado con otros, por
conveniencia lo vamos a colocar en la clase Pelota (ya que esta es la
que rebota en las otras cosas).

En la clase Pelota definimos *def colision(self, objetivo):* el cual
comprueba si la pelota ha chocado con algo (el segundo argumento es el
objeto con el cual esperamos que choque).

Para saber si dos sprites/objetos han chocado usamos
*pygame.sprite.colliderect(objeto1, objeto2)* este comprueba si el
rectángulo del objeto1 entra en contacto con el rectángulo del objeto2,
retornando True en caso de que entren en contacto. Luego simplemente
cambiamos la dirección de la pelota.

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import os
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480
    IMG_DIR = "imagenes"

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    def load_image(nombre, dir_imagen, alpha=False):
        # Encontramos la ruta completa de la imagen
        ruta = os.path.join(dir_imagen, nombre)
        try:
            image = pygame.image.load(ruta)
        except:
            print("Error, no se puede cargar la imagen: " + ruta)
            sys.exit(1)
        # Comprobar si la imagen tiene "canal alpha" (como los png)
        if alpha is True:
            image = image.convert_alpha()
        else:
            image = image.convert()
        return image


    # -----------------------------------------------
    # Creamos los sprites (clases) de los objetos del juego:


    class Pelota(pygame.sprite.Sprite):
        "La bola y su comportamiento en la pantalla"

        def __init__(self):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("bola.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = SCREEN_WIDTH / 2
            self.rect.centery = SCREEN_HEIGHT / 2
            self.speed = [3, 3]

        def update(self):
            if self.rect.left < 0 or self.rect.right > SCREEN_WIDTH:
                self.speed[0] = -self.speed[0]
            if self.rect.top < 0 or self.rect.bottom > SCREEN_HEIGHT:
                self.speed[1] = -self.speed[1]
            self.rect.move_ip((self.speed[0], self.speed[1]))

        def colision(self, objetivo):
            if self.rect.colliderect(objetivo.rect):
                self.speed[0] = -self.speed[0]


    class Paleta(pygame.sprite.Sprite):
        "Define el comportamiento de las paletas de ambos jugadores"

        def __init__(self, x):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("paleta.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = x
            self.rect.centery = SCREEN_HEIGHT / 2

        def humano(self):
            # Controlar que la paleta no salga de la pantalla
            if self.rect.bottom >= SCREEN_HEIGHT:
                self.rect.bottom = SCREEN_HEIGHT
            elif self.rect.top <= 0:
                self.rect.top = 0


    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("Ejemplo de un Pong Simple")

        # cargamos los objetos
        fondo = load_image("fondo.jpg", IMG_DIR, alpha=False)
        bola = Pelota()
        jugador1 = Paleta(40)

        clock = pygame.time.Clock()
        pygame.key.set_repeat(1, 25)  # Activa repeticion de teclas
        pygame.mouse.set_visible(False)

        # el bucle principal del juego
        while True:
            clock.tick(60)
            # Obtenemos la posicon del mouse
            pos_mouse = pygame.mouse.get_pos()
            mov_mouse = pygame.mouse.get_rel()

            # Actualizamos los obejos en pantalla
            jugador1.humano()
            bola.update()

            # Comprobamos si colisionan los objetos
            bola.colision(jugador1)

            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit(0)
                elif event.type == pygame.KEYDOWN:
                    if event.key == K_UP:
                        jugador1.rect.centery -= 5
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 5
                    elif event.key == K_ESCAPE:
                        sys.exit(0)
                elif event.type == pygame.KEYUP:
                    if event.key == K_UP:
                        jugador1.rect.centery += 0
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 0
                # Si el mouse no esta quieto mover la paleta a su posicion
                elif mov_mouse[1] != 0:
                    jugador1.rect.centery = pos_mouse[1]

            # actualizamos la pantalla
            screen.blit(fondo, (0, 0))
            screen.blit(bola.image, bola.rect)
            screen.blit(jugador1.image, jugador1.rect)
            pygame.display.flip()


    if __name__ == "__main__":
        main()

Parte 7: Comportamiento del oponente (crear al enemigo)
=======================================================

Ahora vamos a crear al enemigo, para esto vamos a modificar la clase
Paleta, agregando el método cpu (que sera el segundo jugador controlado
por el computador).

.. code-block:: python

    def cpu(self, objetivo):
        self.rect.centery = objetivo.rect.centery
        if self.rect.bottom >= SCREEN_HEIGHT:
            self.rect.bottom = SCREEN_HEIGHT
        elif self.rect.top <= 0:
            self.rect.top = 0

Básicamente definimos dentro de la clase Paleta un **def cpu(self,
objetivo):** en donde se le dice a la paleta que siga la posición de la
pelota (o sea que se mueva junto con ella) haciendo un
*self.rect.centery = objetivo.rect.centery* (o sea la posición vertical
de la paleta es igual a la posición vertical de la pelota), además
comprobamos que la paleta no se salga de la pantalla

El resto es crear y mostrar la paleta del jugador 2 (que es controlado
por el computador), que es análogo a lo hecho anteriormente, quedando:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import os
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480
    IMG_DIR = "imagenes"

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    def load_image(nombre, dir_imagen, alpha=False):
        # Encontramos la ruta completa de la imagen
        ruta = os.path.join(dir_imagen, nombre)
        try:
            image = pygame.image.load(ruta)
        except:
            print("Error, no se puede cargar la imagen: " + ruta)
            sys.exit(1)
        # Comprobar si la imagen tiene "canal alpha" (como los png)
        if alpha is True:
            image = image.convert_alpha()
        else:
            image = image.convert()
        return image


    # -----------------------------------------------
    # Creamos los sprites (clases) de los objetos del juego:


    class Pelota(pygame.sprite.Sprite):
        "La bola y su comportamiento en la pantalla"

        def __init__(self):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("bola.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = SCREEN_WIDTH / 2
            self.rect.centery = SCREEN_HEIGHT / 2
            self.speed = [3, 3]

        def update(self):
            if self.rect.left < 0 or self.rect.right > SCREEN_WIDTH:
                self.speed[0] = -self.speed[0]
            if self.rect.top < 0 or self.rect.bottom > SCREEN_HEIGHT:
                self.speed[1] = -self.speed[1]
            self.rect.move_ip((self.speed[0], self.speed[1]))

        def colision(self, objetivo):
            if self.rect.colliderect(objetivo.rect):
                self.speed[0] = -self.speed[0]


    class Paleta(pygame.sprite.Sprite):
        "Define el comportamiento de las paletas de ambos jugadores"

        def __init__(self, x):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("paleta.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = x
            self.rect.centery = SCREEN_HEIGHT / 2

        def humano(self):
            # Controlar que la paleta no salga de la pantalla
            if self.rect.bottom >= SCREEN_HEIGHT:
                self.rect.bottom = SCREEN_HEIGHT
            elif self.rect.top <= 0:
                self.rect.top = 0

        def cpu(self, objetivo):
            self.rect.centery = objetivo.rect.centery
            if self.rect.bottom >= SCREEN_HEIGHT:
                self.rect.bottom = SCREEN_HEIGHT
            elif self.rect.top <= 0:
                self.rect.top = 0

    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("Ejemplo de un Pong Simple")

        # cargamos los objetos
        fondo = load_image("fondo.jpg", IMG_DIR, alpha=False)
        bola = Pelota()
        jugador1 = Paleta(40)
        jugador2 = Paleta(SCREEN_WIDTH - 40)

        clock = pygame.time.Clock()
        pygame.key.set_repeat(1, 25)  # Activa repeticion de teclas
        pygame.mouse.set_visible(False)

        # el bucle principal del juego
        while True:
            clock.tick(60)
            # Obtenemos la posicon del mouse
            pos_mouse = pygame.mouse.get_pos()
            mov_mouse = pygame.mouse.get_rel()

            # Actualizamos los obejos en pantalla
            jugador1.humano()
            jugador2.cpu(bola)
            bola.update()

            # Comprobamos si colisionan los objetos
            bola.colision(jugador1)
            bola.colision(jugador2)

            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit(0)
                elif event.type == pygame.KEYDOWN:
                    if event.key == K_UP:
                        jugador1.rect.centery -= 5
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 5
                    elif event.key == K_ESCAPE:
                        sys.exit(0)
                elif event.type == pygame.KEYUP:
                    if event.key == K_UP:
                        jugador1.rect.centery += 0
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 0
                # Si el mouse no esta quieto mover la paleta a su posicion
                elif mov_mouse[1] != 0:
                    jugador1.rect.centery = pos_mouse[1]

            # actualizamos la pantalla
            screen.blit(fondo, (0, 0))
            screen.blit(bola.image, bola.rect)
            screen.blit(jugador1.image, jugador1.rect)
            screen.blit(jugador2.image, jugador2.rect)
            pygame.display.flip()


    if __name__ == "__main__":
        main()

Muy lindo el juego, pero esto tiene un problema; de la manera en que el
jugador 2 esta programado es invencible, ya que el oponente siempre
llega a la pelota (ya que se mueve junto a ella).

{{< youtube MvnBKefZuZo >}}

Parte 8: Sonidos básicos
========================

Ahora vamos por los sonidos, quedando el código de esta manera:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import os
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480
    IMG_DIR = "imagenes"
    SONIDO_DIR = "sonidos"

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    def load_image(nombre, dir_imagen, alpha=False):
        # Encontramos la ruta completa de la imagen
        ruta = os.path.join(dir_imagen, nombre)
        try:
            image = pygame.image.load(ruta)
        except:
            print("Error, no se puede cargar la imagen: " + ruta)
            sys.exit(1)
        # Comprobar si la imagen tiene "canal alpha" (como los png)
        if alpha is True:
            image = image.convert_alpha()
        else:
            image = image.convert()
        return image


    def load_sound(nombre, dir_sonido):
        ruta = os.path.join(dir_sonido, nombre)
        # Intentar cargar el sonido
        try:
            sonido = pygame.mixer.Sound(ruta)
        except (pygame.error) as message:
            print("No se pudo cargar el sonido:", ruta)
            sonido = None
        return sonido

    # -----------------------------------------------
    # Creamos los sprites (clases) de los objetos del juego:


    class Pelota(pygame.sprite.Sprite):
        "La bola y su comportamiento en la pantalla"

        def __init__(self, sonido_golpe, sonido_punto):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("bola.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = SCREEN_WIDTH / 2
            self.rect.centery = SCREEN_HEIGHT / 2
            self.speed = [3, 3]
            self.sonido_golpe = sonido_golpe
            self.sonido_punto = sonido_punto

        def update(self):
            if self.rect.left < 0 or self.rect.right > SCREEN_WIDTH:
                self.speed[0] = -self.speed[0]
                self.sonido_punto.play()  # Reproducir sonido de punto
            if self.rect.top < 0 or self.rect.bottom > SCREEN_HEIGHT:
                self.speed[1] = -self.speed[1]
            self.rect.move_ip((self.speed[0], self.speed[1]))

        def colision(self, objetivo):
            if self.rect.colliderect(objetivo.rect):
                self.speed[0] = -self.speed[0]
                self.sonido_golpe.play()  # Reproducir sonido de rebote


    class Paleta(pygame.sprite.Sprite):
        "Define el comportamiento de las paletas de ambos jugadores"

        def __init__(self, x):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("paleta.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = x
            self.rect.centery = SCREEN_HEIGHT / 2

        def humano(self):
            # Controlar que la paleta no salga de la pantalla
            if self.rect.bottom >= SCREEN_HEIGHT:
                self.rect.bottom = SCREEN_HEIGHT
            elif self.rect.top <= 0:
                self.rect.top = 0

        def cpu(self, objetivo):
            self.rect.centery = objetivo.rect.centery
            if self.rect.bottom >= SCREEN_HEIGHT:
                self.rect.bottom = SCREEN_HEIGHT
            elif self.rect.top <= 0:
                self.rect.top = 0

    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        pygame.mixer.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("Ejemplo de un Pong Simple")

        # cargamos los objetos
        fondo = load_image("fondo.jpg", IMG_DIR, alpha=False)
        sonido_golpe = load_sound("tennis.ogg", SONIDO_DIR)
        sonido_punto = load_sound("aplausos.ogg", SONIDO_DIR)

        bola = Pelota(sonido_golpe, sonido_punto)
        jugador1 = Paleta(40)
        jugador2 = Paleta(SCREEN_WIDTH - 40)

        clock = pygame.time.Clock()
        pygame.key.set_repeat(1, 25)  # Activa repeticion de teclas
        pygame.mouse.set_visible(False)

        # el bucle principal del juego
        while True:
            clock.tick(60)
            # Obtenemos la posicon del mouse
            pos_mouse = pygame.mouse.get_pos()
            mov_mouse = pygame.mouse.get_rel()

            # Actualizamos los obejos en pantalla
            jugador1.humano()
            jugador2.cpu(bola)
            bola.update()

            # Comprobamos si colisionan los objetos
            bola.colision(jugador1)
            bola.colision(jugador2)

            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit(0)
                elif event.type == pygame.KEYDOWN:
                    if event.key == K_UP:
                        jugador1.rect.centery -= 5
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 5
                    elif event.key == K_ESCAPE:
                        sys.exit(0)
                elif event.type == pygame.KEYUP:
                    if event.key == K_UP:
                        jugador1.rect.centery += 0
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 0
                # Si el mouse no esta quieto, mover la paleta a su posicion
                elif mov_mouse[1] != 0:
                    jugador1.rect.centery = pos_mouse[1]

            # actualizamos la pantalla
            screen.blit(fondo, (0, 0))
            screen.blit(bola.image, bola.rect)
            screen.blit(jugador1.image, jugador1.rect)
            screen.blit(jugador2.image, jugador2.rect)
            pygame.display.flip()


    if __name__ == "__main__":
        main()

Lo primero que se se hace es crear una función que cargue los
sonidos *load\_sound*, es análoga a la función para cargar imágenes. Para
abrir el archivo usamos el modulo `pygame.mixer.Sound
<http://www.losersjuegos.com.ar/traducciones/pygame/mixer/sound>`_
abriendo el sonido de la forma *sonido = pygame.mixer.Sound(ruta)*

Luego en el bucle principal del juego hay que iniciar el modulo mixer
(que se encarga de los sonidos), haciendo *pygame.mixer.init()*. Luego
simplemente se cargan los sonidos usando la función que se definio
anterioremente, o sea:

.. code-block:: python

    sonido_golpe = load_sound("tennis.ogg", SONIDO_DIR)
    sonido_punto = load_sound("aplausos.ogg", SONIDO_DIR)

Los cuales se le pasan luego a la clase pelota haciendo *bola =
Pelota(sonido\_golpe, sonido\_punto)*. Dentro de la clase pelota estos
sonidos se almacenan en *self.sonido\_golpe* y *self.sonido\_punto*.

Luego en la clase de la pelota usando `Sound.play()
<http://www.losersjuegos.com.ar/traducciones/pygame/mixer/sound#play>`_
se reproducen los sonidos. Así en el método *colision()* se le indica
que si la pelota colisiona con una paleta reproduzca el archivo
"tennis.ogg" (que se guardo en self.sonido\_golpe) haciendo
*self.sonido\_golpe.play()* y en el método *update()* al llegar a los
bordes izquierdo y derecho de la pantalla se reproduce unos aplausos
haciendo *self.sonido\_punto.play()*.

Parte 9: Mejorando el juego (y modificar al oponente)
=====================================================

Nuestro juego tiene los siguientes problemas:

1. Al actualizar la pantalla, tenemos que cargar uno por uno los sprites (lo
   que no es muy ordenado), así que es mejor agruparlos.
2. El oponente es invencible...
3. Al marcar un punto, la bola debería volver al centro.

Para lo primero es bastante fácil, simplemente juntamos los sprites de
la forma todos = pygame.sprite.RenderPlain(bola, jugador1, jugador2) y
luego los mostramos todos de una vez haciendo **todos.draw(screen)**.

  Nota:
  RenderPlain y RenderClear son alias para `Group
  <http://www.losersjuegos.com.ar/traducciones/pygame/sprite/group>`_ (se
  mantienen por compatibilidad), por lo tanto se manipulan igual que `Group
  <http://www.losersjuegos.com.ar/traducciones/pygame/sprite/group>`_.

Ahora mejoramos al oponente (o más bien hay que modificarlo para que no sea
invencible), quedando de esta manera.

.. code-block:: python

    def cpu(self, pelota):
        self.speed = [0, 2.5]
        if pelota.speed[0] >= 0 and pelota.rect.centerx >= SCREEN_WIDTH / 2:
            if self.rect.centery > pelota.rect.centery:
                self.rect.centery -= self.speed[1]
            if self.rect.centery < pelota.rect.centery:
                self.rect.centery += self.speed[1]

No es muy diferente de la versión anterior, pero en este caso se define
una velocidad (que en el eje y es de 2.5, lo cual es menor a lo 3 de la
pelota) y luego se comprueba que la pelota se mueva a la derecha (hacia
la paleta) con **pelota.speed[0] >= 0** y que la pelota haya pasado la
mitad de la pantalla **pelota.rect.centerx >= SCREEN\_WIDTH / 2 **, si
ambas condiciones se cumplen se comienza a mover la paleta (en caso
contrario se queda quieta).

Ahora si se cumplen las condiciones para que se mueva la paleta, se
compara la posición de la paleta con la pelota: si la pelota esta mas
arriba que la paleta, esta ultima se mueve hacia arriba, por otro lado
si la pelota esta abajo de la paleta, esta se mueve hacia abajo.

Con esto el oponente ya no es invencible, debido a que:

1. solo se mueve si la pelota se acerca a el y a pasado la mitad de la
   pantalla, por lo que el resto del tiempo esta quieto (y por lo tanto
   la posición de la paleta y la pelota no coinciden siempre)
2. la paleta se mueve mas lento que la pelota, por lo cual en tramos
   largos no es capaz de alcanzar a la pelota.

Combinando ambas, el computador puede perder (pese a que la superficie
de la paleta es mayor a la pelota)

    Nota: esto no es `Inteligencia
    artificial <http://es.wikipedia.org/wiki/Inteligencia_artificial>`_
    como tal, sino que es más cercano a un `comportamiento
    predeterminado <http://es.wikipedia.org/wiki/Comportamiento>`_

Para el ultimo problema, simplemente en la parte en donde le actualiza
la bola, luego de reproducir el sonido, reiniciamos la posición de la
bola.

La versión final del juego queda de la siguiente manera:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/

    # ---------------------------
    # Importacion de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import os
    import sys

    # -----------
    # Constantes
    # -----------

    SCREEN_WIDTH = 640
    SCREEN_HEIGHT = 480
    IMG_DIR = "imagenes"
    SONIDO_DIR = "sonidos"

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    def load_image(nombre, dir_imagen, alpha=False):
        # Encontramos la ruta completa de la imagen
        ruta = os.path.join(dir_imagen, nombre)
        try:
            image = pygame.image.load(ruta)
        except:
            print("Error, no se puede cargar la imagen: " + ruta)
            sys.exit(1)
        # Comprobar si la imagen tiene "canal alpha" (como los png)
        if alpha is True:
            image = image.convert_alpha()
        else:
            image = image.convert()
        return image


    def load_sound(nombre, dir_sonido):
        ruta = os.path.join(dir_sonido, nombre)
        # Intentar cargar el sonido
        try:
            sonido = pygame.mixer.Sound(ruta)
        except (pygame.error) as message:
            print("No se pudo cargar el sonido:" + ruta)
            sonido = None
        return sonido

    # -----------------------------------------------
    # Creamos los sprites (clases) de los objetos del juego:


    class Pelota(pygame.sprite.Sprite):
        "La bola y su comportamiento en la pantalla"

        def __init__(self, sonido_golpe, sonido_punto):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("bola.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = SCREEN_WIDTH / 2
            self.rect.centery = SCREEN_HEIGHT / 2
            self.speed = [3, 3]
            self.sonido_golpe = sonido_golpe
            self.sonido_punto = sonido_punto

        def update(self):
            if self.rect.left < 0 or self.rect.right > SCREEN_WIDTH:
                self.speed[0] = -self.speed[0]
                self.sonido_punto.play()  # Reproducir sonido de punto
                self.rect.centerx = SCREEN_WIDTH / 2
                self.rect.centery = SCREEN_HEIGHT / 2
            if self.rect.top < 0 or self.rect.bottom > SCREEN_HEIGHT:
                self.speed[1] = -self.speed[1]
            self.rect.move_ip((self.speed[0], self.speed[1]))

        def colision(self, objetivo):
            if self.rect.colliderect(objetivo.rect):
                self.speed[0] = -self.speed[0]
                self.sonido_golpe.play()  # Reproducir sonido de rebote


    class Paleta(pygame.sprite.Sprite):
        "Define el comportamiento de las paletas de ambos jugadores"

        def __init__(self, x):
            pygame.sprite.Sprite.__init__(self)
            self.image = load_image("paleta.png", IMG_DIR, alpha=True)
            self.rect = self.image.get_rect()
            self.rect.centerx = x
            self.rect.centery = SCREEN_HEIGHT / 2

        def humano(self):
            # Controlar que la paleta no salga de la pantalla
            if self.rect.bottom >= SCREEN_HEIGHT:
                self.rect.bottom = SCREEN_HEIGHT
            elif self.rect.top <= 0:
                self.rect.top = 0

        def cpu(self, pelota):
            self.speed = [0, 2.5]
            if pelota.speed[0] >= 0 and pelota.rect.centerx >= SCREEN_WIDTH / 2:
                if self.rect.centery > pelota.rect.centery:
                    self.rect.centery -= self.speed[1]
                if self.rect.centery < pelota.rect.centery:
                    self.rect.centery += self.speed[1]


    # ------------------------------
    # Funcion principal del juego
    # ------------------------------


    def main():
        pygame.init()
        pygame.mixer.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((SCREEN_WIDTH, SCREEN_HEIGHT))
        pygame.display.set_caption("Ejemplo de un Pong Simple")

        # cargamos los objetos
        fondo = load_image("fondo.jpg", IMG_DIR, alpha=False)
        sonido_golpe = load_sound("tennis.ogg", SONIDO_DIR)
        sonido_punto = load_sound("aplausos.ogg", SONIDO_DIR)

        bola = Pelota(sonido_golpe, sonido_punto)
        jugador1 = Paleta(40)
        jugador2 = Paleta(SCREEN_WIDTH - 40)

        clock = pygame.time.Clock()
        pygame.key.set_repeat(1, 25)  # Activa repeticion de teclas
        pygame.mouse.set_visible(False)

        # el bucle principal del juego
        while True:
            clock.tick(60)
            # Obtenemos la posicon del mouse
            pos_mouse = pygame.mouse.get_pos()
            mov_mouse = pygame.mouse.get_rel()

            # Actualizamos los obejos en pantalla
            jugador1.humano()
            jugador2.cpu(bola)
            bola.update()

            # Comprobamos si colisionan los objetos
            bola.colision(jugador1)
            bola.colision(jugador2)

            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit(0)
                elif event.type == pygame.KEYDOWN:
                    if event.key == K_UP:
                        jugador1.rect.centery -= 5
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 5
                    elif event.key == K_ESCAPE:
                        sys.exit(0)
                elif event.type == pygame.KEYUP:
                    if event.key == K_UP:
                        jugador1.rect.centery += 0
                    elif event.key == K_DOWN:
                        jugador1.rect.centery += 0
                # Si el mouse no esta quieto, mover la paleta a su posicion
                elif mov_mouse[1] != 0:
                    jugador1.rect.centery = pos_mouse[1]

            # actualizamos la pantalla
            screen.blit(fondo, (0, 0))
            todos = pygame.sprite.RenderPlain(bola, jugador1, jugador2)
            todos.draw(screen)
            pygame.display.flip()


    if __name__ == "__main__":
        main()

{{< youtube ev00oxId-yI >}}

Bueno eso es todo por ahora, pueden descargar\ `todos estos ejemplos desde
aquí <http://sites.google.com/site/dbfuentes/archivos/tutorial-pygame-3.zip?attredirects=0&d=1>`__
(o buscarlo en el repositorio de
`github <http://github.com/dbfuentes/tutorial-pygame>`_)

Lo unico que le faltaria al juego es un sistema de puntuaciones, pero
para eso primero hay que aprender como mostrar texto con pygame, lo cual
es uno de los temas a tratar en la siguiente parte de este
tutorial (`la parte 4
<https://www.pythonmania.net/es/2010/07/14/tutorial-pygame-4-figuras-y-texto>`_
).

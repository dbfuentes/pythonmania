+++
title = "Tutorial Pygame 4: manejo de figuras y texto (simular el lanzamiento de un proyectil)"
slug = "tutorial-pygame-4-figuras-y-texto"
date = "2010-07-14T13:37:00-04:00"
lastmod = "2016-08-15T21:17:09-04:00"
categories = ["tutoriales"]
tags = ["python2", "juegos", "pygame"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

En esta `parte del
tutorial <https://www.pythonmania.net/es/2010/03/23/tutorial-pygame-introduccion/>`_
se va a simular el lanzamiento de un proyectil (una esfera), para
mostrar las siguientes temas:

-  Rellenar el fondo con un color especifico
-  Dibujar lineas en la pantalla
-  Dibujar un circulo
-  Sincronizar el movimiento usando tiempo

<!--more-->

A continuación la explicación (aunque es recomendable que hayan leído
`la parte
anterior <https://www.pythonmania.net/es/2010/04/07/tutorial-pygame-3-un-videojuego/>`_
antes de seguir):


Paso 1: Creando lo básico (rellenar el fondo y dibujar lineas)
==============================================================

Lo primero es crear la ventana, que tendrá un fondo azul y en donde
dibujaremos una linea blanca que servirá de separador (en la parte de
arriba se mostrara la información más adelante).

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/07/14/tutorial-pygame-4-figuras-y-texto

    # ---------------------------
    # Importación de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import sys

    # -----------
    # Constantes
    # -----------

    WIDTH = 640
    HEIGHT = 480

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    # ------------------------------
    # Función principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((WIDTH, HEIGHT))
        pygame.display.set_caption("tutorial pygame parte 4")

        # el bucle principal del juego
        while True:
            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()
                elif event.type == pygame.KEYDOWN:
                    if event.key == K_ESCAPE:
                        sys.exit()

            # Re dibujar los elementos en pantalla
            screen.fill((30, 145, 255))
            pygame.draw.line(screen, (255, 255, 255), (0, 25), (640, 25), 2)
            # actualizamos la pantalla
            pygame.display.flip()


    if __name__ == "__main__":
        main()

La mayor parte de este código la explicamos en `la segunda
parte <https://www.pythonmania.net/es/2010/03/25/tutorial-pygame-2-ventana-e-imagenes/>`_
del tutorial. Lo nuevo es que en vez de usar una imagen de fondo,
utilizamos un color para rellenar la pantalla al hacer *screen.fill((30,
145, 255))* que en términos simples llena la pantalla con un color
celeste, el color se ingresa usando `el valor RGB del
color <http://es.wikipedia.org/wiki/Modelo_de_color_RGB>`_, que en este
caso es (30, 145, 255).

    El RGB (Red, Green, Blue) es un modelo de color en donde cualquier
    color se puede formar mediante los colores primarios (rojo, verde,
    azul).

    Los colores primarios se representan de la siguiente manera: el rojo
    es el (255,0,0), el verde con (0,255,0) y el azul con (0,0,255). La
    ausencia de color (o sea el negro) es el (0,0,0) y el blanco
    (255,255,255)

    Cualquier otro color se obtiene con la mezcla de estos, como por
    ejemplo el amarillo se obtiene al combinar el rojo con el verde, o
    sea (255,255,0) y lo mismo para el resto de los colores.

Lo otro fue dibujar una linea blanca en la ventana, para dibujar lineas
se usa `line del modulo
draw <http://www.losersjuegos.com.ar/traducciones/pygame/draw#line>`_
de la siguiente forma:

.. code-block:: python

    pygame.draw.line(Superficie, Color, Inicio, Fin, Grosor)

Que en este caso con *pygame.draw.line(screen, (255, 255, 255), (0, 25),
(640, 25), 2)* Ocurre lo siguiente: la superficie sobre la cual se
dibuja es la pantalla (que llamamos screen), el color de la linea es
blanco (255, 255, 255) y dibuja os una linea paralela a 25 pixeles del
borde, por lo que comienza en (0, 25) y termina en (640, 25) y esta
linea es de 2 pixeles de ancho (grosor).

    Existe también el pygame.draw.aaline() que dibuja lineas aplicando
    anti-alias (suavizar el borde de linea para que la linea no se vea
    pixelada o escalonada) y su uso es el siguiente

    .. code-block:: python

        pygame.draw.aaline(Superficie, Color, Inicio, Fin, blend=1)

    Si blend está en valor True (1), las figuras se mezclarán con la
    tonalidad de los pixeles existentes en lugar de sobre-escribirlos.

Paso 2: Crear el proyectil (dibujar un circulo) y mostrar texto
===============================================================

Ahora dibujaremos un circulo (que sera nuestro proyectil) en la esquina
inferior izquierda y crearemos un sprite que contenga la información del
proyectil.

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/07/14/tutorial-pygame-4-figuras-y-texto

    # ---------------------------
    # Importación de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import sys

    # -----------
    # Constantes
    # -----------

    WIDTH = 640
    HEIGHT = 480

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    class Proyectil(pygame.sprite.Sprite):
        "Clase que representa el proyectil lanzado"

        def __init__(self, x, y):
            self.angulo = 45
            self.veloc = 50
            self.tiempo = 0
            self.x = x
            self.y = y
            self.xreal = x
            self.yreal = HEIGHT - self.y


    # ------------------------------
    # Función principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((WIDTH, HEIGHT))
        pygame.display.set_caption("tutorial pygame parte 4")

        # se define la letra por defecto
        fuente = pygame.font.Font(None, 20)

        # se crea un proyectil a lanzar
        bala = Proyectil(0, HEIGHT)

        # el bucle principal del juego
        while True:
            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()
                elif event.type == pygame.KEYDOWN:
                    if event.key == K_ESCAPE:
                        sys.exit()

            text = "Velocidad: %3d (m/s)   Angulo: %d   x=%d m   y=%d m" % (
                bala.veloc, bala.angulo, bala.xreal, bala.yreal)
            mensaje = fuente.render(text, 1, (255, 255, 255))

            # Re dibujar los elementos en pantalla
            screen.fill((30, 145, 255))
            screen.blit(mensaje, (15, 5))
            pygame.draw.line(screen, (255, 255, 255), (0, 25), (640, 25), 2)
            pygame.draw.circle(screen, (0, 0, 0), (int(bala.x), int(bala.y)), 10)
            # actualizamos la pantalla
            pygame.display.flip()


    if __name__ == "__main__":
        main()

Creamos una clase (sprite) que representa nuestro proyectil y le
ingresamos algunos valores por defecto como son el angulo y velocidad de
disparo, o sea esta parte:

.. code-block:: python

    class Proyectil(pygame.sprite.Sprite):
        "Clase que representa el proyectil lanzado"

        def __init__(self, x, y):
            self.angulo = 45
            self.veloc = 50
            self.tiempo = 0
            self.x = x
            self.y = y
            self.xreal = x
            self.yreal = HEIGHT - self.y

Si se fijan el tiempo lo dejamos en 0 (ya que comenzara a contar a
partir del disparo del proyectil) y definimos dos valores self.x, self.y
que representan la posición del proyectil en la pantalla y otro par de
valores self.xreal, self.yreal que después tomaran la posición que se
calcule que tendrá el proyectil, estos valores serán diferentes en el
eje y, ya que pygame considera el (0,0) en la equina superior izquierda
y las formulas físicas que describen el movimiento (ver mas adelante)
consideran el (0,0) en la esquina inferior izquierda. Mas adelante (en
el while True) iniciamos esta clase (bajo el nombre de "bala") con la
posición inicial de esta (en este caso la esquina inferior izquierda).

Ahora vamos a mostrar en pantalla un texto (que aparecerá en la parte
superior de la ventana) indicando la información básica del proyectil:
angulo y velocidad de disparo (al momento de disparar) y su posición.
Para mostrar el texto primero definimos la fuente a utilizar y su tamaño
haciendo: *fuente = pygame.font.Font(None, 20)*

    En pygame.font.Font el primer valor es la ubicación y nombre de la
    fuente utilizada. Si se ingresa None se utiliza la fuente por
    defecto.

    El segundo valor es la altura de la fuente (en pixeles).

Luego almacenamos el mensaje como una cadena de texto en la variable
"text" (usando la información obtenida desde la clase "bala") y este
texto se tiene que dibujar en alguna superficie (pygame no puede dibujar
el texto directamente, sino hay que crear una "imagen" de de ese texto),
para lo cual usamos *mensaje = fuente.render(text, 1, (255, 255, 255))*
con lo cual almacenamos esa "imagen" en el objeto mensaje (tal como si
fuera un sprite) y después de manera análoga a un sprite esa "imagen" se
muestra en la pantalla usando *screen.blit(mensaje, (15, 5))* donde (15,
5) es la posición en que se muestra (arriba a la izquierda)

    El `Font.render
    <http://www.losersjuegos.com.ar/traducciones/pygame/font/font#render>`_
    toma como primer valor la cadena de texto a mostrar, el segundo
    valor es si usa antialias (=1) o no y el tercero es el color del
    texto. Se puede agregar un cuarto valor para crear un borde en el
    texto

Por ultimo hacemos: *pygame.draw.circle(screen, (0, 0, 0), (int(bala.x),
int(bala.y)), 10)* En este caso dibujamos un circulo en la la
pantalla/ventana (que llamamos screen) este es de un color negro, que
esta en la posición (bala.x, bala.y) o sea en la esquina inferior
izquierda (aunque después al disparar el proyectil estos valores se
modifican, moviendo el circulo), los int son para transformar los
valores a números enteros, ya que los valores originales pueden ser
decimales y el ultimo valor le indica que el circulo tiene un radio de
10 px.

Paso 3: preparando el proyectil
===============================

Ahora vamos a modificar un poco nuestro juego, de tal manera que podamos
aumentar o disminuir tanto el angulo como la velocidad de disparo.

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/07/14/tutorial-pygame-4-figuras-y-texto

    # ---------------------------
    # Importación de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import sys

    # -----------
    # Constantes
    # -----------

    WIDTH = 640
    HEIGHT = 480

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    class Proyectil(pygame.sprite.Sprite):
        "Clase que representa el proyectil lanzado"

        def __init__(self, x, y):
            self.angulo = 45
            self.veloc = 50
            self.tiempo = 0
            self.x = x
            self.y = y
            self.disparar = False
            self.xreal = x
            self.yreal = HEIGHT - self.y

        def update(self):
            "actualizar la posición del proyectil"

            if self.disparar is True:
                # esta en movimiento, hay que actualizar la posición
                pass
            else:
                # se mantiene sin disparar, por lo cual no se hace nada
                pass

            # si sale de la pantalla reiniciar la posición (a inferior izq.)
            if (self.y > HEIGHT) or (self.x > WIDTH):
                self.x = 0
                self.y = HEIGHT
                self.disparar = False

    # ------------------------------
    # Función principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((WIDTH, HEIGHT))
        pygame.display.set_caption("tutorial pygame parte 4")

        # se define la letra por defecto
        fuente = pygame.font.Font(None, 20)

        # se crea un proyectil a lanzar
        bala = Proyectil(0, HEIGHT)

        pygame.key.set_repeat(1, 80)  # Activa repetición de teclas

        # el bucle principal del juego
        while True:
            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()
                elif event.type == pygame.KEYDOWN:
                    if event.key == K_UP:
                        if bala.angulo < 90:
                            bala.angulo = bala.angulo + 1
                    elif event.key == K_DOWN:
                        if bala.angulo > 0:
                            bala.angulo = bala.angulo - 1
                    elif event.key == K_RIGHT:
                        if bala.veloc < 100:
                            bala.veloc = bala.veloc + 1
                    elif event.key == K_LEFT:
                        if bala.veloc > 10:
                            bala.veloc = bala.veloc - 1
                    elif event.key == K_SPACE:
                        bala.disparar = True
                    elif event.key == K_ESCAPE:
                        sys.exit()

            # Actualizar la posición e información
            bala.update()
            text = "Velocidad: %3d (m/s)   Angulo: %d   x=%d m   y=%d m" % (
                bala.veloc, bala.angulo, bala.xreal, bala.yreal)
            mensaje = fuente.render(text, 1, (255, 255, 255))

            # Re dibujar los elementos en pantalla
            screen.fill((30, 145, 255))
            screen.blit(mensaje, (15, 5))
            pygame.draw.line(screen, (255, 255, 255), (0, 25), (640, 25), 2)
            pygame.draw.circle(screen, (0, 0, 0), (int(bala.x), int(bala.y)), 10)
            # actualizamos la pantalla
            pygame.display.flip()


    if __name__ == "__main__":
        main()

El código es bastante sencillo, en la parte en donde se detecta que
teclas se han pulsado se limita la velocidad y angulo de disparo de la
siguiente manera: Al pulsar la flecha hacia arriba que se aumente el
angulo (hasta un máximo de 90 `grados
(sexagesimales) <http://es.wikipedia.org/wiki/Grado_sexagesimal>`_, o
sea un angulo recto) y la flecha hacia abajo lo disminuye hasta un
angulo de 0 grados. Mientes que a la derecha se aumenta la velocidad
(hasta un máximo de 100) y a la izquierda se disminuye la velocidad
(hasta un mínimo de 10).

Dentro de la clase proyectil (o sea la bala) se ha definido un metodo
update (que después es utilizado en el while True) y que va actualizando
la posición del proyectil en la pantalla. En este método se agrega una
comprobación de tal manera que si el proyectil sale de la pantalla los
valores se reinicien.

Si se fijan el valor por defecto para self.disparar es False. De esta
manera al pulsar a barra espaciadora se dispara el proyectil (cambiando
el valor de disparar a True), por lo que al llamar el bala.update() se
va calculando la nueva posición de la bala (a continuación se explicara
como obtener la nueva posición).

Un poco de física
-----------------

Antes de continuar hay que entender la base del lanzamiento de
proyectiles, la cual voy a intentar explicarla de una manera sencilla a
continuación.

Voy a considerar el punto (0,0) "el origen" como la esquina inferior
izquierda, para que se mantengan las ecuaciones de la misma forma en que
se encontrarían en un libro (después se va a ajustar el valor para que
corresponda con el de pygame).

Como se trata de un `lanzamiento de
proyectiles <http://es.wikipedia.org/wiki/Trayectoria_bal%C3%ADstica>`_,
podemos aplicar algunos supuestos para simplificar el problema:

.. raw:: html

    <strike>

- La vaca es esférica y sin rozamiento

.. raw:: html

    </strike>

-  No tendremos en cuenta el efecto de rotación de la Tierra (que desvía
   el proyectil)

-  La velocidad del proyectil es suficientemente pequeña como para
   despreciar la resistencia que presenta el aire en oposición a su
   movimiento (roce)

-  La aceleración de gravedad **g** es constante (no varia su valor
   con la altura ni con curvatura de la superficie terrestre)

Entonces tenemos (de forma simplificada) las siguientes ecuaciones para
determinar la posición (en cualquier momento) de un objeto en el eje x e
y:

.. math::
    x = x_{0} + v_{x}t + {\frac{1}{2}}at^2

    y = y_{0} + v_{y}t + {\frac{1}{2}}at^2

Como en nuestro caso, el objeto a lanzar (la esfera o proyectil) esta
inicialmente en el origen (0,0) por lo cual:

.. math::
    x_{0} = 0

    y_{0} = 0

Además la única aceleración que afecta el proyectil es la gravedad (y
que solo influye de manera vertical), por lo tanto en este caso queda:

.. math::
    x = 0 + v_{x}t

    y = 0 + v_{y}t + {\frac{1}{2}}gt^2

Esto es una parábola. Si consideraremos a la `aceleración de
gravedad <http://es.wikipedia.org/wiki/Intensidad_de_la_gravedad>`_ con
un valor de g = -9,8 (el signo menos es porque actúa hacia abajo) y la
velocidad con la que se lanza el proyectil `es un
vector <http://es.wikipedia.org/wiki/Vector_%28f%C3%ADsica%29>`_ (que
depende del ángulo y su valor), por lo que hay que descomponerla en sus
componentes horizontal (Vx) y vertical (Vy).

Ya que es un vector `la suma de sus
componentes <http://es.wikipedia.org/wiki/Vector_%28f%C3%ADsica%29#Componentes_de_un_vector>`_
(horizontal y vertical) da el valor del modulo del vector. Además el
modulo del vector con las componentes de este en el eje x e y forman un
triangulo, por lo que aplicando
`trigonometría <http://es.wikipedia.org/wiki/Trigonometr%C3%ADa#Razones_trigonom.C3.A9tricas>`_
se puede determinar los valores de los componentes horizontal y
vertical:

.. image:: https://pythonmania.files.wordpress.com/2010/07/pygame_vector_velocidad.png
   :width: 400px
   :height: 308px
   :target: https://pythonmania.files.wordpress.com/2010/07/pygame_vector_velocidad.png
   :alt: vector velocidad

En la imagen se aprecia el triangulo que se forma, así tenemos las
siguientes razones trigonometricas:

.. math::

    cos{\alpha} = {\frac{v_{x}}{v}}

    sen{\alpha} = {\frac{v_{y}}{v}}

Despejando tenemos:

.. math::

    v_{x} = {v} {cos{\alpha}}

    v_{y} = {v} {sen{\alpha}}

Con lo cual se puede obtener la componente en ambos ejes de la
velocidad.

Así en resumen, el proyectil tendrá el siguiente comportamiento:

-  De forma horizontal solo depende de la velocidad (que es constante),
   por lo que se moverá en este caso hacia la derecha
-  De forma vertical tenemos a la velocidad que lo impulsa hacia arriba
   y la gravedad que se opone (hacia abajo). Por lo cual el proyectil en
   un inicio comenzara a subir, pero luego por la aceleración de
   gravedad comenzara a caer (el tiempo esta al cuadrado en el segundo
   termino, por lo cual al aumentar t (el tiempo), la aceleración por el
   tiempo al cuadrado terminara superando a la velocidad por el tiempo y
   en ese instante empezara a caer)

Paso 4: Movimiento del proyectil
================================

Ahora aplicamos lo anterior en nuestro programa, por lo que modificamos
el método update para que a medida que trascurre el tiempo calcule la
posición del proyectil, por lo la versión final de nuestro juego /
simulador queda de la siguiente manera:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: X11/MIT license http://www.opensource.org/licenses/mit-license.php
    # https://www.pythonmania.net/es/2010/07/14/tutorial-pygame-4-figuras-y-texto

    # ---------------------------
    # Importación de los módulos
    # ---------------------------

    import pygame
    from pygame.locals import *
    import sys
    import math

    # -----------
    # Constantes
    # -----------

    WIDTH = 640
    HEIGHT = 480

    # ------------------------------
    # Clases y Funciones utilizadas
    # ------------------------------


    class Proyectil(pygame.sprite.Sprite):
        """Clase que representa el proyectil lanzado"""

        def __init__(self, x, y):
            self.angulo = 45
            self.veloc = 50
            self.tiempo = 0
            self.x = x
            self.y = y
            self.disparar = False
            self.xreal = x
            self.yreal = HEIGHT - self.y

        def update(self):
            "actualizar la posición del proyectil"
            self.velocx = self.veloc * math.cos(math.radians(self.angulo))
            self.velocy = self.veloc * math.sin(math.radians(self.angulo))

            if self.disparar is True:
                # esta en movimiento, hay que actualizar la posición
                self.xreal = (0 + self.velocx * self.tiempo)
                self.yreal = (0 + self.velocy * self.tiempo +
                              (-9.8 * (self.tiempo ** 2)) / 2)
                # Corregir la posición en el eje vertical
                self.x = self.xreal
                self.y = HEIGHT - self.yreal
            else:
                # se mantiene sin disparar, por lo cual no se hace nada
                pass

            # si sale de la pantalla reiniciar la posición (a inferior izq.)
            if (self.y > HEIGHT) or (self.x > WIDTH):
                self.x = 0
                self.y = HEIGHT
                self.tiempo = 0
                self.disparar = False

    # ------------------------------
    # Función principal del juego
    # ------------------------------


    def main():
        pygame.init()
        # creamos la ventana y le indicamos un titulo:
        screen = pygame.display.set_mode((WIDTH, HEIGHT))
        pygame.display.set_caption("tutorial pygame parte 4")

        # se define la letra por defecto
        fuente = pygame.font.Font(None, 20)

        # se crea un proyectil a lanzar
        bala = Proyectil(0, HEIGHT)

        pygame.key.set_repeat(1, 80)  # Activa repetición de teclas
        clock = pygame.time.Clock()

        # el bucle principal del juego
        while True:
            # registramos cuanto ha pasado desde el ultimo ciclo
            tick = clock.tick(60)
            # Posibles entradas del teclado y mouse
            for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    sys.exit()
                elif event.type == pygame.KEYDOWN:
                    if event.key == K_UP:
                        if bala.angulo < 90 and bala.disparar is False:
                            bala.angulo = bala.angulo + 1
                    elif event.key == K_DOWN:
                        if bala.angulo > 0 and bala.disparar is False:
                            bala.angulo = bala.angulo - 1
                    elif event.key == K_RIGHT:
                        if bala.veloc < 100 and bala.disparar is False:
                            bala.veloc = bala.veloc + 1
                    elif event.key == K_LEFT:
                        if bala.veloc > 10 and bala.disparar is False:
                            bala.veloc = bala.veloc - 1
                    elif event.key == K_SPACE:
                        bala.disparar = True
                    elif event.key == K_ESCAPE:
                        sys.exit()

            if bala.disparar is True:
                # al tiempo anterior le sumamos lo transcurrido
                bala.tiempo = bala.tiempo + (tick / 1000.0)

            # Actualizar la posición e información
            bala.update()
            text = "Velocidad: %3d (m/s)   Angulo: %d   x=%d m   y=%d m" % (
                bala.veloc, bala.angulo, bala.xreal, bala.yreal)
            mensaje = fuente.render(text, 1, (255, 255, 255))

            # Re dibujar los elementos en pantalla
            screen.fill((30, 145, 255))
            screen.blit(mensaje, (15, 5))
            pygame.draw.line(screen, (255, 255, 255), (0, 25), (640, 25), 2)
            pygame.draw.circle(screen, (0, 0, 0), (int(bala.x), int(bala.y)), 10)
            # actualizamos la pantalla
            pygame.display.flip()


    if __name__ == "__main__":
        main()

Si se fijan ahora iniciamos un reloj (modulo time) *clock =
pygame.time.Clock()* y luego al comienzo del *while True* registramos
cuanto tiempo (en milisegundos) han pasado desde la llamada anterior del
clock.tick (en la parte anterior del tutorial explique que tick devuelve
el tiempo que ha pasado entre llamadas), por ello hacemos: *tick =
clock.tick(60)*

    Observación: Use *tick = clock.tick(60)* pero es igual de valido
    (y funciona de la misma manera) si se usa *tick = clock.tick()*
    ya que ambas devuelven el tiempo trascurrido y en este caso la
    sincronización del movimiento de los objetos en el juego depende del
    tiempo (y no del framerate)

    El principal motivo para haber usado *clock.tick(60)* es que de esta
    manera se controla el framerate (a la vez que se obtiene cuantos
    milisegundos han pasado), así si tienen un equipo muy potente,
    podemos limitar como corre el juego y que no ocupe todo los recursos
    del equipo al ejecutarse (cosa que se agradece). Para que se hagan
    una idea al usar clock.tick() el juego estaba usando los dos nucleos
    de mi CPU al 100% y alcanzaba unos frames bestiales (y que no eran
    necesarios), en cambio clock.tick(60) el movimiento era igual de
    fluido y el procesador estaba al funcionando al mínimo.

Volviendo al tema, ya que en el tick esta almacenado el tiempo que ha
pasado entre llamadas, ahora simplemente se comprueba si se ha disparado
la bala y si es así hay que sumar el tiempo trascurrido al que se tenia
registrado (que inicialmente era 0 y que en cada ciclo del juego va
aumentando), o sea esto:

.. code-block:: python

    if bala.disparar is True:
        bala.tiempo = bala.tiempo + (tick / 1000.0)

Nota: Se divide por 1000.0 para transformar el tiempo del tick desde
milisegundos a segundos (ya que la velocidad esta dada en m/seg). Se usa
1000.0 en vez de 1000 para que los resultados sean decimales (punto
flotante).

Ahora en la clase del proyectil, se modifica el método update para que
calcule la posición del proyectil, de la siguiente manera:

.. code-block:: python

    def update(self):
        "actualizar la posición del proyectil"
        self.velocx = self.veloc * math.cos(math.radians(self.angulo))
        self.velocy = self.veloc * math.sin(math.radians(self.angulo))

        if self.disparar is True:
            # esta en movimiento, hay que actualizar la posición
            self.xreal = (0 + self.velocx * self.tiempo)
            self.yreal = (0 + self.velocy * self.tiempo +
                (-9.8 * (self.tiempo ** 2)) / 2)
            # Corregir la posición en el eje vertical
            self.x = self.xreal
            self.y = HEIGHT - self.yreal

Primero se calcula la componente de la velocidad en el eje x
(*self.velocx*) e eje y (*self.velocy*), para ello se multiplica la
velocidad (*self.veloc*) por el coseno (*math.cos*) y seno (*math.sin*)
de angulo respectivamente. Eso si el angulo inicial lo tenemos en
`grados
(sexagesimal) <http://es.wikipedia.org/wiki/Grado_sexagesimal>`_ y las
funciones math.sin y math.cos trabajan en
`radianes <http://es.wikipedia.org/wiki/Radianes>`_, por lo que usamos
la función
`*math.radians* <http://docs.python.org/library/math.html#math.radians>`_
para pasar de grados a radianes (aunque también podríamos haber
trabajado desde un comienzo en radianes, pero eso le hubiera quitado la
gracia al ejemplo).

Luego para calcular la posición teórica de la bala usamos las
ecuaciones que se mencionaron en la explicación física, o sea:

.. math::
    x = 0 + v_{x}t

    y = 0 + v_{y}t + {\frac{1}{2}}gt^2


Que aplicadas en nuestro código quedan:

.. code-block:: python

    self.xreal = (0 + self.velocx * self.tiempo)
    self.yreal = (0 + self.velocy * self.tiempo + (-9.8 * (self.tiempo ** 2)) / 2)

y posteriormente corregimos la posición para que se ajuste al sentido de
los ejes que usa pygame; la posición en el eje x se deja igual *self.x =
self.xreal* y la posición en el eje y se invierte *self.y = HEIGHT -
self.yreal*

Por ultimo en la entrada del teclado se pone una comprobación extra para
que no se puedan modificar los datos mientras el proyectil esta en
movimiento.

{{< youtube Gj2jWFT8aLY >}}

Bueno eso es todo por ahora, pueden descargar `todos estos ejemplos desde
aquí <http://sites.google.com/site/dbfuentes/archivos/tutorial-pygame-4.zip?attredirects=0&d=1>`_
(o buscarlo en el repositorio de
`github <http://github.com/dbfuentes/tutorial-pygame>`_).

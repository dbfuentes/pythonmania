+++
title = "Introducción a pyGtk y Glade"
slug = "introduccion-a-pygtk-y-glade"
date = "2009-02-05T15:54:00-04:00"
categories = ["tutoriales"]
tags = ["python2", "glade", "pyGtk"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

De vuelta de mis vacaciones, con las pilas recargadas, se me ha ocurrido
escribir un pequeño tutoríal de introducción a la creación de interfaces
gráficas usando python, GTK+ y glade. El cual dejo a continuación.

El tutoríal esta dividido en dos partes, en cada parte se explica como
crear una ventana que tiene un texto (una etiqueta de texto para ser
exactos), un cuadro donde escribir y un botón. La idea es que al
escribir un texto en la entrada de texto (el cuadro), y al presionar el
botón OK (o la tecla ENTER) el texto se muestra en la etiqueta de la
forma "Hola **nuestro\_texto**". No es un gran programa pero sirve
de ejemplo.

<!--more-->

En la primer parte explica como crear el ejemplo directamente sobre el
código, o sea construir la interfaz y conectarla directamente, todo esto
desde el mismo archivo en python, o sea todo hecho a mano. Mientras que
en la segunda parte se explica como crear la interfaz desde el diseñador
de interfaces Glade (de forma separada y bastante intuitiva) y luego
conectarla (llamarla) desde el archivo en python.

Pero antes de empezar, hay que instalar
`pygtk <http://www.pygtk.org/>`_ y el soporte para
`glade <http://glade.gnome.org/>`_, desde debian (y derivadas como
ubuntu) es tan fácil como hacer un:

``apt-get install python-gtk2 python-glade2``

Además para la segunda parte se necesita
`glade <http://glade.gnome.org/>`_ para crear la interfaz gráfica
(Aunque no es necesario para ejecutarla), así que hacemos:

``apt-get install glade``

Ahora manos a la obra:

Primer Ejemplo: Directamente desde el código
============================================

| En este caso es recomendable echar una ojeada rápida `al manual de
  pygtk <http://www.pygtk.org/pygtk2tutorial-es/>`__
| , no es completamente necesario pero ayuda a entender mejor como
  funcionan los widgets y las señales.

A continuación el ejemplo, los comentarios explican el código:

.. code-block:: python

    #! /usr/bin/env python
    # -*- coding: UTF-8 -*-

    # Importamos el módulo pygtk y le indicamos que use la versión 2 (en caso de
    # que existan varias versiones de pygtk instaladas en el sistema)
    import pygtk
    pygtk.require("2.0")

    # Luego importamos el módulo de gtk para poder acceder a los controles de Gtk+
    import gtk

    # Creamos una clase que contenga la ventana principal del programa y los
    # métodos de cada una las señales
    class MainWin:

        # Primero definimos como sera la ventana:
        def __init__(self):

            # Creamos una ventana toplevel (o sea que esta al frente de todas las
            # ventanas) llamada "main_win" y fijamos su titulo como "Ejemplo 1"
            main_win = gtk.Window(gtk.WINDOW_TOPLEVEL)
            main_win.set_title("Ejemplo 1")

            # A main_win le conectamos una señal (destroy), esto hará que cada
            # vez que se presione el botón salir (la cruz del manejador de
            # ventanas) se llamará al método on_quit que cerrara la ventana
            main_win.connect("destroy", self.on_quit)

            # Para agregar widgets (controles como botones, etiquetas, etc.) a la
            # ventana, primero es necesario crear contenedores como cajas que
            # contengan las widgets. En este ejemplo creamos una caja vertical con
            # un espacio entre widgets de 5px y con la propiedad homogéneo en False
            vbox = gtk.VBox(False, 5)

            # Creamos una etiqueta con el texto "Hola mundo", se usa la palabra
            # reservada "self" de python para poder hacer referencia a esta
            # etiqueta desde otros métodos
            self.label = gtk.Label("Hola mundo")

            # Creamos un cuadro donde escribir (una entrada de texto en blanco)
            # y luego le conectamos la señal "activate" que llama al método
            # "on_button1_clicked", esto producirá que cuando se haga click en el
            # botón Ok (que se creara mas adelante) la entrada de texto reaccione
            self.entry = gtk.Entry()
            self.entry.connect("activate", self.on_button1_clicked)

            # Ahora creamos el botón, que sera el botón OK del inventario de
            # botones de GNOME.
            # Y luego le indicamos al botón que cuando le hagan click emita la
            # señal "clicked" que llamará a "on_button1_clicked"
            button = gtk.Button(stock=gtk.STOCK_OK)
            button.connect("clicked", self.on_button1_clicked)

            # Ahora que ya creamos las widgets (la etiqueta, la entrada de texto y
            # el botón) hay que añadirlos a la caja vertical creada anteriormente

            # Primero le añadimos la etiqueta llamada label a la caja vertical
            vbox.add(self.label)

            # Luego añadimos al inicio de la segunda fila la entrada de texto
            # activando las propiedades de expandir, rellenar y espaciado.
            vbox.pack_start(self.entry, True, True, 0)

            # Finalmente en la tercer fila agregamos el botón.
            vbox.pack_start(button, False, False, 0)

            # Ahora agregamos la caja vertical a la ventana y luego se muestra
            # la caja (y todo lo que contiene) en la ventana principal.
            main_win.add(vbox)
            main_win.show_all()


        # Ahora dentro de nuestra clase principal "MainWin" tenemos que definir
        # que hacen cada uno de los métodos que se llamaron anteriormente

        # Primero definamos el método "on_button1_clicked"
        def on_button1_clicked(self, widget):
            # Primero obtenemos el texto que se escriba en la entrada de texto
            texto = self.entry.get_text()
            # Luego fijamos ese texto a la etiqueta de la forma "Hola texto".
            self.label.set_text("Hola %s" % texto)

        # Ahora se define el método "on_quit" que destruye la aplicación
        def on_quit(self, widget):
            gtk.main_quit()


    # Para terminar iniciamos el programa
    if __name__ == "__main__":
        # Iniciamos la clase.
        MainWin()
        # Además iniciamos el método gtk.main, que genera un ciclo que se utiliza
        # para recibir todas las señales emitidas por los botones y demás widgets.
        gtk.main()

Como todos los script de python los podemos ejecutar haciendo
``$ python ejemplo.py``.

Nota: pueden descargar el código (y demás archivos) de ambos ejemplos
`desde este .zip <http://www.mediafire.com/?eztkzzz5rmo>`_

Segundo Ejemplo: pyGtk + Glade
==============================

En este caso vamos a crear toda la interfaz gráfica usando glade, el
cual crea un archivo con extensión \*.glade en donde guardan los
nombres, señales, etc. de cada uno de los widget y de las ventanas. Así
en el archivo .py solo se indica las widgets que se van a utilizar y que
hacen los métodos.

Primero lanzamos glade y desde ventana principal crearemos un nuevo
proyecto de gtk. Si se fijan bien el programa tiene una paleta con los
widgets que se pueden usar y una ventana de propiedades que muestra la
información de cada widget y permite modificarla (ambas se ven la imagen
a continuación), estas las usaremos frecuentemente así que ubiquenla (en
glade2 se pueden activar/desactivar desde el menú ver).

.. image:: https://pythonmania.files.wordpress.com/2009/02/introduccion_pygtk_01.png
    :width: 450px
    :height: 445px
    :alt: introduccion a pygtk

Ahora desde la paleta hacemos click en el widget ventana (gtk.Window)
para crear una ventana, luego seleccionamos la ventana creada y
modificamos su información usando la ventana de propiedades en donde
podemos cambiar su nombre, titulo, ancho, largo, etc. en este caso le
cambiaremos el titulo a "Ejemplo 2".

El siguiente paso es crear la caja vertical, para eso en la paleta
hacemos click en el widget caja vertical (gtk.VBox), luego otro click en
la ventana en donde queremos colocarla y la dejamos con 3 filas. Nos
queda algo como esto:

.. image:: https://pythonmania.files.wordpress.com/2009/02/introduccion_pygtk_02.png
    :width: 327px
    :height: 220px
    :alt: introduccion a pygtk

Continuaremos insertando una etiqueta (gtk.Label) en la primer fila,
después insertamos una entrada de texto (gtk.Entry) en la segunda fila y
por último un botón (gtk.Button) en la tercera fila. Así nos queda una
ventana como esta

.. image:: https://pythonmania.files.wordpress.com/2009/02/introduccion_pygtk_03.png
    :width: 246px
    :height: 106px
    :alt: introduccion a pygtk

Ahora cambiamos las propiedades de esta etiqueta para que el nombre sera
"label1" y el texto que muestra lo cambiamos de "label1" a "Hola mundo".
En el caso de la entrada de texto (gtk.entry) le dejamos su nombre como
"entry1" y para el botón le dejamos el nombre como "button1" y escogemos
el botón de inventario de aceptar(gtk-ok).

El próximo paso es conectar las señales que emiten los eventos de cada
widget, esto se hace en propiedades, en la pestaña "Señales". Primero
seleccionamos la ventana (window1), buscamos la señal "destroy" y
seleccionamos el manejador gtk\_main\_quit (creo que ya saben para que
es esto).

.. image:: https://pythonmania.files.wordpress.com/2009/02/introduccion_pygtk_04.png
    :width: 325px
    :height: 411px
    :alt: introduccion a pygtk

De igual manera seleccionamos para el con el entry1 la señal "activate"
y el manejador "on\_button1\_clicked"(si no lo encuentran escribanlo).
Por último el para el button1 escogemos la señal "clicked" y el
manejador "on\_button1\_clicked".

Nota: Se puedes usar manejadores personalizados, pero hay que tener
cuidado el escribir el script en python.

Finalmente hay que guardar el archivo con extensión .glade en el mismo
directorio donde vayamos a crear el archivo de python, en mi caso lo
llamare "GladeEjemplo.glade" (aunque tal como lo señalo mas adelante, se
puede usar otros directorios si se tiene cuidado).

Por ultimo nuestro script queda así (los comentarios explican el código):

.. code-block:: python

    #! /usr/bin/env python
    # -*- coding: UTF-8 -*-

    # Importamos el módulo pygtk y le indicamos que use la versión 2
    import pygtk
    pygtk.require("2.0")

    # Luego importamos el módulo de gtk y el gtk.glade, este ultimo que nos sirve
    # para poder llamar/utilizar al archivo de glade
    import gtk
    import gtk.glade

    # Creamos la clase de la ventana principal del programa
    class MainWin:

        def __init__(self):
            # Le decimos a nuestro programa que archivo de glade usar (puede tener
            # un nombre distinto del script). Si no esta en el mismo directorio del
            # script habría que indicarle la ruta completa en donde se encuentra
            self.widgets = gtk.glade.XML("GladeEjemplo.glade")

            # Creamos un pequeño diccionario que contiene las señales definidas en
            # glade y su respectivo método (o llamada)
            signals = { "on_entry1_activate" : self.on_button1_clicked,
                        "on_button1_clicked" : self.on_button1_clicked,
                        "gtk_main_quit" : gtk.main_quit }

            # Luego se auto-conectan las señales.
            self.widgets.signal_autoconnect(signals)
            # Nota: Otra forma de hacerlo es No crear el diccionario signals y
            # solo usar "self.widgets.signal_autoconnect(self)" -->Ojo con el self

            # Ahora obtenemos del archivo glade los widgets que vamos a
            # utilizar (en este caso son label1 y entry1)
            self.label1 = self.widgets.get_widget("label1")
            self.entry1 = self.widgets.get_widget("entry1")

        # Se definen los métodos, en este caso señales como "destroy" ya fueron
        # definidas en el .glade, así solo se necesita definir "on_button1_clicked"
        def on_button1_clicked(self, widget):
            texto = self.entry1.get_text()
            self.label1.set_text("Hola %s" % texto)

    # Para terminar iniciamos el programa
    if __name__ == "__main__":
        MainWin()
        gtk.main()

Nota: pueden descargar el código (y demás archivos) de ambos ejemplos
`desde este
.zip <http://sites.google.com/site/dbfuentes/archivos/introduccion_pygtk_ejemplos.zip?attredirects=0&d=1>`__
(o también `desde aquí <http://www.mediafire.com/?eztkzzz5rmo>`_).

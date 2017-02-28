+++
title = "Threads en pygtk"
slug = "threads-en-pygtk"
date = "2009-05-21T12:46:00-04:00"
categories = ["tutoriales"]
tags = ["python2"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Uno de los problemas que se producen al crear aplicaciones en pygtk es
que si se ejecuta algún comando externo o dentro del mismo código algo
que se demore su tiempo en ser ejecutado, mientras se lleva a cabo esta
tarea la ventana literalmente se congela/bloquea hasta que se termina de
realizar esta tarea, además que no muestra el resultado hasta que
termina (y en muchos casos uno quiere mostrar el progreso mientras se
ejecuta la tarea).

<!--more-->

Una de las soluciones a esto es usar threads (hilos), pero NO "de la
manera normal" como es usan, ya que si se hace de forma normal, los
widgets no se comportan siempre como es debido y a veces los threads ni
siquiera funcionan.

La solución es usar los métodos **gtk.gdk.threads\_init()**,
**gtk.gdk.threads\_enter()** y **gtk.gdk.threads\_leave()** de la
siguiente forma:

-  Al inicio de código hay que poner gtk.gdk.threads\_init() para
   indicarle a la aplicación que se van a usar hilos en la interfaz
   gráfica (linea 13 del ejemplo).
-  Luego en la parte del código que que hace el trabajo pesado (y que
   posiblemente congele la aplicación de manera temporal) hay que poner
   el gtk.gdk.threads\_enter() antes de esas líneas y
   gtk.gdk.threads\_leave() después de esas lineas (lineas 25 y 33 del
   ejemplo).

Ahora veamos el ejemplo (con los correspondientes comentarios), mas
adelante lo explico con mas detalles:

.. code-block:: python

    #!/usr/bin/env python

    # Escrito por Daniel Fuentes B.
    # Licencia: BSD <http://www.opensource.org/licenses/bsd-license.php>

    import pygtk
    pygtk.require("2.0")
    import gtk
    import os
    import threading

    # iniciamos los thead (los iniciamos, aun no lo usamos)
    gtk.gdk.threads_init()

    comando = "find /var"

    # Esta funcion hace el trabajo "pesado", luego es llamada desde la ventana
    def salida_comando(salida_texto, buffer_texto, comando):
        output = os.popen(comando)
        while True:
            linea = output.readline()
            if not linea:
                break
            # Comienza el trababjo pesado, asi que entramos en un thread
            gtk.gdk.threads_enter()
            # Se inserta la siguiente linea de la salida del proceso y luego se
            # coloca el cursor al final del textview (se mueve el scroll)
            iter = buffer_texto.get_end_iter()
            buffer_texto.place_cursor(iter)
            buffer_texto.insert(iter, linea)
            salida_texto.scroll_to_mark(buffer_texto.get_insert(), 0.1)
            # Finaliza el trabajo pesado, asi que salimos del thread
            gtk.gdk.threads_leave()

    # Clase con la ventana principal.
    class MainWindow:
        def  __init__(self):
            # Definimos la ventana inicial
            ventana = gtk.Window(gtk.WINDOW_TOPLEVEL)
            ventana.set_title("Ejemplo de threading en gtk")
            ventana.resize(450,300)
            ventana.connect("destroy", gtk.main_quit)

            # Creamos una caja vertical con un espacio entre widgets de 5px y con
            # la propiedad homogeneo en False. Luego agragamos las widgets al vbox
            vbox = gtk.VBox(False, 5)

            # Una simple etiqueta que se muestra en la parte superior de la ventana
            etiqueta = gtk.Label("Resultado del comando:")

            # Creamos un textview que tenga un auto scroll, o sea que se desplace
            # solo al aparecer nuevo texto que sobrepase la pantalla
            # en este textview se mostrara la salida del comando ejecutado
            scrollwin = gtk.ScrolledWindow()
            scrollwin.set_policy(gtk.POLICY_AUTOMATIC, gtk.POLICY_AUTOMATIC)
            salida_texto = gtk.TextView()
            buffer_texto = salida_texto.get_buffer()
            scrollwin.add(salida_texto)

            # creamos un boton, el que iniciara el comando al hacer click en el
            boton = gtk.Button("iniciar programa")
            boton.connect("clicked", self.on_boton_clicked, salida_texto, buffer_texto, comando)

            # Empacamos el textview con scroll y el boton dentro del vbox
            vbox.pack_start(etiqueta, False, False, 0)
            vbox.pack_start(scrollwin)
            vbox.pack_start(boton, False)

            # y luego unimos el vbox la ventana pricipal y la mostramos
            ventana.add(vbox)
            ventana.show_all()

        def on_boton_clicked(self, widget, salida_texto, buffer_texto, comando):
            # Este boton es el lanza el hilo (threading) al hacer click sobre el
            hilo = threading.Thread(target=salida_comando, args=(salida_texto, buffer_texto, comando))
            hilo.start()

    if __name__ == "__main__":
        MainWindow()
        gtk.main()

Lo que hace el programa es ejecutar un comando *"find /var"* y luego
muestra la salida de este en la ventana.

La clase MainWindow crea la ventana de pygtk, es muy parecida al primer
ejemplo que vimos `en el tutorial de introducción a
pygtk <https://www.pythonmania.net/es/2009/02/05/introduccion-a-pygtk-y-glade/>`_,
el único detalle es que al hacer click en el boton
(*on\_boton\_clicked*) se inicia un thread (hilo) que ejecuta la función
salida\_comando con los correspondientes argumentos.

En cuanto a la salida\_comando es una función que ejecuta un comando
(usando os.popen) tal como si uno lo escribiera en un terminal y luego
toma el texto que resulta de ejecutar el comando y lo muestra en el
textview de nuestra ventana. Esta funcion contiene la parte del código
que hace el trabajo pesado, por esto tiene los gtk.gdk.threads\_enter()
y gtk.gdk.threads\_leave()

Y así se ve la ventana funcionando:

.. image:: https://pythonmania.files.wordpress.com/2009/05/threads_en_pygtk.png
    :width: 450px
    :height: 326px
    :alt: threads en pygtk

Claro que esta no es la única forma de hacerlo, o sino miren en `las
faqs de
pygtk <http://faq.pygtk.org/index.py?req=show&file=faq23.020.htp>`_

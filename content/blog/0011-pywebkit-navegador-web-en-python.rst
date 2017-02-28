+++
title = "PyWebKit, navegador web en python"
slug = "pywebkit-navegador-web-en-python"
date = "2010-02-18T22:25:00-04:00"
categories = ["tutoriales"]
tags = ["python2", "pyGtk", "web"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Mientras actualizaba debian me encontré con un paquete bastante curioso
`python-webkit <http://packages.debian.org/sid/python-webkit>`_ (en
donde me di cuenta que su nombre en realidad es
`pywebkitgtk <http://code.google.com/p/pywebkitgtk/>`_) y para quien se
pregunte que demonios es webkit, simplemente es el motor de renderizado
de de algunos navegadores como
`safari <http://es.wikipedia.org/wiki/Safari_%28navegador%29>`_ o la
pokébola de google (`Chrome <http://es.wikipedia.org/wiki/Google_Chrome>`_),
en otras palabras es el encargado de "mostrar" o "dibujar" las páginas web
dentro de un navegador. Claro que hay otras alternativas como
`pygtkmozembed <http://www.pygtk.org/pygtkmozembed/>`_ (que usa el
motor de firefox) pero no dejan mucho margen para manipularlo.

Volviendo al tema principal, en resumen vamos a hacer un intento de
navegador web, en donde tendremos una barra de direcciones (un entry de
gtk) y abajo va a mostrar la pagina que le indiquemos.

<!--more-->

Primero instalamos pywebkit, de debian basta con hacer un:

``# apt-get install python-webkit libwebkit-dev``

O sino lo bajan `desde su
sitio <http://code.google.com/p/pywebkitgtk>`_

Luego creamos un navegador muy simple:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: utf-8 -*-

    # Escrito por Daniel Fuentes B.
    # Licencia: BSD <http://www.opensource.org/licenses/bsd-license.php>

    import pygtk
    pygtk.require("2.0")
    import gtk
    import webkit

    class Browser:
        # Ventana del programa
        def __init__(self):
            self.window = gtk.Window(gtk.WINDOW_TOPLEVEL)
            self.window.set_position(gtk.WIN_POS_CENTER)
            self.window.set_default_size(800, 600)
            self.window.connect("destroy", self.on_quit)

            # Un vBox, en la parte de arriba hay una caja para ingresar
            # la direccion web, y abago se muestra la pagina
            vbox = gtk.VBox()

            # La parte de la entrada de la url
            self.url_text = gtk.Entry()
            self.url_text.connect('activate', self.url_text_activate)

            # La parte en donde se muestra la pagina que se visita (con scroll incluido)
            self.scroll_window = gtk.ScrolledWindow()
            self.webview = webkit.WebView()
            self.scroll_window.add(self.webview)

            # Unimos todo en el vBox
            vbox.pack_start(self.url_text, fill=True, expand=False)
            # El expand=False al empaquetarlo es para que el entry no ocupe media pantalla
            vbox.pack_start(self.scroll_window, True, True)
            self.window.add(vbox)
            self.window.show_all()

        # Definimos las señales y demas cosas de la ventana:
        def url_text_activate(self, entry):
        # al activar el entry (por ejemplo al hacer enter), se obtiene el
        # texto de la entry (la url) y se activa la funcion que abre la url
            self.open_url(entry.get_text())

        def on_quit(self, widget):
            gtk.main_quit()

        # La funcion magica que abre la url que se le pasa
        def open_url(self, url):
            "Funcion que carga la pagina elegida"
            # cambia el titulo de la ventana
            self.window.set_title("Ejemplo pywebkitgtk - %s" % url)
            # mostramos la direccion de la pagina abierta en el entry
            self.url_text.set_text(url)
            # abre la pagina
            self.webview.open(url)

    if __name__ == "__main__":
        browser = Browser()
        # abrimos la pagina de inicio (opcional)
        browser.open_url("http://code.google.com/p/pywebkitgtk/")
        gtk.main()

El codigo se entiende por si mismo. Si se dan cuenta casi todo el código
se para crear la ventana con pygtk (`tal como fue explicado hace mucho
tiempo en la introducción a pygtk que escribí
<https://www.pythonmania.net/es/2009/02/05/introduccion-a-pygtk-y-glade/>`_),
siendo la unica parte que involucra a webkit la parte en donde se define
open\_url (más o menos a partir de la linea 50).

Y les dejo una captura del ejemplo funcionando:

.. image:: https://pythonmania.files.wordpress.com/2010/02/pywebkit-gtk.png?w=300
    :width: 300px
    :height: 233px
    :target: https://pythonmania.files.wordpress.com/2010/02/pywebkit-gtk.png
    :alt: pywebkit-gtk

Si alguien se interesa en la `web del proyecto hay un navegador mucho
más
funcional <http://code.google.com/p/pywebkitgtk/source/browse/#svn/trunk/demos>`_
o puede revisar `este
blog <http://damncorner.blogspot.com/2009/10/cairo-pywebkit-y-pygtk-semana-de.html>`_

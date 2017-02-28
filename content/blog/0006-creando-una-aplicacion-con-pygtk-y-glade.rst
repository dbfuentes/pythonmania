+++
title = "Creando una aplicacion con pygtk y glade"
slug = "creando-una-aplicacion-con-pygtk-y-glade"
date = "2009-02-16T23:12:00-04:00"
categories = ["tutoriales"]
tags = ["python2", "glade", "pyGtk"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

En el siguiente tutorial voy a explicar como crear una aplicación simple
usando pygtk y glade, la aplicación que voy a crear es un programa que
convierte unidades de temperatura (por ejemplo pasa de Celsius a
Fahrenheit o Kelvin), claro que no es un gran programa pero es mucho
mejor que el "hola mundo" que hice para `el primer tutorial, la
introducción a
pygtk+glade <https://www.pythonmania.net/es/2009/02/05/introduccion-a-pygtk-y-glade/>`_.

En cuanto al nombre del programa, lo voy a llamar "python temperature
converter" (pytemp para abreviar). Así que sin mas preámbulo pasemos
al tutorial.

<!--more-->

Primera Etapa: Crear la ventana
===============================

Desde glade cree un nuevo proyecto de gtk (que llame pytemp), una vez
creado el proyecto, lo siguiente que creo es una nueva ventana (llamada
window1 y que tenga el titulo "Temperature Converter") y en esta ventana
agrego una caja vertical con 4 filas.

En la primera fila le pongo una barra de menú (que mas adelante voy a
editar), en la segunda fila de la caja vertical agrego una tabla (de 2
columnas y 4 filas), en la tercera fila de la caja vertical pongo un
separador horizontal y en la cuarta fila de la caja vertical creo una
caja horizontal con dos columnas (dentro de la caja horizontal en la
izquierda pongo un botón con la etiqueta "Convert" y que el botón use el
icono gtk-ok, mientras que en la parte derecha de la caja horizontal
pongo un textview). Todo esto crea una ventana como la siguiente:

.. image:: https://pythonmania.files.wordpress.com/2009/02/pytemp_001.png
    :width: 408px
    :height: 254px
    :alt: captura pytemp

Luego editamos el menú superior, para ello seleccionamos la barra del
menú y en las propiedades apretamos el botón "Editar menús...", nos sale
una ventana como esta:

.. image:: https://pythonmania.files.wordpress.com/2009/02/pytemp_002.png?w=480
    :width: 480px
    :height: 355px
    :target: http://pythonmania.files.wordpress.com/2009/02/pytemp_002.png
    :alt: captura pytemp

En esta ventana borramos todos los menús que no usaremos, así en en file
solo dejamos la opción salir y en help solo dejamos la opción about (tal
como lo muestra la imagen anterior)

    Nota: Si se preguntan cual es el motivo por que estoy creando la
    ventana en inglés, es debido a que planeo escribir pronto una
    tercera parte del tutorial en donde voy a explicar como traducir una
    aplicación python/pygtk y voy a usar este programa de ejemplo ;)

Volviendo a la muestra ventana, ahora vamos a trabajar sobre la tabla,
en la primera fila primera columna coloco una etiqueta con el texto
"Value".

Luego en la segunda fila, en la primera columna agrego una entrada de
texto que se llamara "entry1" y en la segunda columna de la tabla coloco
un GtkComboBox (que es una lista desplegable) llamado "combobox1" que
tenga los elementos Celsius, Fahrenheit y Kelvin (Nota: poner un
elemento por linea).

    La diferencia principal entre un
    `ComboBox <http://www.pygtk.org/docs/pygtk/class-gtkcombobox.html>`_
    y un
    `ComboBoxEntry <http://www.pygtk.org/docs/pygtk/class-gtkcomboboxentry.html>`_
    es que si bien es cierto que ambos le permiten al usuario
    seleccionar algún item a partir de la una lista desplegable, el
    ComboBoxEntry también le permite al usuario ingresar un nuevo ítem,
    cosa que no permite el ComboBox, además de algunas diferencias en
    como se cargan/manejan.

En cuanto a la tercera fila, la dejo en blanco para que quede algo de
separación entre los elementos.

Luego en la cuarta fila, en la primera columna ubico una etiqueta con el
texto "Target Unit:", y le modifico la propiedad Alineación X dejándola
en 0.80 (esto sirve para desplazar la etiqueta, 0.00 es completamente a
la izquierda, 0.50 en el centro y 1.00 completamente a la derecha). En
cuanto a la segunda columna coloco un GtkComboBox llamado "combobox2"
que tiene los mismos elementos que el anterior (el "combobox1").

Si todo sale bien queda una ventana como esta:

.. image:: https://pythonmania.files.wordpress.com/2009/02/pytemp_003.png
    :width: 271px
    :height: 208px
    :alt: captura pytemp

Finalmente creo las señales correspondientes que son las siguientes `(en
el tutorial anterior ya explique como
agragarlas) <https://www.pythonmania.net/es/2009/02/05/introduccion-a-pygtk-y-glade/>`_:

-  Elemento: "window1", señal "destroy" y el manejador "gtk\_main\_quit"
-  Elemento: "on\_"button1", señal "clicked" y el manejador
   “on\_button1\_clicked”
-  Elemento: "quit1" (En el menú File es la opción Quit), señal
   "activate" y el manejador "gtk\_main\_quit"
-  Elemento: "about1" (En el menú Help es la opción About), señal
   "activate" y el manejador "on\_about1\_activate"

Ahora pasamos al código.

Segunda Etapa: Creando el código
================================

Nota: Voy a tratar de explicar lo que hice de una manera que se entienda
claramente, lo cual no es necesariamente el orden que sigue el código,
por lo que de vez en cuando voy ha dar algunos saltos hacia atrás o
adelante en el código. En todo caso el archivo final esta bien
comentado, así que no es muy difícil que se pierdan.

Lo primero es importar los módulos que necesitamos (en este caso solo
son pygtk, gtk y gtk.glade), para lo cual hacemos:

.. code-block:: python

    # Importamos los módulos necesarios
    try:
        import pygtk
        pygtk.require('2.0') # Intenta usar la versión2
    except:
        # Algunas distribuciones vienen con GTK2, pero no con pyGTK (o pyGTKv2)
        pass

    try:
        import gtk
        import gtk.glade
    except:
        print "You need to install pyGTK or GTKv2 or set your PYTHONPATH correctly"
        sys.exit(1)

Ahora definimos varias funciones que convierten la temperatura. Para eso
vamos a\ `la wikipedia para saber que las conversiones
básicas <http://en.wikipedia.org/wiki/Temperature_conversion_formulas>`_
que son:

- C=(F-32)\*(5/9)
- K=C+273.15

Así nuestras funciones para convertir las temperaturas quedan:

.. code-block:: python

    # Definimos varias funciones que convierten la temperatura.

    def fahrenheit2celsius(temp):
        "Convert Fahrenheit to celsius"
        celsius = (temp - 32) * 5.0 / 9.0
        return celsius

    def celsius2fahrenheit(temp):
        "Convert Celsius to Fahrenheit"
        fahrenheit = (9.0 / 5.0) * temp + 32
        return fahrenheit

    def kelvin2celsius(temp):
        "Convert kelvin to Celsius"
        celsius = temp - 273.15
        return celsius

    def celsius2kelvin(temp):
        "Convert Celsius to kelvin"
        kelvin = temp + 273.15
        return kelvin

.

    Nota: Solo cree 4 conversiones. Si se preguntan por ejemplo como
    pasar de Fahrenheit a Kelvin, simplemente pasan primero de
    Fahrenheit a Celsius y luego ese resultado (en Celsius) lo pasan a
    Kelvin.

Luego vamos a crear una clase que almacena la información del
programa, así solo tengo que escribir esta información una vez y luego
la llamo cuando la necesite en alguna parte del programa, como por
ejemplo en la ventana about (Acerca de).

.. code-block:: python

    # Creamos una clase que almacena la información del programa (después se usara)
    class Info:
        "Store the program information"
        name = "pyTemp"
        version = "0.1"
        copyright = "Copyright © 2009 Daniel Fuentes B."
        authors = ["Daniel Fuentes Barría <dbfuentes @ gmail com>"]
        website = "https://pythonmania.wordpress.com/tutoriales/"
        description = "A temperature converter written in python using PyGTK"
        license = "This program is free software; you can redistribute it and/or \
    modify it under the terms of the GNU General Public License as published by \
    the Free Software Foundation; either version 2 of the License, or (at your \
    option) any later version. \n\nThis program is distributed in the hope that \
    it will be useful, but WITHOUT ANY WARRANTY; without even the implied \
    warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. \
    See the GNU General Public License for more details. \n\nYou should have \
    received a copy of the GNU General Public License along with this program; \
    if not, write to the Free Software Foundation, Inc., 51 Franklin Street, \
    Fifth Floor, Boston, MA 02110-1301, USA."

Fijense que la parte authors es una lista, así que si existieran mas de
un autor, simplemente abría que agregarlos como mas elementos de la
lista.

Ahora lo que creamos es una clase que tenga nuestra Interfaz gráfica,
que es como sigue:

.. code-block:: python

    # Interfaz gráfica (gtk-glade), Clase para el Loop principal (de la GUI)
    class MainGui:
        "GTK/Glade User interface. This is a pyGTK window"
        def __init__(self):
            # Le indicamos al programa que archivo XML de glade usar
            self.widgets = gtk.glade.XML("pytemp.glade")

            # se definen las signals
            signals = { "on_button1_clicked" : self.on_button1_clicked,
                        "on_about1_activate" : self.on_about1_activate,
                        "gtk_main_quit" : gtk.main_quit }

            # y se autoconectan las signals.
            self.widgets.signal_autoconnect(signals)

            # Del archivo glade obtenemos los widgets a usar
            self.entry1 = self.widgets.get_widget("entry1")
            self.textview1 = self.widgets.get_widget("textview1")
            self.combobox1 = self.widgets.get_widget("combobox1")
            self.combobox2 = self.widgets.get_widget("combobox2")

            # Para el ComboBox1 se fija por defecto la primera opción de la lista
            self.combobox1.set_active(0)
            # y en el ComboBox2 se fija por defecto la segunda opción de la lista
            self.combobox2.set_active(1)

Esto fue `explicado en el primer
tutorial, <https://www.pythonmania.net/es/2009/02/05/introduccion-a-pygtk-y-glade/>`_
asi que no voy a repetir la explicación, pero voy a hacer una observación
respecto a los ComboBox.

Por defecto al cargar un ComboBox aparecen sin nada seleccionado, como
no quiero que pase eso, le indico cual elemento tiene que estar activo
por defecto (escogido inicialmente) usando el set\_active(). En cuanto
al orden de los elementos del ComboBox el primero tiene indice 0, el
segundo 1 y así sucesivamente, por lo que le estoy diciendo al programa
que el combobox1 por defecto tenga seleccionado "Celsius" y el combobox2
tenga "Fahrenheit".

Lo que sigue es crear un par de ventanas, la primera sera una ventana
de error.

.. code-block:: python

    # Ventana genérica de error (se le pasan los mensajes de error y los
    # muestra en una ventana de dialogo)
    def error(self, message):
        "Display the error dialog "
        dialog_error = gtk.MessageDialog(parent=None, flags=0, buttons=gtk.BUTTONS_OK)
        dialog_error.set_title("Error")
        label = gtk.Label(message)
        dialog_error.vbox.pack_start(label, True, True, 0)
        # Con show_all() mostramos el contenido del cuadro de dialogo (en este
        # caso solo tiene la etiqueta) si no se hace el dialogo aparece vacío
        dialog_error.show_all()
        # El run y destroy hace que la ventana se cierre al apretar el botón
        dialog_error.run()
        dialog_error.destroy()

Esta ventana (que para ser exactos es un pequeño dialogo) nos muestra un
mensaje de error personalizado, si se preguntan por que no la cree en
glade, la respuesta es bastante simple, si la hubiera hecho en glade
para cada error tendría que estar modificando etiquetas y similares,
mientras que de esta manera simplemente cuando quiero mostrar un mensaje
hago self.error("Mi mensaje").

La siguiente es la ventana acerca de.

.. code-block:: python

    # Ventana About (conocida como Acerca de).
    def about_info(self, data=None):
        "Display the About dialog "
        about = gtk.AboutDialog()
        about.set_name(Info.name)
        about.set_version(Info.version)
        about.set_comments(Info.description)
        about.set_copyright(Info.copyright)

        def openHomePage(widget,url,url2): # Para abrir el sitio
            import webbrowser
            webbrowser.open_new(url)

        gtk.about_dialog_set_url_hook(openHomePage,Info.website)
        about.set_website(Info.website)
        about.set_authors(Info.authors)
        about.set_license(Info.license)
        about.set_wrap_license(True) # Adapta el texto a la ventana
        about.run()
        about.destroy()

En este caso creo la ventana desde el código (y no desde glade) porque
defino una función que al hacer click en la dirección del proyecto, la
abre en el navegador web por defecto. Nota: se puede hacer lo mismo con
las direcciones de email y similares, solo hay que `revisar la
documentación <http://www.pygtk.org/docs/pygtk/class-gtkaboutdialog.html#function-gtk--about-dialog-set-email-hook>`_.

Ahora nos queda establecer las acciones que hay que realizar al hacer
click en un botón o en el menú. En el caso del Help -> About es
bastante sencillo, resultando así:

.. code-block:: python

    # Definimos la ventana about (help > About)
    def on_about1_activate(self, widget):
        "Open the About windows"
        self.about_info()

En cuanto al botón para convertir las unidades la cosa se un poco mas
complicada, ya que al apretarlo tiene que obtener el valor desde el
entry1 y el texto activo (opción escogida) desde cada uno de los
ComboBox. Esto ultimo es un problema.

En teoría haciendo un combobox.get\_active() se obtiene el índice del
valor, pero esto tiene sus inconvenientes: El primero es que el valor
puede ser negativo, como por ejemplo -1 si no hay ninguna selección. El
otro problema es que si por ejemplo tenemos ordenada una lista de cierta
forma y en un futuro agregamos un elemento en medio de esa lista (por
ejemplo que a una lista de ciudades ordenadas alfabéticamente le agregue
una nueva ciudad) los idices de algunos elementos pueden quedar
desplazados, produciendo un desastre en nuestro codigo

Por estos motivos es mejor obtener el texto activo que el indice, para
lo cual vamos a usar una función que nos devuelva el texto activo de un
combobox (para ser mas exactos vamos a usar `una función propuesta en la
documentación de
pygtk <http://www.pygtk.org/pygtk2tutorial-es/sec-ComboBoxAndComboboxEntry.html>`_).


.. code-block:: python

    # Función que obtiene el texto de la opción seleccionada en un ComboBox
    # Se usa para obtener que unidad de temperatura seleccionada de la lista
    def valor_combobox(combobox):
        model = combobox.get_model()
        activo = combobox.get_active()
        if activo < 0:
            return None
        return model[activo][0]

Luego hacemos la conversion con esto:

.. code-block:: python

    # Definimos las acciones a realizar al apretar el botón de convertir
        def on_button1_clicked(self, widget):
            "Convert button"
            # Se crea un buffer en donde se guardaran los resultados
            text_buffer = gtk.TextBuffer()
            # Se obtiene el valor para convertir desde la entrada
            valor = self.entry1.get_text()
            # Obtiene la opción escogida en los 2 ComboBoxs
            selec1 = valor_combobox(self.combobox1)
            selec2 = valor_combobox(self.combobox2)
            try:
                # Intenta transformar el valor ingresado en un numero. En caso
                # de fallar (por ejemplo falla si lo ingresado son letras) se
                # lanza la excepción, si es exitoso se continua con la conversión
                temp_ini = float(valor)

                # Inicia la conversión adecuada dependiendo de la opción
                # escogida en los 2 ComboBoxs (selec1 y selec2)
                if selec1 == "Celsius" and selec2 == "Fahrenheit":
                    text_buffer.set_text(str(celsius2fahrenheit(temp_ini)))
                elif selec1 == "Celsius" and selec2 == "Kelvin":
                    text_buffer.set_text(str(celsius2kelvin(temp_ini)))
                elif selec1 == "Fahrenheit" and selec2 == "Celsius":
                    text_buffer.set_text(str(fahrenheit2celsius(temp_ini)))
                elif selec1 == "Fahrenheit" and selec2 == "Kelvin":
                    # Pasamos primero de F a Celsius, luego de Celsius a Kelvin
                    conversion1 = fahrenheit2celsius(temp_ini)
                    text_buffer.set_text(str(celsius2kelvin(conversion1)))
                elif selec1 == "Kelvin" and selec2 == "Celsius":
                    text_buffer.set_text(str(kelvin2celsius(temp_ini)))
                elif selec1 == "Kelvin" and selec2 == "Fahrenheit":
                     # Pasamos primero de Kelvin a Celsius, luego de Celsius a F
                     conversion1 = kelvin2celsius(temp_ini)
                     text_buffer.set_text(str(celsius2fahrenheit(conversion1)))
                else:
                    # Se produce cuando las dos selecciones son iguales
                    self.error("The initial and target units are the same")

                # Luego se fija (muestra) el buffer (que contiene la temperatura
                # convertida) en textview1 (el cuadro la lado del botón)
                self.textview1.set_buffer(text_buffer)

            except:
                if (len(valor) == 0):
                    # Se produce si no se ingresa nada en la entry1
                    self.error("Please enter a value")
                else:
                    # Se produce si no se ingresa un numero ( por ejemplo se
                    # ingresan letras o símbolos)
                    self.error("The value entered is not valid.\nEnter a \
    valid number")

La idea principal detrás del try: es que que tome el valor ingresado en
el "entry1" y lo transforme en un numero flotante, si no lo puede
transformar (porque no es un numero) se lanza la excepción y en caso de
que si lo transforme continúe haciendo la transformación que corresponda.

Finalmente agregamos un par de cosas (como que se inicie la clase
MainGui) y el código completo queda así:


.. code-block:: python

    #! /usr/bin/env python
    # -*- coding: UTF-8 -*-

    # pytemp - A temperature converter written in python using PyGTK
    # Copyright (C) 2009 Daniel Fuentes Barría
    #
    # This program is free software; you can redistribute it and/or modify
    # it under the terms of the GNU General Public License as published by
    # the Free Software Foundation; either version 2 of the License, or
    # (at your option) any later version.
    #
    # This program is distributed in the hope that it will be useful,
    # but WITHOUT ANY WARRANTY; without even the implied warranty of
    # MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    # GNU General Public License for more details.
    #
    # You should have received a copy of the GNU General Public License along
    # with this program; if not, write to the Free Software Foundation, Inc.,
    # 51 Franklin Street, Fifth Floor, Boston, MA 02110-1301 USA.

    # Importamos los módulos necesarios
    try:
        import pygtk
        pygtk.require('2.0') # Intenta usar la versión2
    except:
        # Algunas distribuciones vienen con GTK2, pero no con pyGTK (o pyGTKv2)
        pass

    try:
        import gtk
        import gtk.glade
    except:
        print "You need to install pyGTK or GTKv2 or set your PYTHONPATH correctly"
        sys.exit(1)

    # Definimos varias funciones que convierten la temperatura.

    def fahrenheit2celsius(temp):
        "Convert Fahrenheit to celsius"
        celsius = (temp - 32) * 5.0 / 9.0
        return celsius

    def celsius2fahrenheit(temp):
        "Convert Celsius to Fahrenheit"
        fahrenheit = (9.0 / 5.0) * temp + 32
        return fahrenheit

    def kelvin2celsius(temp):
        "Convert kelvin to Celsius"
        celsius = temp - 273.15
        return celsius

    def celsius2kelvin(temp):
        "Convert Celsius to kelvin"
        kelvin = temp + 273.15
        return kelvin

    # Función que obtiene el texto de la opción seleccionada en un ComboBox
    # Se usa para obtener que unidad de temperatura seleccionada de la lista
    def valor_combobox(combobox):
        model = combobox.get_model()
        activo = combobox.get_active()
        if activo < 0:
            return None
        return model[activo][0]

    # Creamos una clase que almacena la información del programa (después se usara)
    class Info:
        "Store the program information"
        name = "pyTemp"
        version = "0.1"
        copyright = "Copyright © 2009 Daniel Fuentes B."
        authors = ["Daniel Fuentes Barría <dbfuentes @ gmail com>"]
        website = "https://pythonmania.wordpress.com/tutoriales/"
        description = "A temperature converter written in python using PyGTK"
        license = "This program is free software; you can redistribute it and/or \
    modify it under the terms of the GNU General Public License as published by \
    the Free Software Foundation; either version 2 of the License, or (at your \
    option) any later version. \n\nThis program is distributed in the hope that \
    it will be useful, but WITHOUT ANY WARRANTY; without even the implied \
    warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. \
    See the GNU General Public License for more details. \n\nYou should have \
    received a copy of the GNU General Public License along with this program; \
    if not, write to the Free Software Foundation, Inc., 51 Franklin Street, \
    Fifth Floor, Boston, MA 02110-1301, USA."


    # Interfaz gráfica (gtk-glade), Clase para el Loop principal (de la GUI)
    class MainGui:
        "GTK/Glade User interface. This is a pyGTK window"
        def __init__(self):
            # Le indicamos al programa que archivo XML de glade usar
            self.widgets = gtk.glade.XML("pytemp.glade")

            # se definen las signals
            signals = { "on_button1_clicked" : self.on_button1_clicked,
                        "on_about1_activate" : self.on_about1_activate,
                        "gtk_main_quit" : gtk.main_quit }

            # y se autoconectan las signals.
            self.widgets.signal_autoconnect(signals)

            # Del archivo glade obtenemos los widgets a usar
            self.entry1 = self.widgets.get_widget("entry1")
            self.textview1 = self.widgets.get_widget("textview1")
            self.combobox1 = self.widgets.get_widget("combobox1")
            self.combobox2 = self.widgets.get_widget("combobox2")

            # Para el ComboBox1 se fija por defecto la primera opción de la lista
            self.combobox1.set_active(0)
            # y en el ComboBox2 se fija por defecto la segunda opción de la lista
            self.combobox2.set_active(1)

        # A continuación se definen/crean las ventanas especiales (about, dialogo,
        # etc) y las acciones a realizar en ellas.

        # Ventana genérica de error (se le pasan los mensajes de error y los
        # muestra en una ventana de dialogo)
        def error(self, message):
            "Display the error dialog "
            dialog_error = gtk.MessageDialog(parent=None, flags=0, buttons=gtk.BUTTONS_OK)
            dialog_error.set_title("Error")
            label = gtk.Label(message)
            dialog_error.vbox.pack_start(label, True, True, 0)
            # Con show_all() mostramos el contenido del cuadro de dialogo (en este
            # caso solo tiene la etiqueta) si no se hace el dialogo aparece vacío
            dialog_error.show_all()
            # El run y destroy hace que la ventana se cierre al apretar el botón
            dialog_error.run()
            dialog_error.destroy()

        # Ventana About (conocida como Acerca de).
        def about_info(self, data=None):
            "Display the About dialog "
            about = gtk.AboutDialog()
            about.set_name(Info.name)
            about.set_version(Info.version)
            about.set_comments(Info.description)
            about.set_copyright(Info.copyright)

            def openHomePage(widget,url,url2): # Para abrir el sitio
                import webbrowser
                webbrowser.open_new(url)

            gtk.about_dialog_set_url_hook(openHomePage,Info.website)
            about.set_website(Info.website)
            about.set_authors(Info.authors)
            about.set_license(Info.license)
            about.set_wrap_license(True) # Adapta el texto a la ventana
            about.run()
            about.destroy()


        # Ahora declaramos las acciones a realizar (por menús, botones, etc.):

        # Definimos la ventana about (help > About)
        def on_about1_activate(self, widget):
            "Open the About windows"
            self.about_info()

        # Definimos las acciones a realizar al apretar el botón de convertir
        def on_button1_clicked(self, widget):
            "Convert button"
            # Se crea un buffer en donde se guardaran los resultados
            text_buffer = gtk.TextBuffer()
            # Se obtiene el valor para convertir desde la entrada
            valor = self.entry1.get_text()
            # Obtiene la opción escogida en los 2 ComboBoxs
            selec1 = valor_combobox(self.combobox1)
            selec2 = valor_combobox(self.combobox2)
            try:
                # Intenta transformar el valor ingresado en un numero. En caso
                # de fallar (por ejemplo falla si lo ingresado son letras) se
                # lanza la excepción, si es exitoso se continua con la conversión
                temp_ini = float(valor)

                # Inicia la conversión adecuada dependiendo de la opción
                # escogida en los 2 ComboBoxs (selec1 y selec2)
                if selec1 == "Celsius" and selec2 == "Fahrenheit":
                    text_buffer.set_text(str(celsius2fahrenheit(temp_ini)))
                elif selec1 == "Celsius" and selec2 == "Kelvin":
                    text_buffer.set_text(str(celsius2kelvin(temp_ini)))
                elif selec1 == "Fahrenheit" and selec2 == "Celsius":
                    text_buffer.set_text(str(fahrenheit2celsius(temp_ini)))
                elif selec1 == "Fahrenheit" and selec2 == "Kelvin":
                    # Pasamos primero de F a Celsius, luego de Celsius a Kelvin
                    conversion1 = fahrenheit2celsius(temp_ini)
                    text_buffer.set_text(str(celsius2kelvin(conversion1)))
                elif selec1 == "Kelvin" and selec2 == "Celsius":
                    text_buffer.set_text(str(kelvin2celsius(temp_ini)))
                elif selec1 == "Kelvin" and selec2 == "Fahrenheit":
                     # Pasamos primero de Kelvin a Celsius, luego de Celsius a F
                     conversion1 = kelvin2celsius(temp_ini)
                     text_buffer.set_text(str(celsius2fahrenheit(conversion1)))
                else:
                    # Se produce cuando las dos selecciones son iguales
                    self.error("The initial and target units are the same")

                # Luego se fija (muestra) el buffer (que contiene la temperatura
                # convertida) en textview1 (el cuadro la lado del botón)
                self.textview1.set_buffer(text_buffer)

            except:
                if (len(valor) == 0):
                    # Se produce si no se ingresa nada en la entry1
                    self.error("Please enter a value")
                else:
                    # Se produce si no se ingresa un numero ( por ejemplo se
                    # ingresan letras o símbolos)
                    self.error("The value entered is not valid.\nEnter a \
    valid number")


    if __name__== "__main__":
        MainGui()
        gtk.main()

Y así se ve muestro pequeño programa funcionando:

.. image:: https://pythonmania.files.wordpress.com/2009/02/pytemp_004.png
    :width: 304px
    :height: 192px
    :alt: captura pytemp

Pueden descargarse el código completo `desde
aquí <http://sites.google.com/site/dbfuentes/archivos/pytemp-0.1.zip?attredirects=0&d=1>`_
(o desde `esta
copia <http://www.mediafire.com/?0znzomat20up>`_ o `esta otra
copia <http://launchpad.net/pytemp/0.1/0.1/+download/pytemp-0.1.zip>`_).

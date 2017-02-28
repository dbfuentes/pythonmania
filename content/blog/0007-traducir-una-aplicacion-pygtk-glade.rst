+++
title = "Traducir una aplicacion pygtk-Glade"
slug = "traducir-una-aplicacion-pygtk-glade"
date = "2009-03-15T21:53:00-04:00"
categories = ["tutoriales"]
tags = ["python2", "gettext", "glade", "pyGtk"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

En este tercer tutorial de pyGtk/Glade, voy a explicar como traducir (o
internacionalizar) el texto de los mensajes, etiquetas, botones, etc. de
tal manera que se muestren en el idioma predeterminado del sistema. Para
ello voy a utilizar el ejemplo del segundo tutorial (que es el anterior
a este):

-  `Creando una aplicación con pygtk y
   glade <https://www.pythonmania.net/es/2009/02/16/creando-una-aplicacion-con-pygtk-y-glade/>`__

En donde al pequeño programa (pytemp) lo voy a traducir al español. Para ello
primero hay que marcar que cadenas se van a traducir dentro del código y en el
archivo glade, para después poder generar los archivos de la traducción

<!--more-->

Marcando las cadenas a traducir en el archivo glade
===================================================

Esto es realmente fácil de hacer, solo hay que seleccionar el objeto que
se quiere traducir (botones, menús, etiquetas, etc) e ir a la ventana de
propiedades, en la parte general, en donde se encuentra el texto de la
etiqueta, al lado hay un botón con unos puntos, al presionarlo aparece
la siguiente ventana en donde le podemos indicar si el texto es
traducible o no se tiene que traducir (por defecto glade asume que es
traducible).

.. image:: https://pythonmania.files.wordpress.com/2009/03/pytemp_traduccion_001.png
   :width: 432px
   :height: 468px
   :alt: traduccion pytemp

En nuestro ejemplo vamos a marcar que sean traducibles casi todos los
textos, a excepción de los dos ComboBox, ya que estos contienen nombres
de unidades que no es necesario traducir (y que además es conveniente no
traducirlos para evitar problemas con el código). Por lo tanto revisamos
que los demás sean traducibles.

Marcando las cadenas a traducir en el codigo
============================================

Ahora viene lo interesante, hay que importar el modulo de gettext e
indicarle cual es el dominio y ubicación de las traducciones (se hace de
una maneras similar a esta `explicación que realice hace
tiempo <https://www.pythonmania.net/es/2008/09/10/traducir-aplicaciones-en-python/>`__,
pero además hay que agregarle algunas cosas para glade) Por lo tanto lo
voy a explicar a continuación.

Primero agregamos este código a nuestro programa:

.. code-block:: python

    import gettext

    # Algunas cosas para gettext (para las traducciones)
    APP="pytemp"
    DIR="po"

    # Esto permite traducir los textos escritos en el .py (no en glade)
    gettext.textdomain(APP)
    gettext.bindtextdomain(APP, DIR)

    # Y las siguientes 2 lineas permiten traducir los textos del Glade
    gtk.glade.textdomain(APP)
    gtk.glade.bindtextdomain(APP, DIR)

    # Y con esto podemos marcar las cadenas a traducir de la forma _("cadena")
    _ = gettext.gettext

Con esto importamos el `modulo de gettext
<https://www.pythonmania.net/es/2008/09/10/traducir-aplicaciones-en-python/>`_ ,
y luego el indicamos el dominio del programa (el nombre de la traducción)
en este caso es "pytemp" lo cual se señala en la variable **"APP"** y luego el
directorio en donde se encuentra, que en este caso es **/po**, lo cual lo
señalamos en la variable **"DIR"**. De esta manera se va a buscar la traducción
en un directorio de la forma:

``po/lenguaje/LC_MESSAGES/pytemp.mo``

Después se usa el textdomain y el bindtextdomain dos veces, en la
primera vez se le indica la ubicación de la traducción de las cadenas
escritas en el .py (en el código) y en la segunda vez le indicamos la
ubicación de la traducción de las cadenas que se encuentran en el
archivo glade.

    Nota: Normalmente los archivos de traducción van en
    “/usr/share/locale”, pero mientras se esta desarrollando el programa
    es bastante mas cómodo tenerlos en un directorio local, como en este
    caso que se dejan en "po", además de esta manera si alguien baja el
    código y lo ejecuta, va ver inmediatamente la traducción, sin que
    tenga que estar moviendo archivos a otros directorios.

Luego en la ultima parte se hace una especie de asignación, para que de
esta manera en vez de estar marcando las cadenas haciendo por
ejemplo **gettext.gettext("Texto a traducir")** podemos marcarlas
simplemente haciendo **\_("Texto a traducir")** lo cual resulta mas fácil.

Por ultimo vamos marcando en el código las cadenas que queremos
traducir, por ejemplo:

.. code-block:: python

    except:
        if (len(valor) == 0):
            # Se produce si no se ingresa nada en la entry1
            self.error(_("Please enter a value"))

Lo cual hay que repetirlo con cada una de las cadenas que queremos
traducir (Nota: al final de esta pagina hay un enlace en donde se
encuentra el código terminado, así que pueden revisar cuales cadenas
marque para traducir). Cuando se hayan marcado todas las cadenas, hay
que generar los ficheros de las traducciones.

Generar la plantilla de traducción (.pot)
=========================================

Ahora que ya se han marcados todas las cadenas a traducir, hay que
extraer las cadenas de texto del archivo .glade, para ello usamos
intltool, que en debian se instala haciendo:

``# apt-get install intltool``

Luego que instalemos intltool, usamos intltool-extract para extraer las
cadenas de texto del glade (en el ejemplo “pytemp.glade”) y con ellas
generar un archivo .h (pytemp.h) que contiene estas cadenas:

``intltool-extract --type="gettext/glade" *.glade``

Luego hay que extraer las cadenas del archivo .py y combinarlas con el
archivo .h, para eso hacemos

``xgettext -k_ -kN_ -o messages.pot *.py *.h``

Esto nos genera una plantilla llamada messages.pot que se usa de base
para las traducciones a los demás idiomas.

Generar el archivo de traducción (\*.po) para un idioma en específico
=====================================================================

A partir de la plantilla .pot podemos generar varios ficheros de
traducción (.po) para los distintos idioma. En este ejemplo voy a crear
la traducción al español, o sea voy a crear el archivo "es.po", para
ello:

``msginit -i messages.pot -o es.po``

Ahora este archivo se puede editar a mano (como lo hice en `esta
explicación que realice hace
tiempo <https://www.pythonmania.net/es/2008/09/10/traducir-aplicaciones-en-python/>`__),
o con algún programa como kbabel, gtranslator, poedit, etc. (incluso se
puede usar Launchpad para traducir).

En este caso lo edite con poedit, tal como lo muestra la siguiente
captura:

.. image:: https://pythonmania.files.wordpress.com/2009/03/pytemp_traduccion_002.png?w=480&h=365
    :width: 480px
    :height: 365px
    :alt: traduccion pytemp

Cuando se termina de traducir el archivo, simplemente se guarda

Crear el archivo **.mo**
========================

Finalmente cuando se tiene el archivo .po traducido por completo, hay
que generar un archivo binario (\*.mo) para que pueda usarse la
traducción, para ello se hace:

``msgfmt es.po -o po/es/LC_MESSAGES/pytemp.mo``

De esa manera se genera (y guarda en el directorio adecuado) el archivo
de la traducción, así que la próxima vez que se inicie nuestro programa
lo veremos traducido.

    Nota: Hay que recordar la estructura del directorio siempre es de la
    forma: *directorio/idioma/LC\_MESSAGES/nombre-de-programa.mo* en
    nuestro ejemplo se usa el directorio *po* y el nombre del programa
    es *pytemp*

Si todo sale bien nuestro programa ahora se ve de la siguiente manera:

.. image:: https://pythonmania.files.wordpress.com/2009/03/pytemp_traduccion_003.png
    :width: 332px
    :height: 192px
    :alt: traduccion pytemp

Ahora como extras al código fuente del ejemplo le agregue los clásicos
archivos README, ChangeLog, COPYNG y un directorio "doc" con un par de
manpages (`hechas de esta
manera <http://logicerror.wordpress.com/2009/03/03/crear-una-manpage/>`_),
de esta forma se parece cada vez más a la `estructura típica de un
proyecto de
software <http://es.wikipedia.org/wiki/Distribuci%C3%B3n_de_software#Archivos_est.C3.A1ndar>`_

Finalmente lo que todos esperan, pueden `descargar el código fuente
desde
aquí <http://launchpad.net/pytemp/0.2/0.2/+download/pytemp-0.2.zip>`_ o
`desde
aquí <http://sites.google.com/site/dbfuentes/archivos/pytemp-0.2.zip?attredirects=0&d=1>`_.

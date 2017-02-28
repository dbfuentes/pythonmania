+++
title = "Empaquetar un script python para debian (con cdbs)"
slug = "empaquetar-un-script-python-para-debian"
date = "2009-04-25T20:05:00-04:00"
categories = ["tutoriales"]
tags = ["python2", "debian"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Otro de mis tutoriales, esta ves se trata de como crear un paquete
Debian a partir de algún script/programa escrito en python. Por que
escribo esto, es simple es que he visto `por allí formas no muy
correctas <http://humitos.wordpress.com/2007/09/18/crear-un-paquete-debian-deb-de-un-programa-en-python-py/>`_
de hacerlo (si crean el paquete pero si lo revisan con lintian o
similares, este tiene muchos errores).

Como es casi costumbre vamos a empaquetar `el ejemplo del conversor de
temperatura <https://www.pythonmania.net/es/2009/03/15/traducir-una-aplicacion-pygtk-glade/>`_.

<!--more-->

**Necesitamos:**

-  python
-  dh-make
-  build-essential
-  fakeroot
-  cdbs
-  Las dependencias propias del programa

Además tenemos que tener claro que el nombre de un paquete debian es de
la forma:

.. code-block:: python

    <nombre_del_paquete>_<numero_de_version>-<numero_de_revision_debian>.deb

A continuación los pasos a seguir:

Paso 1: crear un directorio de trabajo
======================================

Al crear un paquete .deb se generan una serie de archivos, así que para
mantener el orden les recomiendo crear un directorio nuevo en donde
vamos a trabajar, para así no tener tantos archivos sueltos:

``$ mkdir sandbox $ cd sandbox``

Paso 2: Obtener las fuentes
===========================

Aquí hay varias posibilidades, como obtener un típico .tgz, .zip, etc. o
descargarlo de un repositorio CVS, SVN, Git, hg, etc. en este caso
usamos el `archivo comprimido de pytemp 0.2 que
deje <http://launchpad.net/pytemp/0.2/0.2/+download/pytemp-0.2.tar.gz>`_
en el ejemplo anterior. Así que lo descargamos a nuestro directorio de
trabajo y luego lo descomprimimos de tal manera que nos quede un
directorio con un nombre similar a pytemp-0.2 (hay `normas precisas
sobre este
nombre <http://www.debian.org/doc/debian-policy/ch-controlfields.html#s-f-Package>`_).

En general deberíamos tener dos cosas un directorio de la forma
[nombre-del-proyecto]-[versión] y también tienen que existir un archivo
con las fuentes comprimidas (normalmente un .tgz o similar por
comodidad, que es el que bajamos) con el mismo contenido en el
directorio padre de ese (ver imagen).

.. image:: https://pythonmania.files.wordpress.com/2009/04/paquete_debian_001.png
    :width: 375px
    :height: 175px
    :alt: paquete debian

Ahora hay que entrar en el directorio pytemp-0.2, definir el nombre del
mantenedor (el que crea el paquete, o sea tú) como la variable de
entorno $DEBFULLNAME y luego ejecuta dh\_make con las opciones a
continuación:

.. code-block:: python

    $ cd pytemp-0.2
    $ export DEBFULLNAME="Tu nombre"
    $ dh_make -f ../pytemp-0.2.tar.gz -e tu@email.es -b -c gpl
    Maintainer name : Tu Nombre
    Email-Address : tu@email.es
    Date : Sat, 25 Apr 2009 16:09:04 -0400
    Package Name : pytemp
    Version : 0.2
    License : gpl
    Using dpatch : no
    Type of Package : cdbs
    Hit to confirm:

La opción -c indica el tipo de licencia a utilizar (las opciones con
GPL, LGPL, BSD entre otras). La dirección de correo del usuario que crea
el paquete se especifica con la opción -e y la opción -b es para que use
cdbs (que nos simplificar la vida al crear el paquete).

    Nota: Sólo hay que utilizar dh\_make la primera vez se crea el
    paquete y nunca más ejecutarlo.

Luego de ejecutarlo, dh\_make ha hecho lo siguiente:

-  Crear un archivo llamado pytemp\_0.2.orig.tar.gz que contiene las
   fuentes, este archivo es el software tal como lo distribuye el autor
   (solo que esta empaquetado en un tar.gz) y formará parte del paquete
   Debian de fuentes.
-  Un directorio llamado debian (dentro del directorio pytemp-0.2) y
   este tiene con un montón de ficheros con la información del paquete,
   los cuales voy a explicar a continuación

Paso 3: modificar archivos del directorio debian
================================================

Hay una serie de ficheros en este directorio, algunos son obligatorios y
otros opcionales. Además varios de ellos son \*.ex estos son plantillas
para hacer cosas especificas (como por ejemplo para crear tareas en
cron, manpages, etc.). No vamos a usar todos así que al final vamos a
borrar los que no necesitemos.

Lo primero que hay es un **changelog**, este tiene los cambios que ha
sufrido el paquete debian (o sea no es el changelog del programa),
simplemente editarlo (puedes seguir el ejemplo de algún paquete debian
para guiarte). Este tienen que estar sí o sí.

    Ya que editar el changelog es una tarea común, existe un utilitario
    que ayuda en esto (dch), asi por ejemplo al hacer dch -e edita el
    changelog y con dch -i agrega un nuevo item en el debian/changelog
    (útil cuando uno aumenta el numero de versión del paquete).

El archivo **copyright** es plantilla de la licencia que hayas elegido
(la que le indicamos a dh\_make), simplemente rellena los datos que
faltan. También va sí o sí.

El archivo **dirs** contiene la ruta de los directorios que el paquete
usara/necesitará para instalar el programa. En nuestro caso es así:

.. code-block:: bash

    usr/bin
    usr/share
    usr/share/applications
    usr/share/man
    usr/share/pytemp

Esto especifica que directorios hay que crear para nuestro programa. Si
se preguntan por que estos directorios, es simple en usr/bin van los
ejecutables, o sea los programas como tal (si queremos que solo el root
lo pueda ejecutar se deja en usr/sbin), luego en usr/share/applications
van unos archivos \*.desktop que permiten que los programas aparezcan el
los menus de gnome, en el menu k, etc. en usr/share van los archivos de
solo lectura (iconos, sonidos, imagenes, etc) que usan los programas,
cada programa tiene su propio directorio dentro de share (por eso le
indicamos el usr/share/pytemp) y en usr/share/man van las paginas del
manual (mas adelante se entra en detalles). Los contenidos de cada
directorio no me los estoy inventando, esta `especificado en el FSH de
debian <http://www.debian.org/doc/packaging-manuals/fhs/fhs-2.3.html>`_
(que explica en forma general que contiene cada directorio de todo el
sistema).

    Nota: En todo caso la mayor parte de\ `estos directorios ya existen
    en el
    sistema <http://www.debian.org/doc/manuals/maint-guide/ch-dother.es.html#s-dirs>`_
    (o se crean mas adelante) así que se pueden omitir (pero prefiero
    ponerlos por si acaso y para que quede mas claro el ejemplo). Además
    si este fichero no existe no provoca problemas, ya que distutils se
    encargaría de crear los directorios por si solo.

El que sigue es el archivo **docs** que tiene los nombres de los
ficheros de documentación (por defecto se incluirán todos los ficheros
existentes en los directorios de más alto nivel del código que se llamen
BUGS, README\*, TODO, etc. ), modificar si es necesario (lo mas probable
que este bien ya que solo hay un README en nuestro programa).

El **menu.ex** lo renombramos a **menu** y luego lo editamos (este
permite que el programa aparezca en el menú debian), las posibles
opciones que puede tener se pueden conocer consultando el manual
(haciendo man menufile). En nuestro caso lo dejamos como:

.. code-block:: python

    ?package(pytemp):needs="X11" section="Applications/Science/Mathematics"\
      title="pytemp" command="/usr/bin/pytemp"

Me pareció lógico que un conversor de unidades vaya en la parte de
ciencias/matemáticas, si alguien opina lo contrario lo puede cambiar.

Las **manpages** (Página man), en debian se supone que cada paquete debe
tener una página de manual (hay algunas excepciones como lo
metapaquetes, pero la mayoría tiene su manpage) para ello dh\_make crea
unas plantillas de manpages que llevan por nombre:

-  manpage.1.ex
-  manpage.sgml.ex
-  manpage.xml.ex

La primera está en formato nroff y las otras dos en DocBook. En nuestro
caso caso el programa ya trae unas paginas de manual (manpages) creadas
por lo tanto no necesitamos crearlas y podemos pasar la siguiente
fichero. (En caso de que el programa no tuviera un manpage, habría que
editar una de ellas y borrar las otras plantillas (o pueden crear una
siguiendo `este método sencillo que
explique <http://logicerror.wordpress.com/2009/03/03/crear-una-manpage/>`__
en mi otro blog).

Ya no necesitamos ningún otro \*.ex (que son archivos de configuración
especiales, como plantillas para crear tareas en cron por ejemplo) y que
para nuestro objetivo no se usan, por lo que podemos borrarlos haciendo
(dentro del directorio pytemp-0.2/debian):

``rm *.ex *.EX``

El siguiente archivo es el **README.debian** que contiene notas sobre
cualquier detalle extra o cambios entre el programa original y su
versión debianizada. En nuestro caso no hay ninguno importante así que
se puede borrar.

Ahora solo nos quedan los dos archivos mas importantes el debianón
puedes ver como quedaría para nuestro ejemplo, una vez hechos los
cambios necesarios:

.. code-block:: python

    Source: pytemp
    Section: utils
    Priority: optional
    Maintainer: Tu Nombre <tu@email.es>
    Build-Depends: cdbs (>= 0.4.49), debhelper (>= 7), python-central (>= 0.5.6), python-all-dev (>= 2.3.5-11), gettext, docbook-xsl, xsltproc
    XS-Python-Version: current, >= 2.4
    Standards-Version: 3.7.3
    Homepage: https://pythonmania.wordpress.com/tutoriales/

    Package: pytemp
    Architecture: all
    Depends: ${python:Depends}, python-gtk2 (>=2.8), python-glade2
    XB-Python-Version: ${python:Versions}
    Description: a Python temperature converter
     pytemp is a temperature converter written in Python using
     PyGTK, Glade and it's licensed under the GPL

Si se dan cuenta hay dos partes definidas/separadas (una general con la
información de las fuentes y otra con información para los paquetes
binarios).

En la primera tenemos:
----------------------

Lo primero que se indica es Source que corresponde al nombre del paquete
fuente.

Luego viene la sección a la que pertenece, en este caso lo deje en utils
(aunque como es un programa gráfico también pudo ser x11), en los
`manuales de debian se mencionan los posibles
valores <http://www.debian.org/doc/debian-policy/ch-archive.html#s-subsections>`_

Los posibles valores para la prioridad lo `pueden encontrar en la
politica de
debian <http://www.debian.org/doc/debian-policy/ch-archive.html#s-priorities>`_.
En nuestro caso va a ser opcional (lo cual es lo común para los
paquetes, ya que extra es para los -dev, -dbg packages y quizás para
unos pocos más y prioridades sobre opcional son solo para unos pocos
paquetes).

El mantenedor es quien crea el paquete, dh\_make rellena automáticamente
la información (con los datos del paso 2) así que eso no lo modificamos.

En Build-Depends (y Build-Depends-Indep) se señalan las dependencias que
hay que instalar para construir el paquete, estas no son necesariamente
las mismas que para instalarlo, ya que por ejemplo se utiliza
docbook-xsl para crear las paginas man, pero no es necesario después
para instalar el paquete creado (ya que las paginas man ya va a estar
listas para ser usadas).

    En nuestro caso docbook-xsl y xsltproc son usados para crear la
    manpage, gettext para crear los archivos \*.mo (con msgfmt) y el
    resto (cdbs, debhelper, python-central, python-all-dev) son usados
    por la combinación de cdbs+distutils (`pueden encontrar más
    información
    aquí <http://python-modules.alioth.debian.org/python-central_howto.txt>`_).

    Nota: si alguien quiere usar python-support en vez de
    python-central, en la parte de las dependencias cambie
    python-central por python-support.

Nótese que solo pusimos Build-Depends y ninguna Build-Depends-Indep.

    Nota: Build-Depends-Indep contiene una lista de las dependencias
    adicionales que requiere dpkg-buildpackage asi que intenten no
    usarlo, para evitar problemas.

XS-Python-Version indica en que versiones de python funciona nuestro
programa. Este numero tiene que ser según la `política de debian para
python <http://www.debian.org/doc/packaging-manuals/python-policy/ch-module_packages.html#s-specifying_versions>`_.

En `Standar
version <http://www.debian.org/doc/debian-policy/ch-controlfields.html#s-f-Standards-Version>`_
va la versión de la política que se sigue, normalmente es rellenada por
dh\_make y es recomendable cambiarla en caso de que hayan cambios
importantes en la política que puedan afectar a tu paquete haciéndolo
incompatible. Pueden saber la politica que sigue el sistema con:

``apt-cache show debian-policy | grep Version``

    Nota: Pueden encontrar el texto la política instalando el paquete
    debian-policy o visitando el `sitio web de la política de
    debian <http://www.debian.org/doc/debian-policy/index.html#contents>`_.

Y la homepage es el sitio del programa.

Ahora en la segunda parte (la información de los paquetes binarios) tenemos:
----------------------------------------------------------------------------

Package que es el nombre del paquete, no hacemos ningún cambio.

Architecture que es la arquitectura del paquete binario (o si son
fuentes), los posibles valores están señalados en `la politica de
debian <http://www.debian.org/doc/debian-policy/ch-controlfields.html#s-f-Architecture>`__
(por ejemplo puede ser i386, amd64, etc.). En nuestro caso como se trata
de un script de python, este puede ser ejecutado en cualquier
arquitectura que tenga el interprete, por lo que el valor es "all".

Depends, que se trata de las dependencias para instalar el paquete, una
forma fácil de averiguarlas es haciendo:

``grep -B 3 -A 3 import ../pytemp.py``

Esto mostrara las partes del script pytemp.py en donde se han hecho
import, asi que solo hay que buscar si esos son módulos estándar o son
de alguna fuente extra.

    En nuestro caso webbrowser y gettext (para ser mas exactos
    pygettext) son parte de los modulos estandar, pero pygtk, gtk y
    gtk.glade son proporcionadas por los paquetes python-gtk2 y
    python-glade2.

Además de las dependencias se pueden agregar nuevas lineas que contengan
Recommends y Suggests. Las recomendaciones son dependencias suaves (o
sea si no se instalan el programa siguen funcionado, aunque sin algunas
características especificas) y sugerencias son exactamente eso, o sea
sugerencias que hace el creador del paquete (aunque estas no afectan al
programa). Hay que `ver la seccion 7 de la politica para mas
detalles <http://www.debian.org/doc/debian-policy/ch-relationships.html#s-binarydeps>`_.

    Nota: para señalar conflictos con otros paquetes se usa la linea
    "Conflicts" y cuando este paquete reemplaza a otro se usa "Replaces".

Luego en XB-Python-Version se indica nuevamente la versión de python,
asi que siempre se pone XB-Python-Version: \${python:Versions}.

Por ultimo en la Description va la descripción del paquete, primero una
corta de 60 caracteres y luego una larga indentada con espacios.

El fichero debian/rules
-----------------------

Este es mas bien un makefile que indica cómo hacer todas las tareas
relacionadas con el paquete: compilación, instalación, limpieza, etc.
Normalmente este fichero fichero tiene un tamaño y complejidad
considerable (incluso para un paquete pequeño), pero como nosotros vamos
a usar CDBS + distutils, vamos a obtener casi mágicamente uno muchísimo
más sencillo y pequeño. Así quedaría en este caso:

.. code-block:: python

    #!/usr/bin/make -f

    DEB_PYTHON_SYSTEM=pycentral

    include /usr/share/cdbs/1/rules/debhelper.mk
    include /usr/share/cdbs/1/class/python-distutils.mk

    # Add here any variable or target overrides you need.
    DBK2MAN_XSL = /usr/share/xml/docbook/stylesheet/nwalsh/manpages/docbook.xsl
    PO = es

    build/pytemp::

    # creamos el manpage
        xsltproc --nonet $(DBK2MAN_XSL) $(CURDIR)/doc/manpage_en.xml

    # creamos directorios para las traduciones
        for lang in $(PO); \
            do mkdir -p $(CURDIR)/debian/pytemp/usr/share/locale/$$lang/LC_MESSAGES; \
        done
    # luego creamos los .mo (traducciones) y los movemos a los directorios correspondientes
        for lang in $(PO); \
            do msgfmt $(CURDIR)/po/$$lang.po -o $(CURDIR)/debian/pytemp/usr/share/locale/$$lang/LC_MESSAGES/pytemp.mo; \
        done

    install/pytemp::
        mv $(CURDIR)/debian/pytemp/usr/bin/pytemp.py $(CURDIR)/debian/pytemp/usr/bin/pytemp

    clean::
        -rm $(CURDIR)/pytemp.1

Si se fijan las primeras 6 lineas van si o si en todo debian/rules e
indican de esta forma que se use cdbs + distutils.

    Nota: si alguien va a usar python-support, hay que cambiar la
    tercera linea por
    ``DEB_PYTHON_SYSTEM = pysupport``

A partir de la linea 8 en adelante vienen las cosas especificas para
cada paquete. En este caso lo dividí en tres partes, un build (que
construye cosas como las traducciones o la manpages), un install (que
instala el script) y un clean (que limpia los directorios después de
crear el paquete).

    Nota: $(CURDIR) representa el directorio en donde se esta
    trabajando, o sea es ../sandbox/pytemp-0.2

Si se fijan en build, con xsltproc creo un manpage a partir de la
plantilla que traía el programa (use solo la en ingles para no complicar
las cosas) y luego se crean las traducciones, en este caso hay un truco,
si en PO se agrega mas de un idioma, (por ejemplo PO = es br jp pl )
como lo que sigue esta en un bucle crearía todas las traducciones con
esas mismas lineas, solo modificando la variable PO.

En install, el mv es debido a que la política de Debian no permite que
haya programas con extensión en los directorios bin, que es donde va a
ir a parar el script (solo se admite archivos sin extensiones y enlaces
simbólicos).

    **Importante**: Sino quieres tener problemas en el futuro lee esto,
    la sintaxis de make (y por herencia de un debian/rules) nos obliga a
    respetar los separadores.¿qué son separadores?, simplemente es la
    forma en que están tabulados los archivos.

    Esto es una regla fija para los archivos make; sino se tabulan, lo
    más seguro es que en la ejecución del make (o debian/rules)
    apareciera un error similar a este:

    ``"makefile:2: *** falta un separador. Alto"``

    También te puede dar errores si mezclas tabuladores con espacios,
    así que cuidado al escribir el debian/rules

Paso 4: Crear el fichero setup.py
=================================

En este ejemplo estamos utilizando cdbs + distutils, que simplifica
mucho las cosas (`distutils es un
sistema <http://docs.python.org/lib/module-distutils.html>`_ para
construir e instalar módulos Python). Por este motivo el debian/rules
era tan sencillo, ya que este delega gran parte de la tarea de la
instalación en distutils. Para usarlo simplemente hay que escribir un
fichero setup.py muy sencillo y colocarlo en el directorio pytemp-0.2/:

.. code-block:: python

    #!/usr/bin/env python
    # -*- coding: UTF-8 -*-

    from distutils.core import setup

    setup(name         = 'pytemp',
          version      = '0.2',
          description  = 'A temperature converter written in python using PyGTK',
          author       = 'Nombre autor',
          author_email = '<email@autor.com>',
          url          = 'https://pythonmania.wordpress.com/tutoriales/',
          license      = 'GPL v2 or later',
          scripts      = ['pytemp.py'],
          data_files   = [('share/pytemp',['pytemp.glade']), ('share/man/man1',['pytemp.1']), ('share/applications',['pytemp.desktop'])],
          )

Paso 5: Un vistazo a las fuentes del programa
=============================================

Mientras estamos desarrollando el programa y deseamos invocar un módulo,
cargar una imagen, etc., utilizamos rutas relativas, o sea, NO
especificamos la ruta completa en donde se encuentra el recurso a
importar, sino que le indicamos algún lugar dentro del directorio de
nuestro código fuente.

Esto supone un problema al momento de instalar el programa en el
sistema, ya que por ejemplo, el .glade se va a guardar en
/usr/share/pytemp/. Por lo tanto tenemos que poner dicha ruta
directamente en la aplicación.

Así que abrimos el script que contiene nuestro programa
(../pytemp-0.2/pytemp.py) y buscamos las lineas que hay que modificar y
las cambiamos por la siguientes:

.. code-block:: python

    # Algunas cosas para gettext (para las traducciones)
    APP="pytemp"
    DIR="/usr/share/locale"

Y para el archivo glade:

.. code-block:: python

    def __init__(self):
        # Le indicamos al programa que archivo XML de glade usar
        self.widgets = gtk.glade.XML("/usr/share/pytemp/pytemp.glade")

Y listo ahora va a abrir estas cosas usando la ruta que tendrán después
de ser instaladas.

Paso 6: El archivo .desktop
===========================

Un archivo .desktop es algo así como un "acceso directo". Se trata de un
archivo de texto que contiene información de nuestro programa, su
descripción, comentarios (que pueden ser en varios idiomas), si se
ejecuta en un terminal o no, define un icono, etc. Este es el crea
entradas en los paneles y en el menu de gnome, menu K, etc.

Mi archivo pytemp.desktop es el siguiente (lo guarde en el directorio
../pytemp-0.2/):

.. code-block:: python

    [Desktop Entry]
    Encoding=UTF-8
    Version=1.0
    Type=Application
    Name=pytemp
    Comment=A temperature converter written in python using PyGTK
    Comment[es]=Un conversor de temperatura escrito en python, usando PyGTK
    GenericName=pytemp
    Terminal=false
    Exec=pytemp
    Categories=GNOME;GTK;Application;Utility;

Paso 7: Generar el paquete "binario"
====================================

Ahora en el en el directorio ../pytemp-0.2/ hacemos lo siguiente:

``$ dpkg-buildpackage -rfakeroot``

Si la compilación del paquete ha funcionado sin problemas, te debería
preguntar la contraseña de tu clave gpg para firmar el paquete y luego
te habrán aparecido varios ficheros en el directorio ../pytemp-0.2/.

    Nota: si tienes varias claves gpg en el equipo, le puedes indicar
    cual usar haciendo (en el ejemplo le indico que use mi clave, la
    AA8B5425):

    ``$ dpkg-buildpackage -sgpg -kAA8B5425 -rfakeroot``

    Nota 2: antes de construir el paquete hay que tener instaladas las
    dependencias que se necesitan para construirlo (las que señalamos en
    la linea Build-Depends del debian/control).

Después que el dpkg-buildpackage termine por lo menos deberías tener los
siguientes archivos en el directorio ../pytemp-0.2/:

-  pytemp-0.2-1.all.deb – El paquete Debian binario (listo para instalar
   en cualquier sistema)
-  pytemp-0.2.orig.tar.gz – El código fuente tal como lo distribuye el
   autor del programa (upstream)
-  pytemp-0.2-1.diff.gz – Todos los cambios respecto al archivo
   orig.tar.gz (incluye todos los archivos que hemos creado y/o
   modificado)
-  pytemp-0.2-1.dsc – Resumen del contenido del paquete, con parte de la
   información del debian/control y con las sumas MD5 y SHA1 para el
   orig.tar.gz y el diff.gz
-  pytemp-0.2-1\_\*\*\*.changes – Los cambios que has hecho en esta
   versión ademas tiene las sumas MD5 y SHA1 de todos los demás archivos
   mencionado

Paso 8: Verificación del paquete
================================

Ahora hay que verificar el paquete, hay que comprobar que:

a) Que tiene los ficheros necesarios y ninguno más (con mc o lesspipe)

``lesspipe *.deb``

b) Que cumple la policy de Debian (con lintian)

``lintian *.deb``

Si les tira algún error o warning, pueden verificar cuan es su causa en
el sitio: `http://lintian.debian.org <http://lintian.debian.org/tags.html>`_

c) Que instala correctamente (con dpkg -i o gdebi).

Paso 9: Limpieza
================

Limpiamos los directorios borrando todos los archivos innecesarios,
haciendo

``pytemp-0.2$ fakeroot debian/rules clean``

es bueno limpiar, ya que mas adelante podríamos modificar el paquete por
algún imprevisto, así que es bueno dejar los directorios en buenas
condiciones

Listo, ya terminamos
--------------------

Finalmente el paquete queda así:

Captura de gdebi con el paquete:

.. image:: https://pythonmania.files.wordpress.com/2009/04/paquete_debian_002.gif
    :width: 450px
    :height: 359px
    :alt: paquete debian (gdebi)

Y como ya aparece la entrada del programa en el menú correcto (no tiene
icono porque no se lo definí):

.. image:: https://pythonmania.files.wordpress.com/2009/04/paquete_debian_003.png
    :width: 475px
    :height: 335px
    :alt: menu paquete debian

Pueden revisar el directorio completo que use para debianizar este
paquete en `el siguiente repositorio de
launchpad <http://bazaar.launchpad.net/~dbfuentes/pytemp/sandbox/files>`_
o pueden descargarse `este archivo con el directorio completo
comprimido <http://www.divshare.com/download/7209592-2c3>`_.

    Nota: si alguien se preogunta como modificar el paquete con una
    nueva versión del "upstream" o para cerrar un bug, `aqui lo
    explican <http://www.debian.org/doc/manuals/maint-guide/ch-update.es.html>`_

Lecturas recomendadas:

-  `Debian New Maintainers’
   Guide <http://www.debian.org/doc/manuals/maint-guide/index.en.html>`_
   (`y la versión en
   esp <http://www.debian.org/doc/manuals/maint-guide/index.es.html>`_)
-  `Debian Policy Manual <http://www.debian.org/doc/debian-policy/>`_
-  `CDBS
   Documentation <https://perso.duckcorp.org/duck/cdbs-doc/cdbs-doc.xhtml>`_
-  `Manuales del DDP para
   desarrolladores <http://www.debian.org/doc/devel-manuals.es.html>`_
-  `Creación de paquetes
   Debian <http://acacha.dyndns.org/mediawiki/index.php/Creaci%C3%B3n_de_paquetes_Debian>`_
   (explica de forma general muchas características de los paquetes)
-  `Debian Python
   Policy <http://www.debian.org/doc/packaging-manuals/python-policy/>`_
-  `Python Modules Packaging
   Team <http://python-modules.alioth.debian.org/>`_ (ojo con el texto
   de python-central)
-  `Debian packaging with CDBS <http://debathena.mit.edu/packaging/>`_
   (explica cada una de las opciones comunes)
-  `Packaging with the new Python policy: A package developers
   view <http://people.debian.org/~srivasta/manoj-policy/x513.html>`_
   (explica el asunto de XB-Python-Version)
-  `PackagingGuide/Python <https://wiki.ubuntu.com/PackagingGuide/Python>`_
   (explica como usar CDBS+pycentral/pysupport).
-  `Filesystem Hierarchy
   Standard <http://www.debian.org/doc/packaging-manuals/fhs/fhs-2.3.html>`_
   (para entender la jerarquia de los directorios en una maquina con
   debian)
-  `DebianPythonFAQ <http://wiki.debian.org/DebianPythonFAQ>`_
   (preguntas frecuentes del empaquetado de programas python)

    En caso de que quieras actualzar en un futuro este paquete, te
    podria interesar leer esto: `actualizar paquetes
    debian <https://www.pythonmania.net/es/2010/03/04/actualizar-paquetes-debian/>`_

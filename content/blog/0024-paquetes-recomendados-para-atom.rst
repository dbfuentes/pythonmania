+++
title = "Paquetes recomendados para GitHub Atom"
slug = "paquetes-recomendados-para-atom"
date = "2017-02-27T22:07:08-03:00"
lastmod = "2019-05-01T17:09:00-04:00"
categories = ["general", "herramientas"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

He estado usando el editor `de GitHub Atom <https://atom.io>`_ en los últimos
meses. No me gusta completamente (hay algunas cosas que extraño de viejo vim:
la rapidez de inicio, la madurez de algunos componentes y poder iniciarlo en
una terminal) pero ahora estoy usando Atom debido a que en la forma que esta
construido (sobre `Electron <http://electron.atom.io/>`_) permite tener una
experiencia de uso bastante única.

**Información General**

- Url: `https://atom.io/ <https://atom.io/>`_
- Desarollador: GitHub
- Plataformas: mac OSX (10.9 o superior), Windows (7  o superior) y GNU/Linux
- Licencia: MIT Licence (software libre)

Por defecto Atom trae incluido algunas extensiones/paquetes por defecto. Pero
si van a utilizarlo para desarrollar, probablemente van a necesitar otros
paquetes y obviamente la comunidad de Atom ya ha creado bastantes de ellos.

En esta entrada he escogido algunos paquetes más útiles y/o "interesantes"
que he encontrado. Y con interesantes,en este caso me refiero a paquetes que
solo se podrían crear en un editor flexible como Atom o alguno similar.

<!--more-->

Configuración de Atom
=====================

Primero ha que abrir la pestaña de las preferencias (Ve a menú
``Edit`` > ``Preferences`` > ``Settings``).

Usar espacios en ves de tabuladores
-----------------------------------

Baja por la pestaña de preferencias hasta encontrar la opción “Soft Tabs” y
asegurate de que esta este marcada. Esta opción convierte automáticamente
los tabuladores a espacios en el momento de ser tipeados.

Fijar la indentación en 4 espacios
----------------------------------

Para fijar la cantidad de espacios por tabulación a 4 espacios (el indentado
tipico de python) y mostrar los caracteres invisibles (como espacios, tabs,
saltos de linea, etc.). Ve a: menú ``Edit`` > ``Preferences`` > ``Settings`` y
allí se puede fijar la cantidad de espacios por tabulación (Tab Length) a 4 y se
pude marcar la opción de ver los caracteres invisibles ("Show Invisibles").

Otros
-----

El siguiente paso es esconder los archivos **\*.pyc** y cualquier otro
archivo incluido en  **.gitignore** (en caso de que estemos trabajando en un
directorio controlado por git), para ello ve a: ``settings`` > ``packages``,
busca ``tree-view``, ve a “package settings” y marca la opción
'Hide VCS Ignored Files'.


Al inicio fijamos las tabulaciones en 4 espacios, pero para mi gusto, al
escribir en HTML prefiero una indentación de solo 2 espacios (debido a que el
HTML tiende a tener varios niveles de indentado  y cuacquer valor mayor a 2
espacios tiende a empujar parte HTML pasado el borde de las 80 columas de las
pantallas tipo terminales). Por lo tanto para el caso del HTML vamos a fijar
un espaciado diferente para el indentado (tab size). Para ello hay que abrir
el archivo de configuración (ve a menú ``Edit > config``) y agrega al final:

.. code-block:: coffee-script

    ".basic.html.text":
      editor:
        tabLength: 2

Ahora vamos a instalar algunos paquetes y temas extras (para ello vamos a
``Edit > Preferences > Install``).

    Nota: Se pueden buscar los paquetes/temas de Atom directamente en
    su `web online <https://atom.io/packages/>`_ o desde las mismas
    preferencias de atom (``Edit > Preferences``)

UI y Temas
==========

El uso de temas es siempre subjetivo y normalmente evito recomendar uno. Sin
embargo, Seti es un tema que hace que Atom luzca atractivo y además agrega
iconos para una gran variedad de archivos como por ejemplo CSS, LESS, JSON,
configuraciones de grunt y más.

- `Seti UI <https://atom.io/themes/seti-ui>`_

Paquetes Generales
==================

Autocomplete-plus
-----------------

Autocomplete-plus comenzó  como un paquete hecho por la comunidad pero ahora
es parte de los paquetes que componen el editor. Solo tipea algunas cosas y
autocomplete-plus automaticamente te va dar algunas sugerencias.

Autocomplete-plus muestra sus opciones en un contexto visual, permitiendo
el mostrar el tipo de opción de autocompletar, una breve descripción y
potencialmente una descripción en profundidad. Así, los usuarios pueden
distinguir que lo ofrecido es un fragmento de código, una función, una
palabra clave o importación.

- `Autocomplete-plus <https://github.com/atom/autocomplete-plus/>`_

Highlight-selected
------------------

Cuando seleccionas una función o una variable en Sublime Text o Notepad++,
todas las otras repeticiones de esa función o variable se destacan en el
documento. Highlight Selected realiza esa funcionalidad en Atom.

- `highlight-selected <https://atom.io/packages/highlight-selected>`_

Pigments
--------

Pigments es un muy buen paquete para mostrar el color de un algún código en
hex, gba, rgba, hsl, hsla. Quizás ya hayas visto antes algún previsualizador de
colores hexadecimal, pero pocos llegan al nivel de Pigments. Analiza los
colores, entiende las variables de preprocesado y Incluso ejecuta funciones
de cambio de color.

Además, puede visualizar la paleta utilizada en el proyecto a través de
Pigmentos. Tu puedes mirar la paleta completa e ir rápidamente ir a la
definición de cualquier color.

- `Pigments <https://atom.io/packages/pigments>`_

Color Picker
------------

Usualmente si deseas utilizar un selector de color, lo más probable es que
abras Photoshop o GIMP y utilice el selector de color que tienen incorporado.
Este paquete le permite seleccionar colores en el editor de Atom y es tan
fácil como hacer clic con el botón derecho del ratón y elegir "Color Picker".
Alternativamente, puede hacerse presionando `` CMD / CTRL + SHIFT + C``.

El selector de color lee actualmente los colores HEX, HEXa, RGB, RGBa, HSL,
HSLa, HSV, HSVa, VEC3 y VEC4.

- `Color Picker <https://atom.io/packages/color-picker>`_

Minimap
-------

`Minimap <https://atom.io/packages/minimap>`_ es uno de los paquetes más
populares para Atom, este paquete muestra una vista previa en miniatura del
archivo, para poder navegar rápidamente por el codigo. Se puede establecer
su posicion a la izquierda o derecha, activar o desactivar el destacado del
palabras claves (de highlight-selected) y más. Incluso Minimap tiene plugins
extras para extender aum ,ás su funcionalidad, como el mostrar los colores
de los códigos en la miniatura del minimap.

- `Atom minimap <https://atom.io/packages/minimap>`_

- `minimap-highlight-selected:
  <https://atom.io/packages/minimap-highlight-selected>`_ Highlight-selected
  Palabras claves (o resultados de busqueda) aparecen en el minimap.

- `minimap-pigments: <https://atom.io/packages/minimap-pigments>`_ Muestra los
  colores de pigments en el minimap.

Atom Beautify
-------------

Beautify transforma tu código desordenado (o Minificado/minify) en algo más
organizado y más legible. Soporta varios lenguajes de programación como HTML,
CSS, JavaScript, PHP, Python, Ruby, Java, C, C ++, C #, Objective-C,
CoffeeScript, typescript, etc.

Después de instalado, para ejecutarlo, simplemente haz un click con el boton
derecho y escoge ``Beautify editor contents``, o alternativamente ve
a ``Packages`` > ``Atom Beautify`` > ``Beautify``.

- `Atom Beautify <https://atom.io/packages/atom-beautify>`_

Linter
------

Atom Linter es la base para usar los distintos paquetes de linter para los
diversos lenguajes de programación (un linter es un programa/script que busca
errores en el código), en otras palabras provee la API para los distintos
linters dentro de Atom. Luego de intalar el paquete "base" necesitas
instalar los linter específicos para el lenguaje que vas a usar.

- `Linter <https://atom.io/packages/linter>`_

Atom Alignment
--------------

Seleccionas las variables que quieres ordenar o alinear, y luego
presiona ``CTRL + ALT + A``. Entonces algo como esto:

.. code-block:: coffee-script

    var a = b;
    var anotherVariable = 12;
    var awesomeModule = require('awesome-module');
    var that = this;

se transforma en esto:

.. code-block:: coffee-script

    var a               = b;
    var anotherVariable = 12;
    var awesomeModule   = require('awesome-module');
    var that            = this;

- `Atom Alignment <https://atom.io/packages/atom-alignment>`_

paquetes para Desarrollo Web
============================

Emmet
-----

Emmet (antes conocido como Zen Coding) es un plugin disponible para varios
editores de texto populares (incluyendo Sublime Text, Visual Studio, Eclipse,
Atom, etc.) este plugin te permite escribir código valido HTML sin tener que
escribir las etiquetas completas de HTML, sino usando las abreviaciones de
Emmet. Por ejemplo, puedes escribir la siguiente linea en tu editor:

.. code-block:: html

    div#content>ul#nav>li*4>a

Y tocar la tecla de "Expand Abbreviation" de Emmet (por defecto la tecla
tab/tabulación). Entonces la abreviación se transforma mágicamente en HTML
valido:

.. code-block:: html

    <div id="content">
      <ul id="nav">
        <li><a href=""></a></li>
        <li><a href=""></a></li>
        <li><a href=""></a></li>
        <li><a href=""></a></li>
      </ul>
    </div>

- `Emmet <https://atom.io/packages/emmet>`_.

Autoclose-html o Less-Than-Slash
--------------------------------

Cuando escribes HTML, Atom no completa (cierra) tus estiquetas (tags). Por
ejemplo, al escribir ``<div>`` uno espera que el correspondiente ``</div>`` sea
agregado automáticamente, pero esto no ocurre en Atom por defecto. Cualquiera
de estos paquetes (Autoclose-html o less-than-slash) agregan esta funcionalidad
a Atom.io y personalmente los encuentro muy útiles ya que hacen que escribir
HTML sea mas rapido.

- `Autoclose-html: <https://atom.io/packages/autoclose-html>`_. Cierra la
  etiqueta abierta cuando se escribe el  ``>``.

- **Alternativa** `less-than-slash: <https://atom.io/packages/less-than-slash>`_
  Cierra la etiqueta abierta cuando se escribe ``</``.

Uglify
------

Este paquete es el opuesto de atom-beautify, esta diseñado para
Minificar/minify los archivos de JavaScript (reducir los archivos eliminando
espacios, tabulaciones, comentarios, etc).

- `Atom-uglify <https://atom.io/packages/uglify>`_

- **Alternative** `Atom-minify: <https://atom.io/packages/atom-minify>`_.
  Minifica/Minifies archivos JS y CSS.

linter-csslint
--------------

Este linter reporta errores que encuentra en los archivos CSS abiertos en Atom.

- `linter-csslint <https://atom.io/packages/linter-csslint>`_ (Require Linter)

less/sass-autocompile
---------------------
Automáticamente compila so archivos LESS/SASS al guardarlos o  via un atajo
del teclado.

- `less-autocompile <https://atom.io/packages/less-autocompile>`_

- `sass-autocompile <https://atom.io/packages/sass-autocompile>`_

Python
======

script
------

¡Ejecuta codigo/scripts en Atom!, en base a la extensión del archivo, a una
seleccion de codigo, o por el numero de linea. Soporta Python, Ruby,
Ruby on Rails, Perl, php, java, C/C++, Haskell, Shell Script y un gran etc.

- `Script <https://atom.io/packages/script>`_

linter flake8 y pydocstyle
--------------------------

A continuación vamos a instalar un linter de Python, para que nos ayude a
detectar errores en nuestro código escrito en python. Hay varios, pero yo
recomiendo uno llamado linter-flake8 que usa por debajo el conocido
flake8 (que tiene que estar ya instalado en el equipo).

- `linter-flake8 <https://atom.io/packages/linter-flake8>`_

Si instalaste el linter-flake8, ya tienes una validación automática contra
el PEP8, pero falta otro paquete que es necesario para validar las cadenas
de documentación (docstrings) de acuerdo a la semántica del PEP 257. Esto se
resuelve instalando el linter-pydocstyle que puede ser usado lado a lado con
flake8 .

- `linter-pydocstyle <https://atom.io/packages/linter-pydocstyle>`_

Bonos Extras
============

- `Expose <https://atom.io/packages/expose>`_ Es una herramienta de manejo de
  archivos, modelada en base al expose de Mac OSX's.  Co el puedes mostrar al
  instante todos los archivos abiertos como pequeñas capturas de pantalla y
  puedes cambiarte entre ellos usando el teclado.

- `Asteroids <https://atom.io/packages/asteroids>`_  Crea un juego de Asteroids
  (el shooter) en cualquier texto abierto y te permite hacer explotar tu codigo.

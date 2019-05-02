+++
title = "Actualizar paquetes debian"
slug = "actualizar-paquetes-debian"
date = "2010-03-04T21:47:00-04:00"
categories = ["tutoriales"]
tags = ["debian"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

En un post anterior explicaba como crear un paquete .deb para un script de
`python <https://pythonmania.wordpress.com/2009/04/25/empaquetar-un-script-python-para-debian/>`_
, ahora voy a explicar como actualizar un paquete cualquiera.

Tenemos dos casos:

#. Hay que realizar algún cambio pequeño, por ejemplo corregir algún bug que
   tenga el paquete (nueva revisión del paquete) o aplicar algún parche. Este
   cambio lo realizas tú, no el desarrollador del programa (o sea no es una
   nueva versión del programa, sino que son cambios específicos del paquete).

#. El desarrollador ha lanzado una nueva versión del programa, por lo que
   tenemos nuevas fuentes (y necesitamos crear un paquete nuevo para esta
   versión).

<!--more-->

Primer caso: nueva revisión del paquete
=======================================

En este caso no ha cambiado el programa fuente, sino que se crea una
nueva revisión en donde se corrigen bugs o se agrega algún parche al
paquete.

Supongamos que teníamos ya creado el paquete para foo-0.7.2
(foo\_0.7.2-1\_all.deb y el directorio en donde creamos el paquete es:
~/sandbox/foo-0.7.2) y ahora se encontró un bug por lo que tenemos que
crear una nueva revisión del paquete (foo\_0.7.2-2\_all.deb)

Los pasos son:

-  entrar al directorio en donde creamos el paquete deb (por ejemplo si
   el directorio debianizado estaba en ~/sandbox/foo-0.7.2, por lo que
   hay que entrar en el)

-  En el terminal escribe:

``export DEBFULLNAME="tu nombre"``
``export DEBEMAIL="tu@email.com"``

-  Añadir una nueva revisión en el fichero de cambios
   (debian/changelog), la manera mas fácil de hacerlo es con:

``dch -i``

-  Abrir el debian/changelog y en la nueva entrada (que creamos con dch)
   hay que incluir una breve descripción del error y su solución en la
   entrada.

    Si agregas un *Closes: #12345* al final de los cambios y estas
    subiendo el paquete a un repositorio oficial (como el de debian o
    ubuntu) el informe del error numero 12345 será automágicamente
    cerrado en el sistema de seguimiento de bugs de la distribución

-  Modifica el código fuente o los archivos que sean necesarios para
   corregir el bug (y si se necesario aplica algún parche)

-  Revisa el resto de los archivos, básicamente tienes que revisar (y
   hacer en caso de que sea necesario) `los pasos desde el numero 3 al 6
   del tutorial
   anterior <https://www.pythonmania.net/es/2009/04/25/empaquetar-un-script-python-para-debian/>`_
   para que se pueda crear correctamente el paquete.

-  Una vez que ya se hayan hechos los cambios necesario, hay que crear
   el paquete haciendo:

``dpkg-buildpackage -rfakeroot``

-  Ahora hay que verificar el paquete, hay que comprobar que:

#. Que tiene los ficheros necesarios y ninguno más (lesspipe
   Nombre\_paquete.deb)
#. Que cumple la policy de Debian, con lintian(lintian
   Nombre\_paquete.deb)
#. Que instala correctamente (con dpkg -i o gdebi)

-  Limpiamos los directorios borrando todos los archivos innecesarios,
   haciendo:

``fakeroot debian/rules clean``

Listo ya tienes tu nuevo paquete foo\_0.7.2-2\_all.deb

    Nota: es importante limpiar, ya que mas adelante podríamos volver a
    modificar el paquete (o crear uno nuevo) y para ello el directorio
    tiene que quedar limpio

Segundo caso: nueva versión del programa
========================================

Supongamos que teníamos ya creado el paquete para foo-0.8.5, y ahora ha
salido la versión foo-0.9.1, tenemos que hacer lo siguiente:

-  Ingresar al directorio padre en donde teníamos el paquete de la
   versión anterior (por ejemplo si el directorio debianizado estaba en
   ~/sandbox/foo-0.8.5 hay que ingresar a ~/sandbox/)

-  Descarga las nuevas fuentes (en el ejemplo foo-0.9.1.tar.gz) y
   verifica los cambios en el código fuente para saber que es lo nuevo,
   o sea lee changelog, NEWS, README y cualquier otra documentación.
   Además ve las diferencias con la versión anterior (busca cualquier
   cosa que pudiera parecer sospechosa o cambios puedan generar algún
   problema)

-  Ahora a las nuevas fuentes hay renombrarlas al formato adecuado para
   las fuentes, o sea nombre\_version.orig.tar.gz (por ejemplo si el
   archivo fuente es foo-0.9.1.tar.gz, tiene que quedar como
   foo\_0.9.1.orig.tar.gz) y pon el archivo ese archivo un directorio
   por encima del antiguo árbol de fuentes (por ejemplo tiene que quedar
   en ~/sandbox/foo\_0.9.1.orig.tar.gz)

-  En el terminal escribe:

``export DEBFULLNAME="tu nombre"``
``export DEBEMAIL="tu@email.com"``

-  Entra en el antiguo directorio (en este caso ~/sandbox/foo-0.8.5) y
   ejecuta:

``uupdate -u ../foo_0.9.1.orig.tar.gz``

-  Ahora tendrás un nuevo directorio (~/sandbox/foo-0.9.1) en donde
   estarán las nuevas fuentes, a las cuales automagicamente se le
   agregaron los archivos del directorio debian (que fueron creados a
   partir de la versión anterior).

-  Entra en ~/sandbox/foo-0.9.1 y modifica el *debian/changelog* para
   completar los cambios (*uupdate* ya creo una entrada en el
   *debian/changelog* sólo tienes que completar la información en caso
   que falte algo)

-  Revisa el resto de los archivos, básicamente tienes que revisar (y hacer
   en caso de que sea necesario) `los pasos desde el numero 3 al 6 del tutorial
   anterior <https://pythonmania.wordpress.com/2009/04/25/empaquetar-un-script-python-para-debian/>`__
   para que se pueda crear correctamente el paquete. Probablemente los
   archivos ya estén creados y solo tienes que revisar si necesitan
   algún cambio o si se necesita algún extra

-  Una vez que ya se hayan hechos los cambios necesario, hay que crear
   el paquete haciendo:

``dpkg-buildpackage -rfakeroot``

-  Ahora hay que verificar el paquete, hay que comprobar que:

#. Que tiene los ficheros necesarios y ninguno más (lesspipe
   Nombre\_paquete.deb)
#. Que cumple la policy de Debian, con lintian(lintian
   Nombre\_paquete.deb)
#. Que instala correctamente (con dpkg -i o gdebi)

-  Limpiamos los directorios borrando todos los archivos innecesarios,
   haciendo:

``fakeroot debian/rules clean``

    Es importante limpiar, ya que mas adelante podríamos volver a
    modificar el paquete (o crear uno nuevo), así que es bueno dejar los
    directorios en buenas condiciones

-  Listo ya tienes tu nuevo paquete

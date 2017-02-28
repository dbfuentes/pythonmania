+++
title = "Creando notificaciones con dbus y python"
slug = "notificaciones-con-dbus-y-python"
date = "2008-09-08T19:00:00-04:00"
categories = ["ejemplos"]
tags = ["python2", "dbus"]
author = "Daniel Fuentes"
draft = false
lang = "es"
+++

Esto es algo que había publicado en mi antiguo blog (ya desaparecido),
asi que lo vuelvo a poner en caso de que a alguien le interese.

Se trata de que se puede usar dbus para mostrar una notificación en
nuestro escritorio (por lo menos en gnome, donde lo probé), como la
que muestro en la siguiente captura:

.. image:: https://pythonmania.files.wordpress.com/2008/09/notificacion_dbus.png
   :width: 350px
   :height: 100px
   :alt: notificacion dbus

El código es el siguiente (es bastante simple y con los comentarios que
tiene queda explicado por si solo).

<!--more-->

.. code-block:: python

    #!/usr/bin/env python

    import dbus

    # Inicializando el bus de tipo session (se comunica entre aplicaciones)
    bus = dbus.SessionBus()

    # Se obtiene el objeto Notifications (es el encargado de las notificaciones)
    notify_object = bus.get_object('org.freedesktop.Notifications','/org/freedesktop/Notifications')

    # Se obtiene una interface del tipo Notificatios
    notify_interface = dbus.Interface(notify_object,'org.freedesktop.Notifications')

    # Finalmente lanzamos la notificacion
    # Nota: el gtk-ok muestra un icono de ok en el mensaje, puede ser cualquier otro
    # como gtk-cut (muestra una tijera), gtk-connect (una conexion), etc.
    notify_id = notify_interface.Notify("DBus Test", 0, "gtk-ok", "Hola a todos",'Un ejemplo de mensaje o texto a mostrar', "",{},10000)

    # Imprimimos el ID de esta notificacion
    print "El ID de la notificacion es: ", notify_id

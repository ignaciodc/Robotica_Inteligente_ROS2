## Ejemplo completo: publicador, suscriptor, acción y servicio con Python

En esta sección se va a mostrar un ejemplo completo de funcionamiento de una arquitectura ROS2. A continuación se muestra el esquema gráfico de dicha arquitectura, en la que se va a basar el código que se mostrará debajo.
<p align="center">
<img width="747" height="319" alt="image" src="https://github.com/user-attachments/assets/29bcef97-fdfc-4b77-ad90-be8bb545b9be" />
</p>


## Árbol de directorios
En todo proyecto software es importantísimo tener clara la distribución de los archivos que lo componen para que las llamadas y demás conexiones se realicen de forma correcta. En un proyecto ROS2 también es muy importante esta circunstancia. A continuación se muestra el árbol de directorio del ejemplo que se propone.

robot_comunicacion/
    package.xml
    setup.py
    setup.cfg
    resource/
      robot_comunicacion

    robot_comunicacion/
      __init__.py
      publisher_node.py
      subscriber_node.py
      service_server.py
      service_client.py
      action_server.py
      action_client.py
      
    srv/
      MiServicio.srv
   action/
      MiTarea.action
   launch/
      comunicacion_launch.py
    test/
      test_comunicacion.py



[← Volver atrás](Readme.md)

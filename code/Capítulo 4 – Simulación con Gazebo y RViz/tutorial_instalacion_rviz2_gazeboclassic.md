## Tutorial de Instalación de RViz 2 y Gazebo Classic 

Las versiones que se van a proponer, Rviz 2 y Gazebo Classic, que pertenece a la SEcción 4.2 del libro, son las que mejor encajan con las versiones de sistema operativo y ROS 2 instaladas previamente. Para empezar con las instalaciones de las dos herramientas debe seguir los siguientes pasos:

**1.	Preparación del entorno ROS**

  - Ejecute ```source /opt/ros/humble/setup.bash ```
  
**2.	Actualización del sistema**

  - Ejecute ```sudo apt update```
  
**3.	Instalación de RViz 2**

  - Ejecute ```sudo apt install ros-humble-rviz2```
  
**4.	Verificación de la instalación**

  - Se ejecuta la herramienta, si llega a abrir la misma es que está bien instalada. Ejecute ```rviz2```
  
**5.	Instalación de Gazebo Classic (Gazebo 11) con integración ROS**

  - Ejecute ```sudo apt install ros-humble-gazebo-ros-pkgs```
      - Este paquete instala: Gazebo versión 11, _plugins gazebo_ros_, soporte para actuadores y sensors simulados, publicación de tiempo simulado ``` /clock```
    
**6.	Verificación de Gazebo Classic**

  - Se ejecuta la herramienta, si llega a abrir la misma es que está bien instalada. Ejecute gazebo
  - Arranque de Gazebo desde ROS 2 con ```ros2 launch gazebo_ros gazebo.launch.py```
      - Esto garantiza: Gazebo use el tiempo simulado, _plugins_ ROS activos, integración con ROS 2 completa
    
**7.	Comprobación de la integración ROS–Gazebo**
  - Con Gazebo en ejecución, abre otra terminal y ejecute:``` ros2 topic list```. Si aparece el tópico ```/clock``` la integración entre Gazebo y ROS 2 es correcta.


[← Volver atrás](Readme.md)

## COLCON (COmplex Launch CONsole) 

Colcon es una herramienta de construcción de paquetes de software orientada a proyectos modulares y multipaquete, como los que utiliza el entorno ROS 2. 
Su principal objetivo es proporcionar una forma eficiente, extensible y reproducible de compilar, probar e instalar múltiples paquetes escritos en distintos lenguajes (como C++, Python, etc.) dentro de un único espacio de trabajo.

Siguiendo los pasos que nos han llevado hasta aquí de forma ordenada, ahora se va a proceder a la instalación de Colcon. Esta instalación se debe realizar teniendo activado el entorno virtual creado anteriormente. Para ello, deben introducirse los siguientes comandos:

```
sudo apt install python3-colcon-common-extensions
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws
colcon build
source install/setup.bash
```


[← Volver atrás](Readme.md)

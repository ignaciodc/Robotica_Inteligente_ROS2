## _slam_toolbox_ en ROS 2
En ROS 2, una de las soluciones más utilizadas y recomendadas para SLAM 2D es slam_toolbox, un paquete desarrollado específicamente para el ecosistema ROS 2. Este paquete implementa algoritmos de SLAM basados en grafos, ofreciendo un equilibrio adecuado entre precisión, robustez y facilidad de uso. Este código de ejemplo se encuentra en la Sección 5.3.1.4 del libro.

Antes de lanzar _slam_toolbox_, se asume que el sistema dispone de:
* Un robot móvil simulado en Gazebo.
* Un sensor LIDAR 2D publicando mensajes sensor_msgs/LaserScan (por ejemplo, en ```/scan```).
* Transformaciones TF correctamente configuradas (_odom_ a _base_link_).
* ROS 2 Humble operativo.


Si SLAM no está instalado previamente, puede instalarse mediante:
```sudo apt install ros-humble-slam-toolbox```
Este paquete incluye nodos, configuraciones por defecto y archivos de lanzamiento compatibles con ROS 2. El modo más habitual de uso es el SLAM online, en el que el mapa se construye en tiempo real mientras el robot se mueve.
Desde una terminal, ejecuta:
```ros2 launch slam_toolbox online_async_launch.py```
Este comando:
* Lanza el nodo slam_toolbox.
* Se suscribe automáticamente al tópico ```/scan```.
* Publica el mapa en ```/map```.
* Emite las transformaciones necesarias para la navegación.
* ```online_async_launch.py``` se encuentra dentro del paquete slam_toolbox
El modo _async_ permite separar el procesamiento del SLAM del ciclo principal de ROS, mejorando la eficiencia.

En proyectos reales y en entornos docentes es habitual crear un archivo de lanzamiento propio que integre SLAM, simulación y visualización. Un ejemplo simplificado en Python sería:
```
from launch import LaunchDescription
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        Node(
            package='slam_toolbox',
            executable='async_slam_toolbox_node',
            name='slam_toolbox',
            output='screen',
            parameters=[{
        'use_sim_time': True
            }]
        )
    ])
```
Este archivo permite:
* Activar el uso de tiempo simulado (```/clock```).
* Integrar slam_toolbox en un launch más amplio que incluya Gazebo y Nav2.





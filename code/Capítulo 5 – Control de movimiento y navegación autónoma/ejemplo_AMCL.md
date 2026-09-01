## Ejemplo práctico: Localización del robot mediante AMCL en ROS 2

El código de este ejemplo se encuentra en la Sección 5.3.2.8 del libro. En este ejemplo se muestra cómo un robot móvil, disponiendo de un mapa previamente generado, es capaz de localizarse de forma autónoma utilizando el algoritmo AMCL. Este escenario representa el caso típico de operación real de un robot de servicio o de navegación autónoma en un entorno conocido. Se asume el siguiente contexto:
* Robot móvil diferencial.
* Sensor LIDAR 2D publicando en el tópico /scan.
* Odometría disponible en /odom.
* Mapa del entorno previamente creado mediante SLAM (mapa_entorno.yaml).
* Simulación en Gazebo Classic y visualización en RViz 2.
* ROS 2 Humble operativo.
  
En primer lugar, debe lanzar el entorno de simulación con el robot:
```ros2 launch mi_robot gazebo.launch.py```

Este lanzamiento debe garantizar que:
* El robot publica ```/odom```.
* El LIDAR publica ```/scan```.
* Las transformaciones TF (_odom_ a _base_link_) están activas

```gazebo.launch.py``` generalmente contiene las siguientes partes:
* Rutas a paquetes
* Rutas a mundo y URDF
* Lanzar Gazebo
* Publicar el modelo del robot
* Insertar el robot en Gazebo 



Un posible código de ejemplo de ```gazebo.launch.py``` podría ser:
```
from launch import LaunchDescription
from launch.actions import IncludeLaunchDescription
from launch.launch_description_sources import PythonLaunchDescriptionSource
from launch_ros.actions import Node
from ament_index_python.packages import get_package_share_directory
import os

def generate_launch_description():
    # Obtener rutas a paquetes
    gazebo_ros_pkg = get_package_share_directory('gazebo_ros')
    robot_pkg = get_package_share_directory('mi_robot')

    # Rutas a mundo y URDF
    world_file = os.path.join(robot_pkg, 'worlds', 'empty.world')
    urdf_file = os.path.join(robot_pkg, 'urdf', 'robot.urdf')
    return LaunchDescription([
        # 1. Lanzar Gazebo
        IncludeLaunchDescription(
            PythonLaunchDescriptionSource(
                os.path.join(gazebo_ros_pkg, 'launch', 'gazebo.launch.py')
            ),
            launch_arguments={
                'world': world_file
            }.items(),
        ),
        # 2. Publicar el modelo del robot
        Node(
            package='robot_state_publisher',
            executable='robot_state_publisher',
            output='screen',
            parameters=[{
                'robot_description': open(urdf_file).read(),
                'use_sim_time': True
            }]
        ),
        # 3. Insertar el robot en Gazebo
        Node(
            package='gazebo_ros',
            executable='spawn_entity.py',
            arguments=[
                '-entity', 'mi_robot',
                '-topic', 'robot_description'
            ],
            output='screen')
    ])
```

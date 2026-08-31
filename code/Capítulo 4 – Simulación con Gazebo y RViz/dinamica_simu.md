## Dinámica simulada — Resultado físico
Este código que representa la última fase dentro de la simulación de un robot, se encuentra en la Sección 4.5.5
```
from launch import LaunchDescription
from launch_ros.actions import Node
from launch.actions import ExecuteProcess

def generate_launch_description():

    robot_description = open('/home/usuario/robot.urdf').read()

    return LaunchDescription([

        Node(
            package='robot_state_publisher',
            executable='robot_state_publisher',
            parameters=[{'robot_description': robot_description}]
        ),

        Node(
            package='controller_manager',
            executable='ros2_control_node',
            parameters=[
                {'robot_description': robot_description},
                '/home/usuario/diff_drive_controller.yaml'
            ]
        ),

        ExecuteProcess(
            cmd=['ign', 'gazebo', '-r', 'empty.sdf'],
            output='screen'
        )
    ])
```
[← Volver atrás](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%204%20%E2%80%93%20Simulaci%C3%B3n%20con%20Gazebo%20y%20RViz/Ejemplo_completo_robot_simulado.md)

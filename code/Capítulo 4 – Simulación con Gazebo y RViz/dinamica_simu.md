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
4.5.5

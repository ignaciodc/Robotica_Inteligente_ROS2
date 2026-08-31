## Control y uso de sensores desde Python. Ejemplo: sensor LIDAR 2D simulado
Desde el punto de vista del desarrollador, un sensor simulado se utiliza exactamente igual que uno real. Por ejemplo, para leer datos de un LIDAR en Python. Este ejemplo se puede encontrar en la Sección 4.4.2.2.:

```
import rclpy
from rclpy.node import Node
from sensor_msgs.msg import LaserScan

class LidarNode(Node):
    def __init__(self):
        super().__init__('lidar_node')
        self.create_subscription(
            LaserScan,
            '/scan',
            self.callback,
            10
        )

    def callback(self, msg):
        self.get_logger().info(f"Distancia frontal: {msg.ranges[len(msg.ranges)//2]}")
```


[← Volver atrás]([/Readme.md)](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%204%20%E2%80%93%20Simulaci%C3%B3n%20con%20Gazebo%20y%20RViz/ejemplo_2d.md)

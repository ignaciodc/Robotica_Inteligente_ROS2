## Control y uso de sensores desde Python
Desde el punto de vista del desarrollador, un sensor simulado se utiliza exactamente igual que uno real. Por ejemplo, para leer datos de un LIDAR en Python:
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


## Cliente rclpy 

```rclpy``` es la librería cliente de ROS 2 para Python. Proporciona clases y funciones para:
* Crear nodos
* Publicar y suscribirse a tópicos
* Usar servicios, acciones, temporizadores, parámetros, etc.

A continuación, se muestra un ejemplo extenso de uso de un publicador completo en ROS 2, haciendo uso de rclpy:

```
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class MiNodo(Node):
    def __init__(self):
        super().__init__('mi_nodo')
        self.publisher_ = self.create_publisher(String, 'saludos', 10)
        self.timer = self.create_timer(1.0, self.timer_callback)

    def timer_callback(self):
        msg = String()
        msg.data = 'Hola desde ROS 2 en Python'
        self.publisher_.publish(msg)
        self.get_logger().info(f'Publicando: {msg.data}')

rclpy.init()
nodo = MiNodo()
rclpy.spin(nodo)
nodo.destroy_node()
rclpy.shutdown()
```
   

[← Volver atrás](Readme.md)

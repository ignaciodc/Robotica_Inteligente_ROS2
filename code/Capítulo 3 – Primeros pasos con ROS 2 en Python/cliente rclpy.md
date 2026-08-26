## Cliente rclpy 

rclpy es la librería cliente de ROS 2 para Python. Proporciona clases y funciones para:
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
### Explicación del código
 El objetivo del código anterior es publicar un mensaje tipo String cada 1 segundo en el tópico llamado /saludos, con el texto: «Hola desde ROS 2 en Python». Veamos qué hace con detenimiento:
 
* En primer lugar, se importan las librerías necesarias para poder ejecutar el código de forma correcta.
* Se define una clase nueva, MiNodo, que hereda de Node.
* Se crea una función constructor con el argumento self.
* Se inicializa el nodo con el nombre «mi_nodo».
* Se crea un publicador, Publisher, que será el que publique mensajes de tipo String en el tópico /saludos, con una cola de mensajes de tamaño 10.
* Seguidamente, se crea un temporizador que ejecuta la función ```timer_callback()``` cada 1 segundo.
* A continuación se define la función ```timer_callback()```, la cual crea un mensaje String, le asigna el texto «Hola desde ROS 2 en Python», lo publica al tópico /saludos, e imprime en consola el mensaje publicado (como log de información).
* Por último, aparece la parte principal del programa:
  * ```rclpy.init()```: inicializa el sistema ROS 2.
  * ```nodo = MiNodo()```: crea una instancia del nodo.
  * ```rclpy.spin(nodo)```: entra en un bucle que mantiene vivo al nodo y permite que funcione el temporizador.
  * Cuando se detiene el nodo (por ejemplo, con Ctrl+C):
    * ```nodo.destroy_node()```: limpia recursos.
    * ```rclpy.shutdown()```: cierra el sistema ROS 2.
   

[← Volver atrás](../Readme.md)

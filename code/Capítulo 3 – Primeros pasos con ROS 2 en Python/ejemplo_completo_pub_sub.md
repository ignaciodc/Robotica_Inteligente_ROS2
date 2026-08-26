## Ejemplo completo: publicador, suscriptor, acción y servicio con Python

En esta sección se va a mostrar un ejemplo completo de funcionamiento de una arquitectura ROS2. A continuación se muestra el esquema gráfico de dicha arquitectura, en la que se va a basar el código que se mostrará debajo.
<p align="center">
<img width="747" height="319" alt="image" src="https://github.com/user-attachments/assets/29bcef97-fdfc-4b77-ad90-be8bb545b9be" />
</p>


## Árbol de directorios
En todo proyecto software es importantísimo tener clara la distribución de los archivos que lo componen para que las llamadas y demás conexiones se realicen de forma correcta. En un proyecto ROS2 también es muy importante esta circunstancia. A continuación se muestra el árbol de directorio del ejemplo que se propone.
```
robot_comunicacion/
    package.xml
    setup.py
    setup.cfg
    resource/
      robot_comunicacion

    robot_comunicacion/
      __init__.py
      publisher_node.py
      subscriber_node.py
      service_server.py
      service_client.py
      action_server.py
      action_client.py
      
    srv/
      MiServicio.srv
   action/
      MiTarea.action
   launch/
      comunicacion_launch.py
    test/
      test_comunicacion.py
```

Seguidamente se va a mostrar y explicar el código de cada uno de los archivos que se van a emplear.


## Publicador
```
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class PublisherNode(Node):
    def __init__(self):
        super().__init__('publisher_node')
        self.publisher_ = self.create_publisher(String, 'robot', 10)
        self.timer = self.create_timer(1.0, self.timer_callback)
        self.count = 0
    def timer_callback(self):
        msg = String()
        msg.data = f'Mensaje numero {self.count}'
        self.publisher_.publish(msg)
        self.get_logger().info(f'Publicado: "{msg.data}"')
        self.count += 1
def main(args=None):
    rclpy.init(args=args)
    node = PublisherNode()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
if __name__ == '__main__':
    main()
```
### Explicación del código
Se ha tomado un publicador similar al que se ha empleado cuando se ha explicado el **[Cliente rclpy](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%203%20%E2%80%93%20Primeros%20pasos%20con%20ROS%202%20en%20Python/cliente%20rclpy.md)**
Las acciones del _main_ suelen ser comunes en casi todos los elementos, a menos que se compliquen algo con hilos o excepciones. Así, el _main_ tiene la siguiente secuencia:
* Inicializa ROS 2.
* Crea el nodo _PublisherNode_.
* Entra en un bucle (_loop_) que mantiene el nodo activo (_rclpy.spin()_).
* Al cerrar el nodo, se destruye y ROS se apaga.


[← Volver atrás](Readme.md)

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
**_publisher_node.py_**
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

## Suscriptor
**_subscriber_node.py_**

```
import rclpy
from rclpy.node import Node
from std_msgs.msg import String

class SubscriberNode(Node):
    def __init__(self):
        super().__init__('subscriber_node')
        self.subscription = self.create_subscription(
            String, 'robot', self.listener_callback, 10)
        self.subscription  # evitar warning variable no usada
    def listener_callback(self, msg):
        self.get_logger().info(f'Recibido: "{msg.data}"')
def main(args=None):
    rclpy.init(args=args)
    node = SubscriberNode()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
if __name__ == '__main__':
    main()
```

## Servicio

Para ejecutar un servicio se necesitan dos componentes separados, el servidor y el cliente. ROS 2 usa un modelo de comunicación basada en servicios (request-response). Esto requiere: un servidor que expone una API tipo servicio, y un cliente que la consume.

### Servidor del servicio
Un Servicio puede tener un Servidor creado y que no haya clientes que soliciten información, pero no podrá funcionar nunca sin un servidor creado.

**_service_server.py_**
   ```
import rclpy
from rclpy.node import Node
from mi_robot_comunicacion.srv import MiServicio

class ServiceServer(Node):
    def __init__(self):
        super().__init__('service_server')
        self.srv = self.create_service(MiServicio, 'sumar_dos_numeros', self.handle_sumar)
    def handle_sumar(self, request, response):
        response.suma = request.a + request.b
        self.get_logger().info(f'Sumar {request.a} + {request.b} = {response.suma}')
        return response
def main(args=None):
    rclpy.init(args=args)
    node = ServiceServer()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
if __name__ == '__main__':
    main()
   ```

### Cliente del servicio
**_service_client.py_**
```
import sys
import rclpy
from rclpy.node import Node
from mi_robot_comunicacion.srv import MiServicio
class ServiceClient(Node):
    def __init__(self):
        super().__init__('service_client')
        self.client = self.create_client(MiServicio, 'sumar_dos_numeros')
        while not self.client.wait_for_service(timeout_sec=1.0):
            self.get_logger().info('Servicio no disponible, esperando...')
        self.request = MiServicio.Request()

    def send_request(self, a, b):
        self.request.a = a
        self.request.b = b
        future = self.client.call_async(self.request)
        future.add_done_callback(self.callback)

    def callback(self, future):
        try:
            response = future.result()
            self.get_logger().info(f'Resultado suma: {response.suma}')
        except Exception as e:
            self.get_logger().error(f'Error llamando servicio: {e}')
``` 


##	Acción
Al igual que los servicios, se requiere implementar un servidor de acciones (_Action Server_), y un cliente de acciones (_Action Client_). Sin embargo, a diferencia de los servicios, las acciones son útiles para tareas que toman tiempo y requieren retroalimentación periódica, con posibilidad de cancelación y un resultado final.

### Servidor de la acción
*_action_server.py_*

``` 
import rclpy
from rclpy.node import Node
from rclpy.action import ActionServer
from mi_robot_comunicacion.action import MiTarea

class ActionServerNode(Node):
    def __init__(self):
        super().__init__('action_server_node')
        self._action_server = ActionServer(
            self,
            MiTarea,
            'mi_tarea',
            execute_callback=self.execute_callback)

    def execute_callback(self, goal_handle):
        self.get_logger().info('Ejecutando accion...')
        goal_data = goal_handle.request.goal_data
        feedback_msg = MiTarea.Feedback()
        result = MiTarea.Result()


        for i in range(1, 11):
            if goal_handle.is_cancel_requested:
                goal_handle.canceled()
                self.get_logger().info('Accion cancelada')
                return MiTarea.Result()

            feedback_msg.porcentaje_avance = i * 10.0
            goal_handle.publish_feedback(feedback_msg)
            self.get_logger().info(f'Progreso: {feedback_msg.porcentaje_avance}%')
            self.sleep_some_time()

        result.resultado_final = f' completada con goal_data={goal_data}'
        goal_handle.succeed()
        return result

    def sleep_some_time(self):
        # Simula tiempo de procesamiento
        import time
        time.sleep(0.5)

def main(args=None):
    rclpy.init(args=args)
    action_server = ActionServerNode()
    rclpy.spin(action_server)
    action_server.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```

### Cliente de la acción
En esta ocasión, en el código que se ofrece a continuación, podrá observar el código propuesto de ejemplo para implementar el cliente de una acción en un proyecto de ROS 2.
     
**_action_client.py_**
```
import rclpy
from rclpy.node import Node
from rclpy.action import ActionClient
from mi_robot_comunicacion.action import MiTarea
class ActionClientNode(Node):

    def __init__(self):
        super().__init__('action_client_node')
        self._action_client = ActionClient(self, MiTarea, 'mi_tarea')

    def send_goal(self, goal_data):
        self._action_client.wait_for_server()
        goal_msg = MiTarea.Goal()
        goal_msg.goal_data = goal_data
        self._send_goal_future = self._action_client.send_goal_async(
            goal_msg,
            feedback_callback=self.feedback_callback)     
       self._send_goal_future.add_done_callback(self.goal_response_callback) 
     
        
    def goal_response_callback(self, future):
        goal_handle = future.result()
        if not goal_handle.accepted:
            self.get_logger().info('Objetivo rechazado :(')
            return

        self.get_logger().info('Objetivo aceptado :)')
        self._get_result_future = goal_handle.get_result_async()
   self._get_result_future.add_done_callback(self.get_result_callback)

    def feedback_callback(self, feedback_msg):
        feedback = feedback_msg.feedback
        self.get_logger().info(f'Feedback recibido: {feedback.porcentaje_avance}%')

    def get_result_callback(self, future):
        result = future.result().result
        self.get_logger().info(f'Resultado final: {result.resultado_final}')

def main(args=None):
    rclpy.init(args=args)
    action_client = ActionClientNode()
    action_client.send_goal(42)
    rclpy.spin(action_client)
    action_client.destroy_node()
    rclpy.shutdown()

if __name__ == '__main__':
    main()
```


[← Volver atrás](Readme.md)

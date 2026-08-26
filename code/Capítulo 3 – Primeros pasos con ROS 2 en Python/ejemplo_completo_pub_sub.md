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

### Explicación del código
Se ha tomado un publicador similar al que se ha empleado cuando se ha explicado el **[Cliente rclpy](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%203%20%E2%80%93%20Primeros%20pasos%20con%20ROS%202%20en%20Python/cliente%20rclpy.md)**
Las acciones del _main_ suelen ser comunes en casi todos los elementos, a menos que se compliquen algo con hilos o excepciones. Así, el _main_ tiene la siguiente secuencia:
* Inicializa ROS 2.
* Crea el nodo _PublisherNode_.
* Entra en un bucle (_loop_) que mantiene el nodo activo (_rclpy.spin()_).
* Al cerrar el nodo, se destruye y ROS se apaga.

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

### Explicación del código
* Una vez definidas las librerías que se necesitan y se declaran las clases necesarias, como ya se ha visto anteriormente, se crea una suscripción incluyendo el tipo de dato que se va a recibir, el nombre del tópico (tiene que coincidir con el nombre del tópico que se escribió en el publicador), una función que se ejecutará cada vez que llegue un mensaje por el canal del tópico indicado ( ```self.listener_callback ```), y el tamaño de la cola de recepción de mensajes por si se reciben ráfagas de datos. 
* Seguidamente, se definirá la función que se ejecutará cada vez que se reciba un dato. En este caso se va a imprimir el mensaje recibido en la consola (_msg.data_) con un log tipo «info».
* Por último, la función _main_, en la última parte del código, al igual que en el publicado será común a la mayoría de los nodos suscriptores, con las mismas excepciones que se han comentado en el publicador. Las acciones que se realizarán en cualquier caso serán las siguientes:
    * Inicializa ROS 2.
    * Crea el nodo SubscriberNode.
    * Entra en un bucle (_loop_) que mantiene el nodo activo (_rclpy.spin()_).
    * Al cerrar el nodo, se destruye y ROS se apaga.

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

####Explicación del código

* En primer lugar, se van a importar las librerías necesarias para ejecutar este componente. Destacar en este sentido que se debe importar el archivo «.srv» del servicio que contiene la descripción del mismo, y que más adelante se abordará su contenido.
* Seguidamente, se declara una clase que herede de _Node_, como se ha realizado en los anteriores componentes- Se crea un nodo nuevo para el servicio servidor.
* A continuación, se crea un servicio de tipo «MiServicio», cuyo nombre es «sumar_dos_numeros».
* La función  ```self.handle_sumar``` se ejecutará cuando se reciba una solicitud del cliente. Esta función, definida a continuación, realiza una suma que se guarda en  ```response.suma ```, de los argumentos  ```request.a ``` y  ```request.b```, dos números recibidos (campos definidos en el archivo «.srv»). Para terminar la función se imprime un mensaje en el _log_ del nodo y se devuelve el response al cliente.
* Para terminar este script se declara la función principal de una forma análoga a lo que se mostró en los anteriores componentes: Inicializa la comunicación con ROS 2, se crea (en este caso) una instancia del nodo servidor, se mantiene el nodo activo esperando solicitudes de servicio, y se destruye el nodo y cierra ROS 2 limpiamente al detenerse la ejecución.

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

#### Explicación del código  
* Inicialmente se van a realizar una serie de importaciones de librerías, las mismas que se necesitaban en el servidor del servicio.
* Seguidamente, se define un nodo nuevo para el cliente y se crea el cliente de servicio del tipo «MiServicio», que se conectará al servicio llamado «sumar_dos_numeros». (debe coincidir con el nombre que se le haya dado al servicio en el servidor). 
* El bucle que sigue espera hasta que el servicio esté disponible. Si no está disponible, imprime un mensaje cada segundo.
* A continuación, se crea una solicitud vacía que luego se llenará con los valores que se deseen enviar al servidor.
* En las siguientes líneas se definen dos funciones:
    * La primera de ellas, ```send_request()```, establece los valores a y b de la solicitud, llama al servicio de forma asíncrona, y usa un _callback_ para manejar la respuesta cuando llegue.
    * La segunda función implementa el _callback_, la función que se ejecuta cuando se recibe la respuesta del servicio. Además, imprime el resultado de la suma (```response.suma```) o un error si hubo un problema.
* Para terminar, la función _main_ realiza las siguientes acciones:
    * Inicializa el entorno de ROS 2 y crea una instancia del cliente.
    * Comprueba que el usuario haya pasado exactamente dos argumentos (los números a sumar a en este ejemplo). Si no, muestra un mensaje que explique el uso del servicio.
    * Toma los números desde los argumentos y envía la solicitud al servicio y lo devuelve.
    * Si el servicio se ha empleado bien (se han enviado dos argumentos), continúa la ejecución, manteniendo el nodo activo para recibir la respuesta, y después de recibir la respuesta, destruye el nodo y cierra ROS 2 correctamente.


[← Volver atrás](Readme.md)

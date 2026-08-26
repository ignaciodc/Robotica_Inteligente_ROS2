## Nodos, tópicos, servicios y acciones

A continuación se definen y se muestra el código de los elementos fundamentales de ROS 2. Los ejemplos enunciados se refieren al siguiente esquema:

<p align="center">
  <img width="256" height="384" alt="Figura 1 2" src="https://github.com/user-attachments/assets/ccdd1ca1-1e48-417f-98f4-af8a2caf0b7c" /><br>

</p>



### Nodos
Un nodo es el componente más básico de una aplicación en ROS. Es un programa que realiza una función específica: por ejemplo, leer sensores, mover un motor o procesar imágenes. De forma general: 
* Son procesos individuales que pueden ejecutarse de forma independiente.
* En un sistema robótico real, puedes tener muchos nodos colaborando.
* Ejemplo: _camera_node_, _navigation_node_, _motor_controller_node_.
  
Siguiendo las directrices de la página oficial de ROS 2, en el siguiente código se propone la creación de un nodo:
```
from rclpy.node import Node

class MiNodo(Node):
    def __init__(self): 
        super().__init__('mi_nodo')
```

#### Explicación del código

* En primer lugar se importa la clase base _Node_ desde el módulo _rclpy.node_. 
* Seguidamente, se define una clase nueva, como se hace normalmente en lenguaje Python, llamada MiNodo, que hereda de Node, haciendo que MiNodo sea un nodo personalizado que puede tener sus propios publicadores, suscriptores, timers, servicios, etc. 
* Por último se llama al constructor de la clase base (_Node_) usando super(). Aquí «mi_nodo» es el nombre del nodo en ROS 2. Este nombre aparecerá cuando se usen comandos como ```ros2 node list```.


### Tópicos
Los tópicos (_topics_) son canales unidireccionales para mensajes asincrónicos. Permiten a los nodos comunicarse de manera asíncrona usando un modelo publicador/suscriptor (_publisher/subscriber_). Un nodo publica mensajes en un tópico, mientras otro/s nodo/s se suscribe/n para recibir esos mensajes sin enviar respuesta al nodo que publica. Se usa para flujos continuos de datos, como sensores, posición o imágenes. 

Como ejemplo se propone Un nodo de cámara publica en el ```topic/image_raw```, y otro nodo se suscribe para procesar la imagen.

### Servicios
Un servicio en ROS permite la comunicación sincrónica entre nodos (modelo solicitud-respuesta). En esta ocasión un nodo solicita una acción puntual, mientras otro nodo responde con un resultado.
Ideal para tareas cortas con inicio y fin claros, como «abre la pinza», «guarda el mapa», etc. Los servicios permiten una comunicación síncrona tipo petición-respuesta. 
Como ejemplo se propone Un nodo puede llamar a un servicio ```/reset_odometry``` para reiniciar el sistema de navegación. 


## Acciones
Las acciones permiten operaciones largas con retroalimentación. Son una extensión de los servicios, diseñados para tareas más largas o predecibles en el tiempo, que requieren seguimiento de progreso y posibilidad de cancelación. Se usa un protocolo especial: _goal_, _feedback_, _result_.

Permiten enviar una orden (por ejemplo, «vaya al punto B») y recibir actualizaciones periódicas (_feedback_) hasta que termine o se cancele.

[← Volver atrás](Readme.md)

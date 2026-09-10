# Capítulo 9 – Integración de LangChain con ROS2


<p align="center">
<img width="280" height="509" alt="image" src="https://github.com/user-attachments/assets/efaf0765-75f8-4202-845a-56dabf605ae0" />

<sub><b>Arquitectura de integración de LangChain en ROS 2</b></sub> </p>


##	Caso práctico: control de robot con lenguaje natural

### Escenario

Suponga un robot móvil básico (real o en simulación, p. ej., en Gazebo) con:
* Un _publisher_ a ```/cmd_vel``` para movimiento (velocidad lineal y angular).
* Un servicio de navegación para metas globales.
* Un nodo de control de manipulador que acepta comandos de posición.

El objetivo es que un operador pueda dictar comandos como:
* «Avanza hacia la puerta y para cuando detectes un obstáculo». 	
* «Navega hasta [1.0, 2.0] y luego gira 45 grados».
  
y que el sistema procese estas órdenes de lenguaje natural hasta acciones robóticas concretas. El diseño se va a basar en las tres capas:

#### 1. Diseño de un agente LangChain

 Su única responsabilidad es traducir lenguaje natural a una estructura JSON validable.

 Se va a mostrar el código por bloques:
* **Definición del esquema de acciones:** Antes de escribir código, se define un contrato semántico:

```
{
  "action": "move | rotate | navigate | stop",
  "params": { ... }
}
Ejemplos válidos:
{ "action": "move", "params": { "speed": 0.3 } }

{ "action": "rotate", "params": { "angle_deg": 45 } }

{ "action": "navigate", "params": { "x": 1.0, "y": 2.0 } }
```

*	**Importaciones y configuración:** Se asume que la variable de entorno OPENAI_API_KEY está configurada.:

```
# Importaciones y configuración: 
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain

# _PromptTemplate_ (núcleo del comportamiento): 
template = PromptTemplate(
    input_variables=["user_input"],
    template="""
Eres un intérprete de comandos para un robot móvil.
Convierte el siguiente comando en un JSON ESTRICTAMENTE válido.

Acciones permitidas:
- move: requiere speed (float)
- rotate: requiere angle_deg (float)
- navigate: requiere x, y (float)
- stop: sin parámetros
Comando: "{user_input}"
Responde SOLO con JSON.
"""
)

# Inicialización del LLM y la cadena
llm = OpenAI(
    temperature=0.3,
    model_name="gpt-4"
)

command_chain = LLMChain(
    llm=llm,
    prompt=template
)

# Ejemplo de uso del agente
user_command = "Navega hasta 1.0, 2.0 y luego gira 45 grados"
json_command = command_chain.run(user_command)
print(json_command)
```

Una salida típica de este código podría ser:
```
{
  "action": "navigate",
  "params": {
    "x": 1.0,
    "y": 2.0
  }
}
```

### 2. Nodo ROS 2: traducción JSON a acciones físicas

Esta parte se ejecuta dentro del ecosistema ROS 2, con una separación de responsabilidades, tal y como se ha indicado antes.
* Recibir JSON
* Validar estructura
* Traducir a primitivas ROS 2
* Publicar o llamar servicios


A continuación, se va a mostrar, el código. Las explicaciones del mismo se pueden encontrar en la Sección 9.3.3 del libro:

```
# Importaciones de librerías necesarias
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist
from nav_msgs.msg import Odometry
import json
import math


# Definición del nodo.

   def __init__(self):
        super().__init__('nl_controller')
        self.cmd_vel_pub = self.create_publisher(Twist, '/cmd_vel', 10)
        self.get_logger().info("Nodo de control por lenguaje natural iniciado")


# Función principal de interpretación
        try:
            cmd = json.loads(json_str)
            action = cmd["action"]
            params = cmd.get("params", {})
        except Exception as e:
            self.get_logger().error(f"JSON inválido: {e}")
            return

# Acción mover
if action == "move":
            speed = float(params.get("speed", 0.0))
            msg = Twist()
            msg.linear.x = speed
            self.cmd_vel_pub.publish(msg)

# Acción rotar
        elif action == "rotate":
            angle = float(params.get("angle_deg", 0.0))
            msg = Twist()
            msg.angular.z = math.radians(angle)

# Acción detener
        elif action == "stop":
            msg = Twist()
            self.cmd_vel_pub.publish(msg)
            self.cmd_vel_pub.publish(msg)

# Acción navegación global (placeholder)
        elif action == "navigate":
            x = params.get("x")
            y = params.get("y")
            self.get_logger().info(f"Navegando a ({x}, {y})")
            # Aquí se llamaría a _NavigateToPose (Nav2)_
```


La línea comentada (Aquí se llamaría a _NavigateToPose (Nav2)_) se podría sustituir por una llamada a función (llamada en esta occasion (send_goal) como esta:
```self.send_goal(cmd["params"]["x"],cmd["params"]["y"], cmd["params"].get("theta", 0.0))```

Habría que añadir el siguiente código:
```
from rclpy.action import ActionClient
from nav2_msgs.action import NavigateToPose
from geometry_msgs.msg import PoseStamped
class Nav2Controller(Node):
    def __init__(self):
        super().__init__('nav2_controller')
        self._action_client = ActionClient(self, NavigateToPose, 'navigate_to_pose')

    def send_goal(self, x, y, theta=0.0):
        goal_msg = NavigateToPose.Goal()
        pose = PoseStamped()
        pose.header.frame_id = "map"
        pose.pose.position.x = x
        pose.pose.position.y = y
        # Orientación simplificada en 2D, quaternion puede calcularse de theta
        pose.pose.orientation.w = 1.0  
        goal_msg.pose = pose

        self._action_client.wait_for_server()
        self._action_client.send_goal_async(goal_msg)
```


Siguiendo con el código
```
# Acción no soportada:

        else:
            self.get_logger().warn(f"Acción no reconocida: {action}")

# Función main:

def main(args=None):
    rclpy.init(args=args)
    node = NaturalLanguageController()

    # Ejemplo manual de prueba
    test_json = '{"action": "move", "params": {"speed": 0.2}}'
    node.handle_json_command(test_json)

    rclpy.spin(node)
    node.destroy_node()
   	    rclpy.shutdown()


# Ejemplo completo de ejecución
Se supone que la entrada del usuario es: «Avanza lentamente y luego para», Una vez traducido el LLM produce los siguientes JSON, uno por cada orden:

1.	{ "action": "move", "params": { "speed": 0.2 } }
2.	{ "action": "stop" }
  ``` 
A continuación, ROS 2 ejecuta:
* Publicación en /cmd_vel
* Robot se mueve
* Robot se detiene

```






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

#### Diseño de un agente LangChain

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

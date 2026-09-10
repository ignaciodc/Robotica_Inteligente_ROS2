## Instalación y configuración con Python
Todo el contenido de esta página y la explicación del código empleado se encuentra en la Sección 8.4 del libro.

### Requisitos previos mínimos
* Python 3.9 o superior.
* Entorno virtual recomendado (venv o conda).
* Acceso a un modelo LLM (API o modelo local).
  * API comercial (OpenAI, Cohere, Anthropic, etc.).
  * Modelos locales mediante HuggingFace Transformers

### Instalación básica
La instalación se realiza mediante pip:
```pip install langchain ```

Dependiendo del proveedor de modelos, pueden ser necesarios paquetes adicionales:
 ``` pip install openai transformers sentencepiece ```
Esto permite acceder a:
* Modelos OpenAI (GPT-3, GPT-4).
* Modelos locales de HuggingFace.
* Tokenizadores para modelos multilingües.

###	Configuración de credenciales

Para usar modelos comerciales como OpenAI con LangChain, se deben definir variables de entorno:

```export OPENAI_API_KEY="tu_api_key" # Linux/ macOS```

```setx OPENAI_API_KEY "tu_api_key" # Windows(CMD)```


###	Ejemplos de uso

### Ejemplo mínimo
Un ejemplo básico de interacción con un modelo usando LangChain, que ilustra cómo LangChain simplifica la interacción con modelos de lenguaje podría ser:

```
from langchain.llms import OpenAI
llm = OpenAI(temperature=0.0)
response = llm("Explica brevemente qué es un LLM")
print(response)
```

Instalaciones recomendadas para este ejemplo:

* Python 3.9 o superior.
* Entorno virtual recomendado (venv o conda).
* Librería LangChain:
  * ```pip install langchain.```
  * ```pip install langchain-community``` (necesario en versiones recientes).
* Cliente de OpenAI (en este ejemplo):
  * ```pip install openai```
* Clave de API de OpenAI:
  * ```export OPENAI_API_KEY="tu_api_key_aqui" (Linux)/macOS```
  * ```setx OPENAI_API_KEY "tu_api_key_aqui" Windows (CMD)```
 


###	Ejemplo completo con Chain, memoria y herramientas
A continuación, se presenta un ejemplo más completo, que incluye:
* Memoria de conversación (permite recordar el contexto entre múltiples interacciones).
* Uso de cadenas (Chains) para estructurar la interacción.
* Manejo de herramientas (pueden ser funciones externas o APIs conectadas al LLM).
* 
```
from langchain.chat_models import ChatOpenAI
from langchain.prompts import ChatPromptTemplate, MessagesPlaceholder
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferMemory

# Configurar LLM con temperatura controlada
chat_model = ChatOpenAI(
    temperature=0.3,
    model_name="gpt-3.5-turbo"
)

# Crear memoria para almacenar la conversación
memory = ConversationBufferMemory(
    memory_key="chat_history",
    input_key="user_input",
    output_key="response",
    return_messages=True
)

# Plantilla de prompt (estructura de la conversación)
prompt_template = ChatPromptTemplate.from_messages([
    ("system", "Eres un asistente técnico de ingeniería, profesional y conciso."),
    MessagesPlaceholder(variable_name="chat_history"),
    ("human", "{user_input}")
])

# Cadena de conversación con memoria
conversation = ConversationChain(
    llm=chat_model,
    prompt=prompt_template,
    memory=memory,
    verbose=True
)

# Ejecución de la conversación
user_messages = [
    "Explica qué es un modelo de lenguaje grande (LLM).",
    "¿Cuál es la diferencia entre GPT y BERT?",
    "Dame un ejemplo de uso de LLM en robótica."
]

for msg in user_messages:
    response = conversation.run(user_input=msg)
    print(f"Usuario: {msg}")
    print(f"Asistente: {response}\n")
```








<p align="center">
<img width="744" height="482" alt="image" src="https://github.com/user-attachments/assets/811c0741-9fa2-4876-88cc-f7568c7d0d91" />
<sub><b>Arquitectura conceptual de LangChain</b></sub> </p>

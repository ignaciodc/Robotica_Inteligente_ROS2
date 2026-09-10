### Ejemplo completo con Chain, memoria y herramientas

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


[← Volver atrás](Readme.md)

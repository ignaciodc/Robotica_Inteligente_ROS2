### Ejemplo completo empleando LangChain con la API de OpenAI

```
# 1. Importación de librerías
from langchain.llms import OpenAI
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain
import os

# 2. Configuración de credenciales
# Definir tu clave de API de OpenAI como variable de entorno antes de ejecutar el script:
# Linux/macOS: export OPENAI_API_KEY="tu_api_key"
# Windows PowerShell: setx OPENAI_API_KEY "tu_api_key"
api_key = os.getenv("OPENAI_API_KEY")
if not api_key:
    raise ValueError("No se encontró la variable de entorno OPENAI_API_KEY")

# 3. Inicialización del LLM
# Ajustamos parámetros como temperatura y max_tokens para controlar la creatividad y la extensión de la respuesta
llm = OpenAI(openai_api_key=api_key, temperature=0.7, max_tokens=300)

# 4. Creación de un PromptTemplate
# Este template permite personalizar la interacción con el LLM
prompt_template = PromptTemplate(
    input_variables=["topic"],
    template="Eres un asistente experto en ingeniería. Explica de manera clara y concisa el concepto de '{topic}' con ejemplos prácticos."
)
# 5. Construcción de la cadena LLMChain
chain = LLMChain(llm=llm, prompt=prompt_template)

# 6. Ejecución del ejemplo
topic = "arquitectura Transformer en NLP"
response = chain.run(topic)
print("Respuesta del modelo:\n")
print(response)

# 7. Flujo adicional: múltiples preguntas
questions = [
    "Ventajas de usar Transformers frente a RNN",
    "Ejemplo de aplicación en robótica",
    "Cómo escalar un LLM en producción"
]

print("\nRespuestas adicionales:\n")
for q in questions:
    resp = chain.run(q)
    print(f"Tema: {q}\n{resp}\n{'-'*80}")

```

<br>

[← Volver atrás](Readme.md)


### Ejemplo completo usando LangChain con un modelo de HuggingFace

```
# 1. Importación de librerías
from langchain.llms import HuggingFacePipeline
from langchain.prompts import PromptTemplate
from langchain.chains import LLMChain
from transformers import pipeline

# 2. Configuración del pipeline de HuggingFace
# Aquí usamos un modelo de ejemplo: "google/flan-t5-small" para generación de texto
hf_pipeline = pipeline(
    "text2text-generation",
    model="google/flan-t5-small",
    tokenizer="google/flan-t5-small",
    device=0  # 0 para GPU, -1 para CPU
)

# 3. Inicialización del LLM con HuggingFacePipeline
llm = HuggingFacePipeline(pipeline=hf_pipeline)

# 4. Creación de un PromptTemplate
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
    "Cómo escalar un modelo HuggingFace en producción"
]

print("\nRespuestas adicionales:\n")
for q in questions:
    resp = chain.run(q)
    print(f"Tema: {q}\n{resp}\n{'-'*80}")
```

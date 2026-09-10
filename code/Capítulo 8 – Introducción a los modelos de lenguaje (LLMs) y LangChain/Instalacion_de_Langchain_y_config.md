## Instalación y configuración con Python
Todo el contenido de esta página se encuentra en la Sección 8.4 del libro.

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









<p align="center">
<img width="744" height="482" alt="image" src="https://github.com/user-attachments/assets/811c0741-9fa2-4876-88cc-f7568c7d0d91" />
<sub><b>Arquitectura conceptual de LangChain</b></sub> </p>

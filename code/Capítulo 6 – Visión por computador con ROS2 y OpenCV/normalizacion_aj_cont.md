### 6.4.3.	Normalización y ajuste de contraste

Este código se encuentra en la Sección 6.4.3 del libro. En entornos reales, los cambios de iluminación son frecuentes y afectan significativamente la calidad de la percepción visual de los robots. 

* **Ecualización de histograma:** La ecualización de histograma es un método que redistribuye los niveles de intensidad de una imagen para que ocupen todo el rango dinámico disponible. Esto mejora el contraste y permite resaltar detalles que podrían perderse en condiciones de iluminación baja o alta. 

``` 
import cv2

# Conversión a escala de grises
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

# Aplicación de ecualización de histograma
equalized = cv2.equalizeHist(gray)

``` 

* **Ajuste de brillo y contraste:**	permite modificar directamente la intensidad de los píxeles y la dispersión de valores, compensando efectos de subexposición o sobreexposición.

``` 
alpha = 1.2  # Contraste (1.0 = sin cambio)
beta = 30    # Brillo (0 = sin cambio)

adjusted = cv2.convertScaleAbs(frame, alpha=alpha, beta=beta)
```



[← Volver atrás](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%206%20%E2%80%93%20Visi%C3%B3n%20por%20computador%20con%20ROS2%20y%20OpenCV/Preprocesamiento_de_imagenes.md)

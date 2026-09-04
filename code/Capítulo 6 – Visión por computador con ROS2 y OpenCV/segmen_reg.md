### Segmentación de regiones

Se encuentra en la Sección 6.4.5 del libro. La segmentación de regiones es una operación fundamental en visión por computador, cuyo objetivo es dividir una imagen en regiones homogéneas que correspondan a objetos o partes relevantes de la escena. Existen varias técnicas ampliamente utilizadas para realizar segmentación, cada una con ventajas y limitaciones según el entorno y la naturaleza de los objetos:

* **Umbralización:** La umbralización es la técnica más simple y consiste en convertir una imagen a binaria en función de un umbral de intensidad.
* 
```
import cv2

# Conversión a escala de grises
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

# Umbralización global
_, binary = cv2.threshold(gray, 127, 255, cv2.THRESH_BINARY)

cv2.imshow("Thresholded Image", binary)
cv2.waitKey(1)
```

* **Segmentación por color:** La segmentación por color aprovecha espacios de color como HSV para aislar regiones que cumplen ciertas características cromáticas.

```
 import cv2
import numpy as np

# Conversión a HSV
hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

# Definición de rango de color
lower_red = np.array([0, 100, 100])
upper_red = np.array([10, 255, 255])

# Creación de máscara
mask = cv2.inRange(hsv, lower_red, upper_red)

# Aplicación de la máscara
segmented = cv2.bitwise_and(frame, frame, mask=mask)

cv2.imshow("Segmented by Color", segmented)
cv2.waitKey(1)
```

* **Crecimiento de regiones:** El crecimiento de regiones es una técnica más sofisticada que agrupa píxeles contiguos con características similares, basándose en intensidad, color o textura. 

```
# Se plantea como pseudocódigo conceptual
region = seed_pixel
while similar_neighbors(region):
    expand(region)
```



[← Volver atrás](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%206%20%E2%80%93%20Visi%C3%B3n%20por%20computador%20con%20ROS2%20y%20OpenCV/Preprocesamiento_de_imagenes.md)






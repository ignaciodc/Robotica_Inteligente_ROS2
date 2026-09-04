### Detección de bordes

Se encuentra en la Sección 6.4.4 del libro. La detección de bordes es una de las operaciones más fundamentales en visión por computador y procesamiento de imágenes. Los bordes representan cambios bruscos de intensidad o color, que suelen corresponder a contornos de objetos, límites de superficies o transiciones en el entorno. Detectar estos bordes de manera fiable permite al robot interpretar correctamente la geometría de la escena y simplificar la información para algoritmos posteriores.

 ``` 
import cv2

# Conversión a escala de grises
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

# Aplicación de filtro gaussiano para suavizado
blur = cv2.GaussianBlur(gray, (5, 5), 0)

# Detección de bordes mediante Canny
edges = cv2.Canny(blur, threshold1=50, threshold2=150)

# Visualización
cv2.imshow("Edges", edges)
cv2.waitKey(1)
 ``` 



[← Volver atrás](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%206%20%E2%80%93%20Visi%C3%B3n%20por%20computador%20con%20ROS2%20y%20OpenCV/Preprocesamiento_de_imagenes.md)


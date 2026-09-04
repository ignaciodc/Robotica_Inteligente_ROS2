## Conversión de espacios de color
El código se encuentra en la Sección 6.4.1. Sería el siguiente. Notar que usamos la bien conocida librería OpenCV ```import cv2``` en Python.
```
import cv2

# Captura de un frame desde la cámara
frame = cv2.imread("imagen_robot.png")

# Conversión a escala de grises
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

# Conversión a HSV
hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

```



[← Volver atrás](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%206%20%E2%80%93%20Visi%C3%B3n%20por%20computador%20con%20ROS2%20y%20OpenCV/Preprocesamiento_de_imagenes.md)


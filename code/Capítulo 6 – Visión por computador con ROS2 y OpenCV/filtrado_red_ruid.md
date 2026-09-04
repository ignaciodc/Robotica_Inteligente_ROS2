### Filtrado y reducción de ruido
El código de esta parte se encuentra en la Sección 6.4.2 del libro. El ruido visual puede afectar gravemente a los algoritmos de detección. La elección del filtro depende del tipo de ruido y del compromiso entre suavizado y preservación de bordes. OpenCV ofrece filtros como:
* Filtro gaussiano: Suaviza el ruido uniforme o aleatorio
´´´
import cv2
# Imagen en escala de grises
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
# Aplicación de filtro gaussiano
blur_gauss = cv2.GaussianBlur(gray, (5, 5), 0)
´´´

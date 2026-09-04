### Detección por color
Se corresponde con la Sección 6.5.1 del libro. La detección de objetos basada en color es una de las técnicas más sencillas y eficientes dentro de la visión por computador. Su objetivo es identificar regiones de la imagen que correspondan a un color específico, aislando objetos que tengan tonalidades distintivas respecto al fondo. Para aplicar este método se deben seguir los siguientes pasos:

* **Conversión a espacio de color HSV:** El espacio HSV (Hue, Saturation, Value) separa la información de color (matiz) de la intensidad luminosa. Esto hace que la detección sea más robusta frente a cambios de iluminación que en RGB/BGR. En este caso el código sería el mismo que se propuso en la Sección 6.4.1 del libro, es decir:

 ```   
import cv2

# Captura de un frame desde la cámara
frame = cv2.imread("imagen_robot.png")

# Conversión a escala de grises
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

# Conversión a HSV
hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
 ``` 

* **Definición de rangos de color:** Se especifica un rango inferior y superior para el matiz, saturación y valor que correspondan al color del objeto a detectar. El código para este paso sería:

 ``` 
import numpy as np

# Rango de color rojo en HSV
lower_red = np.array([0, 100, 100])
upper_red = np.array([10, 255, 255])
 ```

* **Generación de máscara binaria:** Se crea una máscara binaria, donde los píxeles que caen dentro del rango definido se establecen como blancos (255) y el resto como negros (0). Esto permite aislar las regiones de interés para procesamiento posterior.

 ``` 
# Creación de la máscara
mask = cv2.inRange(hsv, lower_red, upper_red)

# Aplicación de la máscara sobre la imagen original
result = cv2.bitwise_and(frame, frame, mask=mask)

# Visualización
cv2.imshow("Detección por color", result)
cv2.waitKey(0)
cv2.destroyAllWindows()
 ``` 

A continuación, puede observar un ejemplo avanzado en ROS 2, donde un **nodo se puede integrar la detección por color con un suscriptor de cámara y publicar la posición del objeto detectado**:

 ``` 
import rclpy
from rclpy.node import Node
from sensor_msgs.msg import Image
from cv_bridge import CvBridge
import cv2
import numpy as np

class ColorTracker(Node):
    def __init__(self):
        super().__init__('color_tracker')
        self.bridge = CvBridge()
        self.subscription = self.create_subscription(
            Image,
            '/camera/image_raw',
            self.image_callback,
            10
        )

    def image_callback(self, msg):
        frame = self.bridge.imgmsg_to_cv2(msg, desired_encoding='bgr8')
        hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
        lower_red = np.array([0,100,100])
        upper_red = np.array([10,255,255])
        mask = cv2.inRange(hsv, lower_red, upper_red)
        result = cv2.bitwise_and(frame, frame, mask=mask)
        cv2.imshow("Detección por color", result)
        cv2.waitKey(1)

def main(args=None):
    rclpy.init(args=args)
    node = ColorTracker()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
 ``` 



[← Volver atrás](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%206%20%E2%80%93%20Visi%C3%B3n%20por%20computador%20con%20ROS2%20y%20OpenCV/deteccion_objetos.md)



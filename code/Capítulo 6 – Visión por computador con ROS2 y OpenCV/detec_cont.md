###	Detección de contornos

El código de este apartado se encuentra en la Sección 6.5.2 del libro. La detección de contornos es una técnica clave en visión por computador, que se aplica sobre imágenes binarizadas (por ejemplo, después de segmentación por color o umbralización). Se puede crear un nodo que reciba imágenes de la cámara, aplique segmentación por color y detecte contornos, como puede apreciar en el siguiente código completo para ROS 2, con una estructura similar a la que ya se comentó en capítulos anteriores, junto a las particularidades que se han mostrado en este capítulo:

 ``` 
import rclpy
from rclpy.node import Node
from sensor_msgs.msg import Image
from cv_bridge import CvBridge
import cv2
import numpy as np

class ContourDetector(Node):
    def __init__(self):
        super().__init__('contour_detector')
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
        contours, _ = cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

        for cnt in contours:
            area = cv2.contourArea(cnt)
            if area > 500:
                M = cv2.moments(cnt)
                cx = int(M["m10"]/M["m00"])
                cy = int(M["m01"]/M["m00"])
                cv2.circle(frame, (cx, cy), 5, (255,0,0), -1)
        cv2.imshow("Contornos", frame)
        cv2.waitKey(1)

def main(args=None):
    rclpy.init(args=args)
    node = ContourDetector()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
 ``` 



[← Volver atrás](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%206%20%E2%80%93%20Visi%C3%B3n%20por%20computador%20con%20ROS2%20y%20OpenCV/deteccion_objetos.md)

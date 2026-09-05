
## Ejemplo de código TensorFlow integrado en ROS 2


Este ejemplo, y su explicación paso a paso, se encuentra en la Sección 7.2.2.5 del libro. A continuación, se muestra un ejemplo completo de código en Python empleando Pytorch con integración en ROS 2. Este código implementa un nodo ROS 2 en Python que utiliza un modelo de detección de objetos preentrenado en TensorFlow para procesar imágenes procedentes de una cámara y extraer información semántica sobre los objetos detectados en la escena.

```
import rclpy
from rclpy.node import Node
from sensor_msgs.msg import Image
from cv_bridge import CvBridge
import tensorflow as tf
import cv2
import numpy as np

class ObjectDetector(Node):
    def __init__(self):
        super().__init__('object_detector')
        self.bridge = CvBridge()
        self.subscription = self.create_subscription(
            Image,
            '/camera/image_raw',
            self.image_callback,
            10
        )
        # Cargar modelo SSD preentrenado
        self.model = tf.saved_model.load('ssd_mobilenet_v2_fpnlite_320x320/saved_model')

def image_callback(self, msg):
        cv_image = self.bridge.imgmsg_to_cv2(msg, desired_encoding='bgr8')
        input_tensor = tf.convert_to_tensor(cv_image)
        input_tensor = input_tensor[tf.newaxis,...]
        detections = self.model(input_tensor)
        # Extracción de resultados
        boxes = detections['detection_boxes'][0].numpy()
        scores = detections['detection_scores'][0].numpy()
        classes = detections['detection_classes'][0].numpy()
        for i in range(len(scores)):
            if scores[i] > 0.5:
                print(f'Clase: {int(classes[i])}, Confianza: {scores[i]:.2f}')

def main(args=None):
    rclpy.init(args=args)
    node = ObjectDetector()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()

```

  <br>
  
  [← Volver atrás](Readme.md)




## Ejemplo de código TensorFlow integrado en ROS 2


Este ejemplo se encuentra en la Sección 7.2.2.5 del libro. A continuación, se muestra un ejemplo completo de código en Python empleando Pytorch con integración en ROS 2. Este código implementa un nodo ROS 2 en Python que utiliza un modelo de detección de objetos preentrenado en TensorFlow para procesar imágenes procedentes de una cámara y extraer información semántica sobre los objetos detectados en la escena.


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

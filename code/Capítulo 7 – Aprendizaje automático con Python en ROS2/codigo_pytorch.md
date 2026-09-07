## Ejemplo de código Pytorch integrado en ROS 2
Este ejemplo, y su explicación paso a paso, se encuentra en la Sección 7.2.1.5 del libro. A continuación, se muestra un ejemplo completo de código en Python empleando Pytorch con integración en ROS 2 para clasificación de imágenes.

```
import rclpy
from rclpy.node import Node
from sensor_msgs.msg import Image
from cv_bridge import CvBridge
import torch
import torchvision.transforms as transforms
from torchvision import models
import cv2
import numpy as np


class ImageClassifier(Node):
    def __init__(self):
        super().__init__('image_classifier')
        self.bridge = CvBridge()
        self.subscription = self.create_subscription(
            Image,
            '/camera/image_raw',
            self.image_callback,
            10
        )
        # Cargar modelo preentrenado
        self.model = models.resnet18(pretrained=True)
        self.model.eval()
        # Transformaciones
        self.transform = transforms.Compose([
            transforms.ToPILImage(),
            transforms.Resize((224, 224)),
            transforms.ToTensor(),
            transforms.Normalize(mean=[0.485, 0.456, 0.406],
                                 std=[0.229, 0.224, 0.225])
        ])
        # Cargar etiquetas de ImageNet
        with open('imagenet_classes.txt') as f:
            self.labels = [line.strip() for line in f.readlines()]

    def image_callback(self, msg):
        cv_image = self.bridge.imgmsg_to_cv2(msg, desired_encoding='bgr8')
        input_tensor = self.transform(cv_image).unsqueeze(0)
        with torch.no_grad():
            output = self.model(input_tensor)
        _, pred = torch.max(output, 1)
        label = self.labels[pred.item()]
        self.get_logger().info(f'Objeto detectado: {label}')

def main(args=None):
    rclpy.init(args=args)
    node = ImageClassifier()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()

```

  <br>
  
  [← Volver atrás](Readme.md)

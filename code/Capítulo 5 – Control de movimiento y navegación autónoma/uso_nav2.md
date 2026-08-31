## 5.2.3.	Uso básico de Nav2 desde Python


```from nav2_simple_commander.robot_navigator import BasicNavigator
import rclpy

rclpy.init()
navigator = BasicNavigator()
# Esperar a que Nav2 esté activo
navigator.waitUntilNav2Active()
# Definir un objetivo en el mapa
pose = navigator.getPoseStamped([2.0, 1.0, 0.0], frame_id='map')
navigator.goToPose(pose)
while not navigator.isTaskComplete():
rclpy.spin_once(navigator)
rclpy.shutdown()
```

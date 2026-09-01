## 5.2.3.	Uso básico de Nav2 desde Python

Aunque Nav2 se configura principalmente mediante archivos YAML y launch files, es habitual interactuar con él desde nodos Python para:
* Enviar objetivos de navegación.
*	Cancelar o modificar rutas.
*	Supervisar el estado del robot.



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
[← Volver atrás](Readme.md)

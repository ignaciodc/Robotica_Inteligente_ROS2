##	Entornos virtuales en Python

Aunque no es obligatorio, es más que recomendable trabajar con ROS 2 mediante entornos virtuales de programación, ya que por un lado evita conflictos entre versiones de paquetes Python usados por ROS 2 y otros proyectos o el sistema. 

Por ejemplo, se pueden instalar versiones específicas de bibliotecas (numpy, opencv, tensorflow, etc.) sin afectar el entorno global de Python ni al propio ROS. La secuencia necesaria para crear un entorno virtual es la siguiente:

```
sudo apt install python3-venv
python3 -m venv ros2_ws/venv
source ros2_ws/venv/bin/activate
```


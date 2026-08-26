## Compilación y ejecución 


Los pasos que le permitirán realizar una compilación correcta, y posterior ejecución de un paquete son las siguientes:
```
cd ~/ros2_ws
colcon build --packages-select mi_paquete_python
source install/setup.bash
ros2 run mi_paquete_python <nodo>
```


[← Volver atrás](Readme.md)

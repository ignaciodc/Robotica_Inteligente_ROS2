## Gazebo - Simulador


El código de esta fase se encuentra en la Sección  4.5.4. En esta fase se conecta el simulador con la definición del robot. Sin este _plugin_, no hay simulación controlada.

```
<gazebo>
  <plugin name="gazebo_ros2_control" filename="libgazebo_ros2_control.so"/>
</gazebo>
```

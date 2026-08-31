
## _ros2_control_ — Interfaces de hardware

Según el esquema de funcionamiento de la simulación: _ros2_control_ actuará de puente entre el modelo y el control traduciendo los _joints_ del URDF en interfaces de control. 


```
<ros2_control name="GazeboSystem" type="system">
  <hardware>
    <plugin>gazebo_ros2_control/GazeboSystem</plugin>
  </hardware>

  <joint name="left_wheel_joint">
    <command_interface name="velocity"/>
    <state_interface name="velocity"/>
  </joint>

  <joint name="right_wheel_joint">
    <command_interface name="velocity"/>
    <state_interface name="velocity"/>
  </joint>
</ros2_control>
```
[← Volver atrás](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%204%20%E2%80%93%20Simulaci%C3%B3n%20con%20Gazebo%20y%20RViz/Ejemplo_completo_robot_simulado.md)

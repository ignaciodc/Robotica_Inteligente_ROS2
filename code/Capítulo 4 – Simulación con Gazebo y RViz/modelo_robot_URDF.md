##	URDF (modelo del robot)

El siguiente código se encuentra en la Sección 4.5.1 del libro. En el mismo se detalla de una forma más amplia cada una  En él se define, de forma completa, el modelo del robot. Así, lo que a continuación se muestra es el modelo de un robot móvil terrestre de tipo diferencial:
```
<?xml version="1.0"?>
<robot name="robot_simple">

  <!-- Link base -->
  <link name="base_link">
    <inertial>
      <mass value="5.0"/>
      <origin xyz="0 0 0"/>
      <inertia ixx="0.1" iyy="0.1" izz="0.1"
               ixy="0.0" ixz="0.0" iyz="0.0"/>
    </inertial>

    <visual>
      <geometry>
        <box size="0.4 0.3 0.1"/>
      </geometry>
    </visual>

    <collision>
      <geometry>
        <box size="0.4 0.3 0.1"/>
      </geometry>
    </collision>
  </link>

  <!-- Rueda izquierda -->
  <link name="left_wheel"/>
  <joint name="left_wheel_joint" type="continuous">
    <parent link="base_link"/>
    <child link="left_wheel"/>
    <axis xyz="0 1 0"/>
  </joint>

  <!-- Rueda derecha -->
  <link name="right_wheel"/>
  <joint name="right_wheel_joint" type="continuous">
    <parent link="base_link"/>
    <child link="right_wheel"/>
    <axis xyz="0 1 0"/>
  </joint>
</robot>
```

https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%204%20%E2%80%93%20Simulaci%C3%B3n%20con%20Gazebo%20y%20RViz/Ejemplo_completo_robot_simulado.md

4.5.1.	URDF (modelo del robot)
En el siguiente código se define, de forma completa, el modelo del robot, como se ha estado mostrando a lo largo de este Capítulo. Se ha decidido implementar un robot sencillo, pero excesivamente simple, para que pueda ver algo más elaborado en el ejemplo. Así, lo que a continuación se muestra es el modelo de un robot móvil terrestre de tipo diferencial:
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

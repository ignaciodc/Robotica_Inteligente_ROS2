## Estructura básica de un URDF

Un URDF típico se estructura como:

<robot name="mi_robot">
    <link name="base_link">
        <visual> ... </visual>
        <collision> ... </collision>
        <inertial> ... </inertial>
    </link>

    <joint name="articulacion1" type="continuous">
        <parent link="base_link"/>
        <child link="link1"/>
        <origin xyz="0 0 0.1" rpy="0 0 0"/>
        <axis xyz="0 0 1"/>
    </joint>

    <link name="link1">
        ...
    </link>
</robot>
Los elementos clave del código anterior son los siguientes:
•	Link
	visual: representación gráfica.
	collision: volumen geométrico utilizado para colisiones.
	inertial: masa, centro de masa y tensor de inercia.
•	Joint
	type: continuous, revolute, prismatic, fixed, planar.
	axis: eje del movimiento.
	origin: posición de la articulación respecto al padre.
	limit: velocidad, esfuerzo y ángulos.


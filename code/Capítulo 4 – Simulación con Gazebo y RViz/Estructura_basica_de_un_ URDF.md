## Estructura básica de un URDF

Un URDF típico se estructura como:
```
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
```
Los elementos clave del código anterior son los siguientes:
* **Link**
    * **_visual_**: representación gráfica.
    * **_collision_**: volumen geométrico utilizado para colisiones.
    * **_inertial_**: masa, centro de masa y tensor de inercia.
* **Joint**
    * **_type_**: continuous, revolute, prismatic, fixed, planar.
    * **_axis_**: eje del movimiento.
    * **_origin_**: posición de la articulación respecto al padre.
    * **_limit_**: velocidad, esfuerzo y ángulos.
 

[← Volver atrás](Readme.md)


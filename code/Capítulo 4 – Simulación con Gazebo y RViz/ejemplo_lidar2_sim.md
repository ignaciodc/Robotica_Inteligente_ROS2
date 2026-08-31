## Diseño del sensor Lidar 2D en Gazebo
Este ejemplo se corresponde a la Sección 4.4.2.2. Uno de los sensores más utilizados en robótica móvil es el LIDAR 2D. En Gazebo, se implementa mediante un sensor de tipo ray.
```
<gazebo reference="lidar_link">
  <sensor type="ray" name="lidar">
    <update_rate>10</update_rate>
    <ray>
      <scan>
        <horizontal>
          <samples>720</samples>
          <min_angle>-1.57</min_angle>
          <max_angle>1.57</max_angle>
        </horizontal>
      </scan>
      <range>
        <min>0.12</min>
        <max>10.0</max>
      </range>
    </ray>
  </sensor>
</gazebo>
```
Este sensor publica mensajes LaserScan que pueden ser consumidos directamente por nodos de navegación, SLAM o evitación de obstáculos.



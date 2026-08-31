##	Controlador — _diff_drive_controller_

El código de esta fichero se puede encontrar en la Sección 4.5.3. del libro. En este caso se va a emplear un fichero de configuración «.yaml», de manera que se inicia creando el controller_manager que va a permitir gestionar qué controladores se cargan. El contenido del mismo sería el siguiente:

```
controller_manager:
  ros__parameters:
    update_rate: 100
    diff_drive_controller:
      type: diff_drive_controller/DiffDriveController
    joint_state_broadcaster:
      type: joint_state_broadcaster/JointStateBroadcaster
diff_drive_controller:
  ros__parameters:
    left_wheel_names: ["left_wheel_joint"]
    right_wheel_names: ["right_wheel_joint"]
    wheel_separation: 0.3
    wheel_radius: 0.05
    cmd_vel_timeout: 0.5
    publish_rate: 50.0
```


[← Volver atrás](https://github.com/ignaciodc/Robotica_Inteligente_ROS2/blob/main/code/Cap%C3%ADtulo%204%20%E2%80%93%20Simulaci%C3%B3n%20con%20Gazebo%20y%20RViz/Ejemplo_completo_robot_simulado.md)

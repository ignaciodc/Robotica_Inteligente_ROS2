# Capítulo 9 – Integración de LangChain con ROS2


<p align="center">
<img width="280" height="509" alt="image" src="https://github.com/user-attachments/assets/efaf0765-75f8-4202-845a-56dabf605ae0" />

<sub><b>Arquitectura de integración de LangChain en ROS 2</b></sub> </p>


##	Caso práctico: control de robot con lenguaje natural

### Escenario

Suponga un robot móvil básico (real o en simulación, p. ej., en Gazebo) con:
* Un publisher a /cmd_vel para movimiento (velocidad lineal y angular).
* Un servicio de navegación para metas globales.
* Un nodo de control de manipulador que acepta comandos de posición.
El objetivo es que un operador pueda dictar comandos como:
* «Avanza hacia la puerta y para cuando detectes un obstáculo». 	
* «Navega hasta [1.0, 2.0] y luego gira 45 grados».
  
y que el sistema procese estas órdenes de lenguaje natural hasta acciones robóticas concretas. El diseño se va a basar en las tres capas:

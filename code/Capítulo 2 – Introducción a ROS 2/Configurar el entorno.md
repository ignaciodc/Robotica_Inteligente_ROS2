## Configurar el entorno:

Para configurar el entorno se debe añadir la línea ```~/.bashrc``` al final del archivo _setup_. Esto hace que cada vez que abra una terminal se cargue automáticamente el entorno de ROS 2 Humble. 

La segunda línea aplica inmediatamente los cambios realizados en ```~/.bashrc``` sin necesidad de cerrar y abrir la terminal. El conjunto de instrucciones para conseguir esto sería:
```
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
source ~/.bashrc
```


[← Volver atrás](Readme.md)

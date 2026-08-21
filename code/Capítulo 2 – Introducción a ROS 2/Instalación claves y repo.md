### Instalación de claves y repositorio

Para la instalación de las claves y el repositorio se deben realizar los siguientes pasos, en el mismo orden:

**1. Instalar herramientas necesarias**
```
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt install curl gnupg lsb-release
```
**2. Crear carpeta segura para claves si no existe**
```
sudo mkdir -p /etc/apt/keyrings
```
**3. Descargar la clave GPG y guardarla como archivo**
```
curl -sSL https://raw.githubusercontent.com/ros/rosdistro/ \\ <br>  
master/ros.asc | sudo gpg --dearmor -o /etc/apt/keyrings/ros.gpg
```



**4. Anadir el repositorio de ROS 2 usando la clave GPG del archivo**
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/ros.gpg] \\ <br>
http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" | \\ <br>
sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null \\ <br>
```

**5. Actualizar los paquetes**
```
sudo apt update
```
**NOTA: Las "\\" detrás de algunas de las líneas se emplean para indicar que la instrucción continúa debajo. Si la instrucción entra en una línea (como sucede en una Terminal), no es necesario escribir la "\\"**

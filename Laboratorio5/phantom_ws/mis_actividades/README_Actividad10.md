# Actividad 10: Trayectoria Sinusoidal de una Articulación

## 📖 Descripción
En la presente actividad se provee una Interfaz Gráfica de Usuario (GUI) orientada a la evaluación del comportamiento dinámico y el seguimiento de trayectoria de un único servomotor del manipulador Phantom X Pincher. 

En concordancia con los requerimientos de la guía de laboratorio, el sistema comanda el desplazamiento continuo de la articulación seleccionada con base en la siguiente función matemática:

$$q(t) = q_0 + A \sin(2\pi f t)$$

La interfaz ha sido diseñada para facilitar la ejecución de las **cuatro pruebas experimentales** requeridas (mediante la combinación de dos amplitudes y dos frecuencias distintas). Con el fin de garantizar que el manipulador opere exclusivamente dentro de los límites seguros (establecidos en la Actividad 6), el algoritmo **calcula de manera dinámica la Amplitud Máxima permisible** en función de la posición central ($q_0$) definida por el usuario. Tras la culminación de cada prueba, se genera de forma automática una gráfica donde se contrasta la posición deseada frente a la posición real medida, procediendo al cálculo del **Error Máximo** y del **Error Cuadrático Medio (MSE)**.

## 🛠️ Requisitos
Para el entorno operativo Ubuntu bajo ROS 2 Jazzy, se requiere la instalación de las librerías matemáticas y gráficas a través del gestor de paquetes del sistema operativo:

```bash
sudo apt update
sudo apt install python3-tk python3-numpy python3-matplotlib
```

## 🚀 Instrucciones de Ejecución
En una terminal independiente, se debe ingresar al espacio de trabajo, cargar el entorno y ejecutar el script correspondiente mediante las siguientes instrucciones:

```bash
# 1. Navegación al espacio de trabajo
cd ~/Laboratorio5/phantom_ws

# 2. Carga del entorno de ROS 2 y del workspace compilado
source /opt/ros/jazzy/setup.bash
source install/setup.bash

# 3. Ejecución de la interfaz de trayectoria sinusoidal
python3 mis_actividades/actividad10.py
```

## 🎯 Interfaz y Flujo de Trabajo
1. **Selección de Articulación:** Se debe elegir la articulación a evaluar mediante el primer menú desplegable.
2. **Posición Central ($q_0$):** Es necesario definir el punto de equilibrio de la onda. El sistema validará internamente que esta coordenada se encuentre dentro de los límites mecánicos seguros.
3. **Amplitud ($A$):** Se define la magnitud de la oscilación respecto a $q_0$. El algoritmo restringirá matemáticamente la inserción de cualquier valor cuya suma resulte en una colisión del manipulador contra sus topes físicos.
4. **Frecuencia y Tiempo:** Se deben ingresar los parámetros de frecuencia ($f$) en Hz y el tiempo total de duración de la prueba en segundos.
5. **Ejecución y Análisis:** Tras accionar el comando **▶ EJECUTAR TRAYECTORIA SINUSOIDAL**, el actuador se desplazará de manera interpolada hacia $q_0$ para dar inicio a la oscilación. Al concluir el tiempo definido, el sistema retornará a su posición de reposo (0°) y se desplegará la gráfica de resultados, la cual será almacenada automáticamente en el directorio local para su posterior inclusión en los informes respectivos.

## ⚠️ Consideraciones de Seguridad
* **🚨 PARADA DE EMERGENCIA:** Se dispone de un control de acción inmediata cuya activación interrumpe el bucle de la función sinusoidal y suprime el torque del motor de forma instantánea ante la detección de vibraciones de alta frecuencia o comportamientos anómalos.
* **Control de Hardware:** Se incluye un panel intermedio destinado a la habilitación o deshabilitación manual del torque de los actuadores (Torque ON / OFF), así como un comando de acceso rápido para forzar el retorno de todas las articulaciones a su posición de reposo (HOME a 0°).

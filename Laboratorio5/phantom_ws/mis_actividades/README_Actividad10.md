# Actividad 10 - Trayectoria Sinusoidal de una Articulación

## 📖 Descripción
Esta actividad proporciona una Interfaz Gráfica (GUI) para evaluar el comportamiento dinámico y el seguimiento de trayectoria de un único servomotor del Phantom X Pincher. 

De acuerdo con la guía de laboratorio, el programa hace que la articulación seleccionada describa un movimiento continuo basado en la función matemática:
$$q(t) = q_0 + A \sin(2\pi f t)$$

La interfaz está diseñada para que realices fácilmente las **cuatro pruebas experimentales** exigidas (combinando dos amplitudes y dos frecuencias diferentes). Para garantizar que el robot permanezca dentro de los límites seguros (Actividad 6), el software **calcula dinámicamente la Amplitud Máxima permitida** basándose en la posición central ($q_0$) ingresada por el usuario. Al finalizar cada prueba, genera automáticamente una gráfica comparando la posición deseada vs la medida, calculando el **Error Máximo** y el **Error Cuadrático Medio (MSE)**.

## 🛠️ Requisitos
Asegúrate de tener instaladas las librerías matemáticas y gráficas en tu entorno de Ubuntu / ROS 2 Jazzy utilizando el gestor de paquetes del sistema operativo:

```bash
sudo apt update
sudo apt install python3-tk python3-numpy python3-matplotlib
```

## 🚀 Ejecución
Abre una terminal en tu workspace y ejecuta el script:

```bash
cd ~/ros2_jazzy/Lab5/Codigos/phantom_ws
python3 mis_actividades/actividad10.py
```

## 🎯 Interfaz y Flujo de Trabajo
1. **Selección de Articulación:** Elige la articulación a evaluar en el primer menú desplegable.
2. **Posición Central ($q_0$):** Define el punto de equilibrio de la onda. El sistema validará que este punto esté dentro de los límites mecánicos seguros.
3. **Amplitud ($A$):** Define qué tanto oscilará el robot respecto a $q_0$. El programa bloqueará matemáticamente cualquier valor que pueda hacer que la cresta de la onda estrelle el robot contra sus topes físicos.
4. **Frecuencia y Tiempo:** Ingresa la frecuencia ($f$) en Hz y el tiempo total de la prueba en segundos.
5. **Ejecución y Análisis:** Al presionar **▶ EJECUTAR TRAYECTORIA SINUSOIDAL**, el robot viajará suavemente a $q_0$ y comenzará a oscilar. Al terminar el tiempo, retornará a $0^\circ$ y desplegará la gráfica de resultados (la cual se autoguardará en la misma carpeta del script para tu informe).

## ⚠️ Herramientas de Seguridad
* **🚨 PARADA DE EMERGENCIA:** Botón de acción inmediata que interrumpe el bucle de la ecuación sinusoidal y corta el torque del motor instantáneamente ante vibraciones o movimientos peligrosos.
* **Control de Hardware:** Panel intermedio para habilitar o liberar los motores (Torque ON / OFF) y un botón de acceso rápido para llevar todas las articulaciones a HOME ($0^\circ$).
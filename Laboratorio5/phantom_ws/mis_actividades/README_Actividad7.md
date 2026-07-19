# Actividad 7 - Movimiento Simultáneo y Validación de Límites

## 📖 Descripción
Esta actividad implementa una Interfaz Gráfica (GUI) para comandar el desplazamiento **simultáneo** de todas las articulaciones del robot Phantom X Pincher X100. 

Siguiendo las instrucciones de la guía de laboratorio, el sistema tiene pre-programadas las 5 configuraciones articulares solicitadas:
1. `[0, 0, 0, 0, 0]`
2. `[25, 25, 20, -20, 0]`
3. `[-35, 35, -30, 30, 0]`
4. `[85, -20, 55, 25, 0]`
5. `[80, -35, 55, -45, 0]`

El aspecto más importante de este script es su **Sistema de Validación Automática**. Antes de mover el robot, el programa compara la configuración teórica solicitada con los límites seguros experimentales guardados en la Actividad 6. Si una configuración no es segura, el software la trunca automáticamente a los límites permitidos e imprime una **justificación detallada** en la consola gráfica explicando el riesgo (ej. riesgo de colisión o daño al servomotor).

## 🛠️ Requisitos
El entorno gráfico requiere el módulo de Tkinter para ROS 2 en Ubuntu. Asegúrate de tenerlo instalado usando `apt`:

```bash
sudo apt update
sudo apt install python3-tk
```

## 🚀 Ejecución
Abre una terminal en tu workspace y ejecuta el script:

```bash
cd ~/ros2_jazzy/Lab5/Codigos/phantom_ws
python3 mis_actividades/actividad7.py
```

## 🎯 Interfaz y Uso
1. **Selección Rápida:** Utiliza el menú desplegable en la sección "Configuraciones del Laboratorio" para elegir una de las 5 configuraciones (Configuración 1 a 5).
2. **Validación y Justificación:** Al presionar **▶ EJECUTAR CONFIGURACIÓN**, el panel inferior (Análisis de Restricciones) te mostrará de inmediato:
   * Los valores teóricos.
   * Si la configuración es segura (se ejecuta tal cual).
   * Si requiere modificación (imprime la justificación del porqué y envía los valores truncados).
3. **Control de Velocidad Dinámico:** Utiliza el slider de Velocidad (1-1023) para ajustar qué tan rápido o lento quieres que el robot ejecute el movimiento simultáneo hacia el punto objetivo.
4. **Reporte Final:** Una vez el robot termina de moverse, la consola imprime un reporte comparando la posición que se deseaba alcanzar con la posición real que miden los motores.

## ⚠️ Herramientas de Seguridad
* **🚨 PARADA DE EMERGENCIA:** Un botón rojo gigante en la parte superior detiene de golpe el movimiento apagando el torque de los motores ante cualquier anomalía física.
* **Retorno Seguro:** Botón dedicado para volver de manera simultánea a la posición de HOME `[0, 0, 0, 0, 0]`.
* **Control Manual del Torque:** Botones dedicados de Torque ON y Torque OFF para intervenir el brazo de forma manual si es necesario.
# Actividad 8 - Movimiento Secuencial y Comparación Cinemática

## 📖 Descripción
Esta actividad implementa una Interfaz Gráfica (GUI) diseñada para ejecutar las mismas 5 configuraciones articulares de la Actividad 7, pero cambiando radicalmente el paradigma de control: **movimiento secuencial**.

Atendiendo a la guía de laboratorio, el programa fuerza al robot a alcanzar el punto final moviendo una sola articulación a la vez, estrictamente en el siguiente orden:
1. Base
2. Hombro
3. Codo
4. Muñeca
5. Pinza

El objetivo principal de esta interfaz es proveer las herramientas de medición (cronometraje exacto en consola) para que el usuario pueda comparar el movimiento secuencial versus el simultáneo (Actividad 7) en términos de **tiempo de ejecución, trayectoria del TCP (Tool Center Point) y suavidad**. Al igual que en la actividad anterior, cuenta con un sistema de validación automática que trunca y justifica cualquier valor que exceda los límites mecánicos seguros.

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
python3 mis_actividades/actividad8.py
```

## 🎯 Interfaz y Uso
1. **Configuraciones:** Utiliza el menú desplegable para elegir una de las 5 configuraciones del laboratorio.
2. **Control de Tiempo Secuencial:** A diferencia de la Act 7, aquí puedes definir el **"Tiempo por articulación (s)"**. Si lo ajustas a 1.5s, la secuencia completa tardará exactamente 7.5 segundos (1.5s x 5 articulaciones).
3. **Validación:** Al dar clic en **▶ EJECUTAR SECUENCIAL**, el panel verificará la viabilidad de la pose. Si detecta peligro de colisión, ajustará los ángulos a los límites de la Actividad 6 y te explicará el motivo.
4. **Ejecución y Comparación:** El robot comenzará a moverse eslabón por eslabón. Al terminar, la consola te entregará un resumen detallando el tiempo total exacto que tomó la operación, dato crucial para tu análisis comparativo.

## ⚠️ Herramientas de Seguridad
* **🚨 PARADA DE EMERGENCIA:** Detiene el flujo de la secuencia en cualquier punto y corta el torque de los motores inmediatamente.
* **Control de Torque:** Botones dedicados para habilitar o deshabilitar la rigidez de los motores manualmente.
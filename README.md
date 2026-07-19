# Laboratorio No. 05: Phantom X Pincher X100 – ROS 2 Jazzy – RViz 🤖
**Universidad Nacional de Colombia - Robótica**

## 📖 Descripción del Proyecto
Este repositorio contiene la implementación completa del Laboratorio No. 05. El proyecto abarca desde la calibración básica y extracción de parámetros de Denavit-Hartenberg (DH), hasta la implementación de Cinemática Inversa geométrica y el desarrollo de un motor de coreografía Data-Driven que sincroniza los movimientos del robot con procesamiento de audio en tiempo real (FFT).

Todo el sistema está diseñado mediante Interfases Gráficas (GUIs) construidas con `tkinter`, permitiendo un control bidireccional fluido tanto en simulación (RViz) como con el hardware físico (servomotores Dynamixel).

---

## 🏗️ Arquitectura y Relación de Archivos
El proyecto está estructurado de manera modular. Ninguna actividad funciona de manera aislada; todas dependen de un núcleo central de comunicaciones y de parámetros guardados progresivamente.

*   **El Cerebro (Middleware):** `joint_selector.py`
    Es el nodo base de ROS 2. Traduce los comandos de las GUIs a trayectorias articulares (`JointTrajectory`) y lee los encoders en tiempo real. Todas las actividades (5 a 15) importan este script para poder mover el robot.
*   **Fase 1 - Calibración y Seguridad (Actividades 5 y 6):** 
    Establecen los márgenes operativos. La Actividad 6 genera un archivo `limites_seguros.json` que el `joint_selector.py` lee automáticamente para proteger los motores de colisiones físicas.
*   **Fase 2 - Cinemática y Control (Actividades 7 a 12):** 
    Implementan movimientos simultáneos, secuenciales, interpolación (lineal y cúbica) y resuelven la cinemática directa (DH) e inversa para posicionamiento cartesiano.
*   **Fase 3 - Enseñanza y Trazado (Actividades 13 y 14):** 
    Uso del robot como *Teach Pendant* guardando secuencias en `.yaml` (Act 13) y como plotter tridimensional en RViz (Act 14).
*   **Fase 4 - Reto Final (Actividad 15 y Analizador de Audio):** 
    El script `analizador_audio.py` extrae la energía y frecuencia de pistas musicales usando la Transformada Rápida de Fourier, generando archivos `.csv`. La Actividad 15 consume estos datos para ejecutar una coreografía autónoma sincronizada con la música.

---

## 🛠️ Configuración del Entorno (Setup)

Para garantizar la estabilidad del sistema, las dependencias se dividen en dos niveles: las globales del sistema operativo (para evitar romper ROS 2) y las específicas del procesamiento de señales musicales.

### 1. Dependencias Globales del Sistema (Ubuntu / ROS 2)
Instala las herramientas matemáticas, gráficas y de ROS 2 usando el gestor de paquetes del sistema:
```bash
sudo apt update
sudo apt install python3-tk python3-numpy python3-matplotlib python3-yaml ros-jazzy-visualization-msgs
```

### 2. Entorno Virtual (Procesamiento de Audio)
Para aislar las librerías complejas de análisis musical (Reto Final), crea y configura un entorno virtual dentro del workspace:
```bash
cd ~/ros2_jazzy/Lab5/Codigos/phantom_ws
python3 -m venv mi_entorno
source mi_entorno/bin/activate
pip install librosa pandas pygame pillow
```

---

## 🚀 Guía de Ejecución

Antes de ejecutar cualquier script, debes compilar tu workspace (si hay cambios en el URDF) y cargar las variables de entorno de ROS 2.

**1. Preparar las terminales:**
Abre una terminal y lanza el controlador del robot (o RViz):
```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_jazzy/Lab5/Codigos/phantom_ws
source install/setup.bash
# Lanza tu archivo .launch.py aquí (Ej: ros2 launch phantomx_description display.launch.py)
```

**2. Ejecutar las Actividades:**
En una segunda terminal, activa tu entorno virtual y ejecuta la actividad deseada:
```bash
source /opt/ros/jazzy/setup.bash
cd ~/ros2_jazzy/Lab5/Codigos/phantom_ws
source install/setup.bash
source mi_entorno/bin/activate

# Ejemplo para ejecutar la Actividad 12 (Cinemática Inversa):
python3 mis_actividades/actividad12.py
```

---

## ⚠️ Notas de Seguridad Importantes
*   **Parada de Emergencia:** Todas las GUIs de este repositorio cuentan con un botón rojo de Emergencia. Al presionarlo, se anula instantáneamente el torque de los motores y se destruyen los bucles de movimiento.
*   **Operación Física:** Mantén siempre presionado o al alcance el botón de detención de la fuente de poder del Phantom X Pincher durante las primeras pruebas de coreografía cartesiana.
*   **RViz:** El archivo de configuración de RViz (`.rviz`) se encuentra en el repositorio. Cárgalo para visualizar correctamente los trazados cartesianos de la Actividad 14 y el seguimiento del TCP.
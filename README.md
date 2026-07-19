# Laboratorio No. 05: Phantom X Pincher X100 – ROS 2 Jazzy – RViz 🤖
**Universidad Nacional de Colombia - Robótica**

## 📖 Descripción del Proyecto
En el presente repositorio se expone la implementación completa correspondiente al Laboratorio No. 05. El desarrollo abarca desde la calibración básica y extracción de parámetros de Denavit-Hartenberg (DH), hasta la solución de la cinemática inversa geométrica y la construcción de un motor de coreografía guiado por datos (Data-Driven), mediante el cual se sincronizan los movimientos del manipulador robótico con el procesamiento de señales de audio en tiempo real (FFT).

El sistema completo ha sido diseñado utilizando interfaces gráficas de usuario (GUIs) implementadas mediante la librería `tkinter`, a través de las cuales se permite un control bidireccional fluido tanto en el entorno de simulación (RViz) como en el hardware físico (servomotores Dynamixel).

---

## 🏗️ Arquitectura y Relación de Archivos
La estructuración del proyecto se ha realizado de manera modular. Las actividades no operan de forma aislada; por el contrario, todas se encuentran subordinadas a un núcleo central de comunicaciones y al registro progresivo de parámetros.

*   **El Cerebro (Middleware):** `joint_selector.py`
    Constituye el nodo base de ROS 2. A través de este script, los comandos emitidos por las GUIs son traducidos a trayectorias articulares (`JointTrajectory`) y se realiza la lectura de los encoders en tiempo real. Este nodo es importado por la totalidad de las actividades (5 a 15) para habilitar el control cinemático del robot.
*   **Fase 1 - Calibración y Seguridad (Actividades 5 y 6):** 
    Se establecen los márgenes operativos del sistema. Mediante la Actividad 6, se genera un archivo `limites_seguros.json`, el cual es leído de manera automática por `joint_selector.py` con el fin de proteger los motores contra posibles colisiones físicas.
*   **Fase 2 - Cinemática y Control (Actividades 7 a 12):** 
    Se implementan movimientos simultáneos, secuenciales e interpolación (lineal y cúbica). Adicionalmente, se da solución a la cinemática directa (DH) e inversa para el posicionamiento en el espacio cartesiano.
*   **Fase 3 - Enseñanza y Trazado (Actividades 13 y 14):** 
    El manipulador es utilizado como *Teach Pendant*, permitiendo el guardado de secuencias en archivos `.yaml` (Actividad 13) y empleándose como graficador tridimensional en el entorno de RViz (Actividad 14).
*   **Fase 4 - Reto Final (Actividad 15 y Analizador de Audio):** 
    Mediante el script `analizador_audio.py`, se extrae la energía y la frecuencia de pistas musicales utilizando la Transformada Rápida de Fourier, generándose los datos en archivos `.csv`. Esta información es consumida por la Actividad 15 para la ejecución de una coreografía autónoma sincronizada con la pista de audio.

---

## 🛠️ Guía de Instalación desde Cero (Setup)

### 🛑 Prerrequisito Fundamental: ROS 2 Jazzy
Para el correcto funcionamiento de este proyecto, **es obligatorio contar con una instalación previa de ROS 2 Jazzy en el sistema (Ubuntu 24.04)**. 
En caso de no disponer de dicha instalación, se debe seguir la guía oficial del laboratorio mediante el siguiente enlace:
👉 [Instalación de ROS 2 Jazzy (LabSIR-UN)](https://github.com/labsir-un/02_Rob_2026_I_Intro_ROS2_Jazzy)

Una vez configurado ROS 2 Jazzy, se puede proceder con los siguientes pasos de configuración.

---

### Paso 1: Descarga y organización de los archivos (Importante)
Para garantizar la correcta ejecución de los comandos, los archivos deben ser ubicados en una ruta específica. Se dispone de dos alternativas para la obtención del proyecto:

**Opción A: Descarga mediante archivo .ZIP (Método recomendado para usuarios sin Git)**
1. En la parte superior del repositorio en GitHub, se debe hacer clic en el botón verde **"<> Code"** y seleccionar **"Download ZIP"**.
2. El archivo descargado debe ser ubicado en el directorio de *Descargas* y posteriormente descomprimido (clic derecho -> *Extraer aquí*). 
3. Como resultado de la descompresión, se generará una carpeta denominada `Laboratorio_05_Robotica-main`. 
4. **Se debe ingresar a dicho directorio.** En su interior, se encontrará una subcarpeta llamada `Laboratorio5` junto con otros documentos adjuntos (como este archivo README y documentación en formato PDF).
5. **TODOS** los archivos contenidos dentro del directorio `-main` deben ser copiados y **pegados directamente en el directorio personal del sistema (Directorio Home o `~`)**. 
   *(Nota: No se deben mantener los archivos dentro de la carpeta de Descargas ni dentro del directorio "-main", dado que los comandos de configuración de rutas presentarán fallos).*

**Opción B: Clonación mediante Git**
Se debe abrir una terminal de comandos y ejecutar las siguientes instrucciones para descargar y ubicar los archivos directamente en el directorio Home:
```bash
cd ~
git clone [https://github.com/DavidPirateque/Laboratorio_05_Robotica.git](https://github.com/DavidPirateque/Laboratorio_05_Robotica.git)
mv Laboratorio_05_Robotica/* ~
rm -rf Laboratorio_05_Robotica
```

### Paso 2: Instalación de Dependencias del Sistema
Se requiere la instalación de diversas herramientas matemáticas, gráficas y de visualización operativas bajo ROS 2. Para ello, se debe ejecutar el siguiente bloque en la terminal:
```bash
sudo apt update
sudo apt install python3-tk python3-numpy python3-matplotlib python3-yaml python3-venv ros-jazzy-visualization-msgs
```

### Paso 3: Compilación del Espacio de Trabajo (Workspace)
La totalidad del código fuente está contenida en el espacio de trabajo `phantom_ws` (ubicado ahora dentro de la carpeta `Laboratorio5` en el directorio Home). Es estrictamente necesario compilar el proyecto antes de su primer uso.
En la terminal, se deben ejecutar los siguientes comandos:
```bash
# Ingreso al espacio de trabajo
cd ~/Laboratorio5/phantom_ws

# Carga de la instalación base de ROS 2
source /opt/ros/jazzy/setup.bash

# Compilación del proyecto
colcon build
```

### Paso 4: Creación del Entorno Virtual (Para Procesamiento de Señales)
Con el objetivo de prevenir conflictos en las librerías dependientes de ROS 2, se debe configurar un entorno virtual aislado dedicado exclusivamente al procesamiento de las librerías musicales del Reto Final.
En la misma terminal (verificando que la ruta actual sea `~/Laboratorio5/phantom_ws`), se debe ejecutar:
```bash
# Creación del entorno virtual
python3 -m venv mi_entorno

# Activación del entorno virtual
source mi_entorno/bin/activate

# Instalación de dependencias de audio e interfaz gráfica
pip install librosa pandas pygame pillow
```

---

## 🚀 Guía de Ejecución

Para la ejecución de los algoritmos del proyecto, se requiere la apertura simultánea de **dos terminales**. Ambas deben ser direccionadas al espacio de trabajo previamente compilado.

**Terminal 1: Lanzamiento del Robot (Simulación o Hardware Físico)**
```bash
# 1. Navegación al espacio de trabajo
cd ~/Laboratorio5/phantom_ws

# 2. Carga del entorno de ROS 2 y del workspace compilado
source /opt/ros/jazzy/setup.bash
source install/setup.bash

# 3. Lanzamiento del controlador (Ejemplo para simulación visual en RViz)
ros2 launch phantomx_pincher_description display.launch.py
```

**Terminal 2: Ejecución de Actividades y GUIs**
```bash
# 1. Navegación al espacio de trabajo
cd ~/Laboratorio5/phantom_ws

# 2. Carga del entorno de ROS 2 y del workspace compilado
source /opt/ros/jazzy/setup.bash
source install/setup.bash

# 3. Activación del entorno virtual de Python
source mi_entorno/bin/activate

# 4. Ejecución del script deseado (Ejemplo: Actividad 12)
python3 mis_actividades/actividad12.py
```

---

## ⚠️ Consideraciones de Seguridad
*   **Parada de Emergencia:** Todas las interfaces gráficas desarrolladas en este repositorio están equipadas con un botón de parada de emergencia. Al ser activado, el torque de los actuadores es anulado instantáneamente y los bucles de ejecución de trayectoria son interrumpidos.
*   **Operación Física:** Se recomienda estrictamente mantener presionado, o al alcance inmediato, el interruptor de la fuente de alimentación del Phantom X Pincher durante las pruebas iniciales de control espacial.
*   **Visualización en RViz:** El archivo de configuración correspondiente a RViz (`.rviz`) se encuentra adjunto en el repositorio. Este debe ser importado en el simulador para asegurar la correcta visualización de los trazados cartesianos durante la Actividad 14 y el monitoreo del punto central de la herramienta (TCP).

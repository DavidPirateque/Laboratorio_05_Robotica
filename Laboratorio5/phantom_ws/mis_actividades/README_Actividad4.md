# Actividad 4: Interfaz de Control Cinético (GUI)

Esta actividad implementa una interfaz gráfica en Tkinter para controlar de forma manual y simultánea las articulaciones del PhantomX.

## 🛠️ Requisitos
* Python 3 y `tkinter` (incluido en la mayoría de distribuciones de Linux).
* El simulador principal de ROS 2 debe estar corriendo.

## ▶️ Cómo ejecutar
En una terminal nueva, carga el entorno y ejecuta el script:
```bash
source ~/ros2_jazzy/Lab5/Codigos/phantom_ws/install/setup.bash
python3 ~/ros2_jazzy/Lab5/Codigos/phantom_ws/mis_actividades/actividad4.py
```

## 🧠 Funcionamiento
El script `joint_selector.py` actúa como nodo backend suscribiéndose a `/joint_states` y publicando trayectorias a los controladores del brazo (`/joint_trajectory_controller/...`) y la pinza (`/gripper_trajectory_controller/...`). La interfaz gráfica en `actividad4.py` se comunica con este nodo para enviar los comandos de los sliders.
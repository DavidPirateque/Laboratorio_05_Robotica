# Actividad 5 - Calibración de Cero y Error Articular

## 📖 Descripción
Esta actividad implementa un **Asistente Gráfico de Calibración** interactivo para el robot Phantom X Pincher X100. Su objetivo es evaluar la precisión de los servomotores Dynamixel enviando posiciones conocidas y midiendo la respuesta física real del hardware.

El script cumple con los lineamientos de calibración:
1. Envía cinco posiciones angulares distribuidas dentro del rango seguro de cada articulación.
2. Registra la posición solicitada (Deseada) y la reportada por el servomotor (Medida).
3. Calcula el error articular matemático: $e_q = q_{deseado} - q_{medido}$
4. Determina automáticamente el error máximo, el error promedio y el desplazamiento de cero.
5. Genera de forma autónoma una gráfica comparativa guardada en el directorio local.

## 🛠️ Requisitos
En Ubuntu con ROS 2 Jazzy, asegúrate de tener instaladas las dependencias del sistema usando `apt` para evitar conflictos de entorno:

```bash
sudo apt update
sudo apt install python3-matplotlib python3-numpy python3-tk
```

## 🚀 Ejecución
Ubícate en tu workspace y ejecuta el nodo gráfico:

```bash
cd ~/ros2_jazzy/Lab5/Codigos/phantom_ws
python3 mis_actividades/actividad5.py
```

## 🎯 Interfaz y Uso
1. **Selección:** Al abrir la interfaz, selecciona la articulación que deseas evaluar desde el menú desplegable (Base, Hombro, Codo, Muñeca, Pinza).
2. **Lectura en Vivo:** El monitor verde mostrará la posición actual del motor en tiempo real.
3. **Iniciar Prueba:** Haz clic en "INICIAR PRUEBA DE 5 PUNTOS". El robot se moverá de forma autónoma a 5 puntos estratégicos definidos en el código.
4. **Análisis:** La tabla central se poblará con las columnas correspondientes. La sección de Estadísticas mostrará el Error Máximo, Error Promedio y Desplazamiento de Cero.
5. **Gráficas:** Al finalizar, el script creará automáticamente una carpeta llamada `graficas_act5` dentro del mismo directorio y guardará una imagen `.png` trazando la curva "Ideal" vs "Real".

## ⚠️ Seguridad
La interfaz cuenta con un sistema de paro de emergencia (Botón Rojo) que corta inmediatamente el torque de los motores sin cerrar el programa, permitiendo proteger la integridad del Phantom X en caso de un comportamiento errático.
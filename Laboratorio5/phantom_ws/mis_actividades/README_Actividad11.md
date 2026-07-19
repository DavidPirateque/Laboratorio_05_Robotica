# Actividad 11 - Cinemática Directa (Parámetros DH)

## 📖 Descripción
Esta actividad implementa la Cinemática Directa del robot Phantom X Pincher X100 utilizando la convención estándar de matrices de Denavit-Hartenberg (DH). 

Tal como se solicita en la guía de laboratorio, el programa recibe los valores angulares de las cuatro articulaciones del brazo ($q_1, q_2, q_3, q_4$) y calcula la posición espacial cartesiana ($x, y, z$) junto con la orientación de la pinza mediante ángulos de Euler ($roll, pitch, yaw$). La interfaz incluye un acceso rápido a las 5 configuraciones de la Actividad 7, permitiendo al usuario realizar el cálculo teórico y enviar simultáneamente el comando al robot para comparar matemáticamente el resultado contra la posición visual observada en RViz.

## 🛠️ Requisitos
El cálculo matricial avanzado requiere la librería `numpy`. Al estar en Ubuntu con ROS 2 Jazzy, instala las dependencias mediante el gestor de paquetes del sistema para mantener la integridad del entorno:

```bash
sudo apt update
sudo apt install python3-tk python3-numpy
```

## 🚀 Ejecución
Abre una terminal en tu workspace y ejecuta el script de la interfaz:

```bash
cd ~/ros2_jazzy/Lab5/Codigos/phantom_ws
python3 mis_actividades/actividad11.py
```

## 🎯 Interfaz y Flujo de Trabajo
1. **Configuraciones de Prueba (Act. 7):** Utiliza el menú superior para cargar instantáneamente cualquiera de las 5 configuraciones predefinidas del laboratorio. Al hacer clic en "Cargar Configuración", los valores llenarán las casillas inferiores automáticamente.
2. **Entrada Manual:** Si lo prefieres, ingresa valores personalizados para $q_1, q_2, q_3$ y $q_4$ en los selectores numéricos. La interfaz restringe matemáticamente las entradas a los límites seguros definidos en la Actividad 6.
3. **Cálculo y Movimiento:** Al presionar **▶ MOVER Y CALCULAR CINEMÁTICA**, el script realiza dos acciones en paralelo:
   * Calcula la multiplicación de matrices de transformación homogénea ($T_{0}^{4} = T_{0}^{1} \cdot T_{1}^{2} \cdot T_{2}^{3} \cdot T_{3}^{4}$) y extrae los datos.
   * Envía la configuración articular al robot físico/simulado.
4. **Análisis de Resultados:** El panel inferior "Resultados Teóricos" actualizará inmediatamente las coordenadas $X, Y, Z$ (mostradas en milímetros para mayor legibilidad) y la orientación $Roll, Pitch, Yaw$ (en grados). 
5. **Validación Visual:** Compara estos valores teóricos impresos en la interfaz con la posición del TCP (Tool Center Point) reportada visualmente por las grillas de RViz.

## ⚙️ Notas Matemáticas del Hardware
El código implementa un "Offset Geométrico" transparente para el usuario. Físicamente, el ángulo $0^\circ$ del hombro ($q_2$) del Phantom X apunta hacia arriba. Sin embargo, en la convención clásica de Denavit-Hartenberg, $0^\circ$ apunta hacia adelante (eje X). El script aplica automáticamente una resta de $90^\circ$ ($-\frac{\pi}{2}$ radianes) en la matriz $T_{1}^{2}$ para reconciliar el modelo matemático con la realidad mecánica.
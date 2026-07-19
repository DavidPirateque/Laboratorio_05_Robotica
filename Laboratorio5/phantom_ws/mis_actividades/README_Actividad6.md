# Actividad 6 - Determinación de Límites Seguros

## 📖 Descripción
Esta actividad implementa un **Asistente Visual Interactivo** para calibrar y determinar experimentalmente los límites seguros del robot Phantom X Pincher X100. 

Siguiendo estrictamente las directrices del laboratorio, el programa permite registrar manualmente las posiciones extremas de cada motor para calcular un rango de operación seguro. Genera automáticamente la tabla de parámetros (*Articulación, Límite inferior, Límite superior y Margen de seguridad*) y exporta esta información. En actividades futuras, estos datos se utilizarán para impedir a nivel de software el envío de valores ubicados fuera de estos límites y evitar colisiones contra los topes mecánicos.

## 🛠️ Requisitos
Al ser un sistema Ubuntu con ROS 2 Jazzy, las dependencias gráficas deben instalarse mediante los repositorios del sistema operativo (`apt`) para mantener limpio el entorno global:

    sudo apt update
    sudo apt install python3-tk

## 🚀 Ejecución
Ubícate en tu workspace de ROS 2 y ejecuta el script de la interfaz:

    cd ~/ros2_jazzy/Lab5/Codigos/phantom_ws
    python3 mis_actividades/actividad6.py

## 🎯 Interfaz y Flujo de Trabajo
1. **Torque OFF Automático:** Apenas se inicia la interfaz, el sistema apaga el torque de los Dynamixel de forma automática para permitirte mover el robot libremente con tus manos.
2. **Configuración del Margen:** En el panel global puedes ajustar el "Margen de seguridad" en grados (por defecto 5.0°). Este valor se restará/sumará a los extremos que registres para crear el "colchón" de seguridad.
3. **Calibración Manual por Articulación:**
   * Selecciona la articulación en el menú desplegable (Base, Hombro, Codo, Muñeca, Pinza).
   * Mueve el eslabón físicamente con tu mano cerca de su tope mecánico y haz clic en **📸 REGISTRAR EXTREMO 1**.
   * Mueve el eslabón hacia el lado opuesto y haz clic en **📸 REGISTRAR EXTREMO 2**.
4. **Tabla Dinámica:** Inmediatamente después de registrar ambos extremos, la tabla de Límites Seguros se llenará con los cálculos exactos.
5. **Exportación:** Cuando hayas registrado las 5 articulaciones, se habilitará el botón verde inferior. Al pulsarlo, se creará la carpeta `limites_act6` exportando los resultados consolidados en formatos `limites_seguros.csv` y `limites_seguros.json`.

## ⚠️ Herramientas de Seguridad
* **Reinicio de Motores:** Si en algún momento sientes resistencia en los motores, presiona "🔄 REINICIAR MOTORES (Torque OFF)" para liberarlos.
* **Monitor en Vivo:** Observa siempre el texto verde "Posición:" para asegurarte de que el encoder del motor está registrando tu movimiento manual en tiempo real antes de capturar el punto.
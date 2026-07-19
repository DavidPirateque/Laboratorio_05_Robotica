# Actividad 13 - Enseñanza y Repetición de Poses (Teach & Play)

## 📖 Descripción
Esta actividad transforma el computador en una "Consola de Aprendizaje" (Teach Pendant) industrial. 

Cumpliendo con los 7 requisitos de la guía, la interfaz permite un flujo de trabajo bidireccional completo:
1. **Modo Enseñanza (Teach):** Puedes mover el robot a una configuración deseada utilizando los controles deslizantes en pantalla, o presionar el botón "TORQUE OFF" para liberar los motores y mover el brazo físicamente con tus manos.
2. **Captura y Registro:** Una vez en la posición, le asignas un nombre y el programa guarda la configuración actual (extrayendo los ángulos de RViz o de los encoders reales del motor) en una lista secuencial visible.
3. **Persistencia (YAML):** La secuencia completa de poses puede exportarse y cargarse desde un archivo `.yaml` nativo, garantizando que el trabajo no se pierda al cerrar el programa.
4. **Modo Repetición (Play):** Puedes reproducir la secuencia completa en el orden registrado, modificando dinámicamente el tiempo de transición entre cada pose para que el movimiento sea rápido o extremadamente suave.
5. **Control en Vivo:** Durante la reproducción, un botón de detención permite pausar el ciclo de manera controlada o abortarlo mediante la Parada de Emergencia.

## 🛠️ Requisitos
Asegúrate de contar con la librería nativa para el manejo de archivos YAML en Ubuntu:

```bash
sudo apt update
sudo apt install python3-tk python3-yaml
```

## 🚀 Ejecución
Abre tu terminal en el workspace y arranca la consola de aprendizaje:

```bash
cd ~/ros2_jazzy/Lab5/Codigos/phantom_ws
python3 mis_actividades/actividad13.py
```

## 🎯 Interfaz y Flujo de Trabajo
1. **Enseñar al Robot:** 
   * Haz clic en **TORQUE OFF**. Mueve físicamente el Phantom X con tus manos a una posición.
   * Escribe un nombre en la casilla "Nombre Pose" (ej. `Aproximacion Pieza 1`).
   * Haz clic en **📸 CAPTURAR Y GUARDAR POSE ACTUAL**. Verás cómo la pose se añade a la lista secuencial de la derecha.
   * Repite el proceso para crear una trayectoria (ej. agarrar, subir, mover, soltar).
2. **Exportar a YAML:** Cuando tengas tu secuencia lista, haz clic en **💾 Guardar YAML**. El archivo se almacenará en la subcarpeta `yamls` de tu directorio `mis_actividades`.
3. **Reproducir Secuencia:** 
   * Haz clic en **TORQUE ON** para devolverle la fuerza a los motores.
   * Selecciona el "Tiempo de transición" en la zona de Playback (ej. 3.0 s).
   * Haz clic en **▶ REPRODUCIR SECUENCIA**. El robot ejecutará tu rutina completa.
4. **Simulación Pura:** Si no tienes el robot físico conectado, puedes usar los Sliders de la columna izquierda ("Control Manual de Articulaciones") para mover el modelo en RViz y capturar sus posiciones exactamente igual.

## ⚠️ Herramientas de Seguridad
* **🚨 PARADA DE EMERGENCIA:** Funcionalidad crítica que anula instantáneamente el torque de los motores e interrumpe el bucle de reproducción en progreso.
* **Freno Controlado:** El botón **⏹ DETENER REPRODUCCIÓN** no tira el robot al suelo; frena la rutina de manera controlada al completar la transición actual.
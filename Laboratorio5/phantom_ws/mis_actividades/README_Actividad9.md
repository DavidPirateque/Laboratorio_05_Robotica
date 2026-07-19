# Actividad 9 - Interpolación de Trayectorias (Lineal vs Cúbica)

## 📖 Descripción
Esta actividad implementa una Interfaz Gráfica (GUI) para evaluar y comparar la suavidad del movimiento del robot Phantom X Pincher utilizando dos métodos matemáticos diferentes: **Interpolación Lineal** e **Interpolación Cúbica**.

Atendiendo a los requerimientos de la guía del laboratorio, la aplicación permite al usuario definir dos configuraciones articulares alejadas (Pose A y Pose B). Al ejecutar la rutina, el programa sigue un orden estricto:
1. **Cálculo Matemático:** Genera los vectores de posición para ambas trayectorias y los exporta a un archivo `trayectorias_act9.csv`.
2. **Ejecución Física/Simulada:** Mueve el robot de Home a la Pose A, y luego interpola paso a paso hasta la Pose B (primero usando el método lineal, y luego repite el proceso usando el método cúbico), con pausas de 3 segundos para confirmar la llegada.
3. **Análisis Gráfico:** Únicamente tras finalizar la rutina física, despliega y guarda automáticamente una gráfica (`grafica_act9_todas_articulaciones.png`) que superpone ambas posiciones angulares en función del tiempo para comparar su suavidad.

## 🛠️ Requisitos
Para garantizar el correcto funcionamiento en Ubuntu con ROS 2 Jazzy y evitar conflictos con el entorno de Python administrado por el sistema, instala las dependencias mediante `apt`:

```bash
sudo apt update
sudo apt install python3-tk python3-numpy python3-matplotlib
```

## 🚀 Ejecución
Abre una terminal en tu workspace y ejecuta el script de la interfaz:

```bash
cd ~/ros2_jazzy/Lab5/Codigos/phantom_ws
python3 mis_actividades/actividad9.py
```

## 🎯 Interfaz y Flujo de Trabajo
1. **Configuración de Poses:** Define los ángulos de cada articulación para la **Pose A (Inicio)** y la **Pose B (Destino)**. El sistema carga por defecto dos poses alejadas sugeridas por la guía.
2. **Parámetros de Tiempo:** Ajusta el *Tiempo Total* que debe tomar el viaje entre la Pose A y B, y el *Paso (dt)*, que define la resolución temporal (frecuencia de muestreo) de la interpolación.
3. **Ejecución de la Rutina:** Presiona el botón verde central (**▶ CALCULAR, MOVER ROBOT Y LUEGO GRAFICAR**).
4. **Desarrollo Automático:** Observa la "Consola de Operaciones" integrada. El sistema te informará paso a paso mientras viaja a Home, evalúa la trayectoria Lineal, y luego la Cúbica, esperando la confirmación real de llegada en cada parada.
5. **Resultados:** Al concluir, se abrirá una ventana interactiva de Matplotlib mostrando el comportamiento de las 5 articulaciones, permitiéndote analizar visualmente cómo la interpolación cúbica suaviza los arranques y llegadas frente a la velocidad constante de la lineal.

## ⚠️ Herramientas de Seguridad
* **🚨 PARADA DE EMERGENCIA:** Un botón superior dedicado que corta de inmediato el torque de los motores e interrumpe el bucle de interpolación.
* **Control Manual:** Botones rápidos para apagar/encender el torque y retornar a Home (0°) de forma segura antes o después de la rutina.
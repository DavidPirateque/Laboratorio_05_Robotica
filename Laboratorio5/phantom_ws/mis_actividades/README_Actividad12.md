# Actividad 12 - Cinemática Inversa

## 📖 Descripción
Esta actividad resuelve el problema geométrico de la Cinemática Inversa para el robot Phantom X Pincher X100 mediante una Interfaz Gráfica interactiva.

Siguiendo al pie de la letra las exigencias de la guía de laboratorio, el programa permite al usuario ingresar una coordenada espacial objetivo ($x, y, z$) y un ángulo de ataque ($Theta$). Con esta información, el algoritmo:
1. **Calcula geométricamente las soluciones posibles** utilizando trigonometría espacial.
2. **Identifica y separa** las configuraciones resultantes en posturas de "Codo Arriba" (Elbow Up) y "Codo Abajo" (Elbow Down).
3. **Descarta inteligentemente** cualquier solución matemática que requiera que un motor exceda los límites seguros experimentales hallados en la Actividad 6.
4. **Alerta al usuario** a través de la consola si el punto solicitado está fuera del alcance físico del robot.
5. **Toma una decisión autónoma** seleccionando y ejecutando la solución válida cuya trayectoria requiera el menor desplazamiento articular desde la posición actual del robot.

La interfaz incluye 5 pruebas cartesianas pre-programadas de alta utilidad (como posturas de agarre vertical, horizontal y reposo alto) para evaluar el sistema de manera integral.

## 🛠️ Requisitos
Asegúrate de contar con la librería matemática y gráfica nativa de tu entorno Ubuntu con ROS 2 Jazzy:

```bash
sudo apt update
sudo apt install python3-tk python3-numpy
```

## 🚀 Ejecución
Sitúate en tu workspace y arranca la interfaz:

```bash
cd ~/ros2_jazzy/Lab5/Codigos/phantom_ws
python3 mis_actividades/actividad12.py
```

## 🎯 Interfaz y Flujo de Trabajo
1. **Pruebas Rápidas:** Utiliza el menú desplegable superior para cargar automáticamente coordenadas de prueba comunes. Al dar clic en "Cargar Coordenadas", los valores $X, Y, Z$ y $Theta$ se llenarán en los selectores.
2. **Coordenadas Personalizadas:** Puedes ignorar las pruebas rápidas e ingresar manualmente tus propios valores en milímetros y grados.
3. **Validación y Ejecución:** Presiona el gran botón verde **▶ CALCULAR INVERSA Y MOVER**. 
4. **Análisis de Consola:** Observa la sección inferior. El programa te mostrará el desglose de su proceso de toma de decisiones:
   * Te dirá qué posturas calculó (Codo Arriba/Abajo).
   * Te indicará cuáles son válidas y cuáles descartó (y el porqué).
   * Te informará cuál eligió ejecutar basándose en la distancia euclidiana articular.
5. **Control de Velocidad:** Utiliza el slider de velocidad para determinar qué tan fluido y rápido viaja el robot a la postura ganadora.

## ⚠️ Herramientas de Seguridad
* **🚨 PARADA DE EMERGENCIA:** Funcionalidad crítica que anula instantáneamente el torque de los motores e interrumpe cualquier comando espacial en progreso.
* **Comandos Directos:** Botones para liberar el torque y permitir mover el robot a mano, así como un atajo rápido para regresar a la pose HOME ($0^\circ$).
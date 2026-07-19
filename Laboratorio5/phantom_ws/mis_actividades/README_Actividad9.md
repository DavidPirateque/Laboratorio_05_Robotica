# Actividad 9: Interpolación de Trayectorias (Lineal vs Cúbica)

## 📖 Descripción
En la presente actividad se implementa una Interfaz Gráfica de Usuario (GUI) orientada a la evaluación y comparación de la suavidad en el desplazamiento del manipulador Phantom X Pincher X100, empleando para ello dos métodos matemáticos distintos: **Interpolación Lineal** e **Interpolación Cúbica**.

En estricto cumplimiento con los requerimientos de la guía de laboratorio, el sistema permite la definición de dos configuraciones articulares distantes (Pose A y Pose B). Al inicializar la rutina, el algoritmo ejecuta de forma secuencial las siguientes fases:
1. **Cálculo Matemático:** Se calculan los vectores de posición para ambas trayectorias, los cuales son exportados automáticamente al archivo local `trayectorias_act9.csv`.
2. **Ejecución Física y/o Simulada:** Se comanda el traslado del manipulador desde su posición de origen (Home) hacia la Pose A, para posteriormente ejecutar la interpolación discreta hacia la Pose B (evaluando primeramente el método lineal, seguido de una repetición bajo el método cúbico). Se incorporan pausas de 3 segundos para garantizar la confirmación de llegada y estabilización del sistema en cada parada.
3. **Análisis Gráfico:** Exclusivamente tras la culminación de la rutina física, se despliega y almacena de forma automática un gráfico (`grafica_act9_todas_articulaciones.png`), en el cual se superponen las trayectorias angulares en función del tiempo para facilitar el análisis comparativo de la suavidad cinemática.

## 🧮 Fundamentos Matemáticos e Implementación

### Modelo de Interpolación Lineal
La interpolación lineal establece una velocidad constante a lo largo del recorrido. Para una posición inicial $q_0$ y una posición final $q_f$ en un tiempo total $t_f$, la ecuación de posición angular está dada por:
$$q(t) = q_0 + \left( \frac{q_f - q_0}{t_f} \right) t$$
Si bien su cálculo computacional es de baja carga de procesamiento, este método genera discontinuidades en la derivada de la posición (velocidad) en los instantes de inicio ($t=0$) y llegada ($t=t_f$). Físicamente, esto se traduce en solicitaciones mecánicas bruscas (aceleraciones infinitas teóricas) que exigen los servomotores.

### Modelo de Interpolación Cúbica (Polinomial)
Con el objetivo de mitigar las fuerzas inerciales y garantizar arranques y paradas suaves, se impone que la velocidad inicial y final del manipulador sean nulas ($\dot{q}(0) = 0$ y $\dot{q}(t_f) = 0$). Para satisfacer estas 4 condiciones de frontera, se formula un polinomio de tercer grado:
$$q(t) = a_0 + a_1 t + a_2 t^2 + a_3 t^3$$
Al resolver el sistema de ecuaciones, el algoritmo calcula internamente los coeficientes para cada articulación de la siguiente manera:
$$a_0 = q_0$$
$$a_1 = 0$$
$$a_2 = \frac{3(q_f - q_0)}{t_f^2}$$
$$a_3 = -\frac{2(q_f - q_0)}{t_f^3}$$

### Arquitectura de Software
Computacionalmente, el algoritmo utiliza la librería `numpy` para discretizar el vector de tiempo total mediante el parámetro de paso de integración ($dt$) configurado por el usuario. Los arreglos espaciales resultantes son consolidados en matrices e iterados en un bucle de control. Durante la ejecución física, el script publica progresivamente los puntos de trayectoria hacia el nodo central (`joint_selector.py`), el cual empaqueta dichas coordenadas en mensajes de tipo `JointTrajectory` y efectúa su transmisión a los controladores de hardware nativos de ROS 2.

## 🛠️ Requisitos
Para garantizar la estabilidad del sistema en el entorno operativo Ubuntu bajo ROS 2 Jazzy, y prevenir conflictos con el gestor de paquetes de Python, se requiere la instalación de las dependencias a través del repositorio del sistema operativo (`apt`):

```bash
sudo apt update
sudo apt install python3-tk python3-numpy python3-matplotlib
```

## 🚀 Instrucciones de Ejecución
En una terminal independiente, se debe ingresar al espacio de trabajo, cargar el entorno y ejecutar el script correspondiente mediante las siguientes instrucciones:

```bash
# 1. Navegación al espacio de trabajo
cd ~/Laboratorio5/phantom_ws

# 2. Carga del entorno de ROS 2 y del workspace compilado
source /opt/ros/jazzy/setup.bash
source install/setup.bash

# 3. Ejecución de la interfaz de interpolación
python3 mis_actividades/actividad9.py
```

## 🎯 Interfaz y Flujo de Trabajo
1. **Configuración de Poses:** Es necesario definir los parámetros angulares de cada articulación correspondientes a la **Pose A (Inicio)** y a la **Pose B (Destino)**. El sistema inicializa por defecto con dos configuraciones espacialmente alejadas, sugeridas en la guía de práctica.
2. **Parámetros Temporales:** Se debe configurar el *Tiempo Total* proyectado para la transición entre las Poses A y B, así como el *Paso (dt)*, el cual determina la resolución temporal (frecuencia de discretización) del algoritmo de interpolación.
3. **Ejecución de la Rutina:** Se debe accionar el control principal de ejecución (**▶ CALCULAR, MOVER ROBOT Y LUEGO GRAFICAR**).
4. **Desarrollo Automático:** A través de la "Consola de Operaciones" integrada, el sistema reportará de manera progresiva el estado de ejecución: traslado a Home, evaluación de la trayectoria Lineal y posterior evaluación de la trayectoria Cúbica, realizando pausas de comprobación de estado estacionario durante cada parada.
5. **Resultados y Visualización:** Al finalizar el ciclo, se desplegará una ventana interactiva de Matplotlib ilustrando el comportamiento temporal de los 5 grados de libertad. Esto permite el análisis empírico y visual de la mitigación de las discontinuidades de velocidad (arranques y paradas bruscas) propia del perfil cúbico, en contraste con la velocidad constante del perfil lineal.

## ⚠️ Consideraciones de Seguridad
* **🚨 PARADA DE EMERGENCIA:** Se dispone de un control superior dedicado cuya activación suprime de forma inmediata el torque de los actuadores e interrumpe el bucle de interpolación de trayectoria.
* **Control Manual y Retorno:** Se incluyen comandos de acceso rápido para la habilitación o deshabilitación manual del torque (Torque ON/OFF) y para forzar un retorno seguro a la configuración de reposo (Home a 0°) de manera previa o posterior a la ejecución de la rutina.

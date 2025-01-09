# Trabajo de curso de Visión por Computador

## Autores - Grupo 21
- Héctor Wood Santana
- Alejandro Viera Ruiz

## Tabla de contenido

- [Motivación y propuesta](#motivación-y-propuesta)
- [Objetivo y funcionamiento](#objetivo-y-funcionamiento)
- [Tecnologías utilizadas](#tecnologías-utilizadas)
- [Implementación del programa](#implementación-del-programa)
- [](#fuentes-y-tecnologías-utilizadas)

## Motivación y propuesta

Se plantea la posibilidad de controlar un ordenador mediante gestos, captando el movimiento de las manos de un usuario y traduciendo las distintas posiciones en órdenes para el sistema operativo. Como base, se busca manejar el control de un videojuego ejecutado desde el dispositivo con gestos, para ello, nos apoyamos en los modelos de inteligencia artifical ya entrenados que **Google** pone a disposición del público a través del conjunto de bibliotecas de **mediapipe**. 

Inicialmente, se propone el uso de dos modelos presentes en esta biblioteca: 
- [*Gesture Recognizer*](https://mediapipe-studio.webapps.google.com/demo/gesture_recognizer?hl=es-419)
- [*Hand Landmark Detection*](https://mediapipe-studio.webapps.google.com/demo/hand_landmarker?hl=es-419)

La mayor diferencia entre estos dos modelos radica en el punto de partida para el desarrollo del programa. Mientras que el modelo *gesture recognizer* ya provee al desarrollador de una serie de gestos reconocidos, el modelo *hand landmark detection* parte de un nivel inferior, pero, a nuestro criterio, con mayor potencial.

Tras probar ambos modelos, pudimos verificar que era posible manejar los controles de un videojuego con cualquiera. Sin embargo, nos decantamos por *hand landmark detection* (en adelante HLD) debido a que nos da control de una inmensa cantidad de puntos que conforman la mano y vimos la posibilidad de implementar más funcionalidades. Por esta razón, finalmente nos decidimos a crear un programa **capaz de manejar el ordenador desde el escritorio, incluyendo periféricos como el ratón y pudiendo cambiar entre esquemas de control durante la ejecución.**

## Objetivo y funcionamiento

El funcionamiento esperado del programa es el siguiente:

1. Inicialización del modelo de detección de manos
2. Creación de ventana y obtención de información mediante cámara integrada o conectada.
3. Tratado de datos de posición de las manos.
4. Ejecución de acción correspondiente.

Dado que el programa detecta una serie de puntos de la mano, decidimos recoger los datos de las yemas de los dedos. Relacionamos la ventana con la pantalla del ordenador y traducimos el movimiento del dedo índice derecho en movimiento del ratón. Al juntar las yemas de los dedos corazón, anular y meñique con el pulgar, tenemos 3 posibles acciones, añadiendo la mano izquierda, que también puede *"pulsar"* con el dedo índice, **podemos mapear 7 gestos a diferentes comandos.** 

Por si no fuera suficiente, si reservamos una acción (en este caso, la pulsación entre meñique y pulgar derecho) como interruptor, podemos alternar entre 2 conjuntos de 6 acciones distintas además de controlar el ratón.

## Tecnologías utilizadas

Una vez concretado el alcance del proyecto. Seleccionamos las librerías que son indispensables para el desarrollo del trabajo. Estas serían:

- **Mediapipe:** base fundamental para la implementación de los controles.
- **PyAutoGUI:** esencial para traducir las acciones del usuario en movimiento del ratón, pulsaciones y ejecución de atajos (o *macros*).
- **Subprocess y psutil:** Librerías especializadas para ejecutar programas, obtener información y gestionarlos respectivamente.
- **time:** Útil para evitar solapamiento y proveer una mejor experiencia de usuario.

### Implementación del programa

Siguiendo un proceso muy similar al de prácticas anteriores, inicializamos el modelo HLD y establecemos el nivel de confianza en un 70%:
```python
mp_hands = mp.solutions.hands
hands = mp_hands.Hands(min_detection_confidence=0.7, 
                        min_tracking_confidence=0.7)
mp_draw = mp.solutions.drawing_utils
```

A continuación, creamos el objeto que contendrá las imágenes captadas por la camara y guardamos las dimensiones de la pantalla gracias a PyAutoGUI (función ``pyautogui.size``).

El siguiente paso, previo al tratado de datos, es la creación de una serie de variables que nos ayudarán a manejar el programa:
- flag: Para alternar entre los esquemas de control.
- proc_counter: Contador de procesos activos.
- proc_switch: Tratamiento de procesos.

Finalmente, nos adentramos en la lógica detrás del programa. Inicializamos la lectura de *frames*, visualizamos las imagenes que capta la cámara y procesamos la información con el modelo de mediapipe.

De esta forma, buscamos detectar las manos del usuario, si el modelo detecta alguna, es capaz de identificar de cual se trata, por lo que entramos en un proceso de seguimiento. 

De forma constante, si el usuario muestra la mano derecha, el índice se enlaza al ratón, pudiendolo mover libremente por la pantalla.

Si el pulgar de una de las manos se junta con cualquiera del resto de dedos, se ejecutará alguna de las siguientes acciones según el caso:

### Esquema 1:
- Mano izquierda:
    - Índice: Click izquierdo del ratón
    - Corazón: Click derecho del ratón
    - Anular: Desplazamiento de la pantalla hacia arriba
    - Meñique: Desplazamiento de la pantalla hacia abajo

- Mano derecha:
    - [Índice: Reservado para movimiento del ratón]
    - Corazón: Abrir y cerrar teclado virtual
    - Anular: Abrir y cerrar Calculadora
    - [Meñique: Alternar entre esquemas de control]

### Esquema 2:
- Mano izquierda:
    - Índice: Pulsar y arrastrar click izquierdo del ratón
    - Corazón: Apertura de bloc de notas y activación de reconocimiento de voz.
    - Anular: 
        - [Primera activación] Copiar selección
        - [Segunda activación] Pegar selección
    - Meñique: Apertura de buscador (Chrome) con URL personalizada.

- Mano derecha:
    - [Índice: Reservado para movimiento del ratón]
    - Corazón: Abrir y cerrar MS Paint
    - Anular: Abrir y cerrar Chrome con acceso directo ulpgc.es.
    - [Meñique: Alternar entre esquemas de control]

Para poder implementar estas funciones, implementamos una serie de funciones auxiliares para:
- Transformar las coordenadas de la imagen en coordenadas equivalentes en la pantalla, teniendo en cuenta sus dimensiones.
- Calcular la distancia entre dos puntos.

- Verificar si un proceso se encuentra activo.

- Cerrar procesos activos

### Fuentes

Para poder desarrollar este programa, ha sido necesario consultar la documentación de cada una de las librerías importadas:

- [Subprocess - Docs Python](https://docs.python.org/3.13/library/subprocess.html#module-subprocess)
- [Gesture Recognizer - AI Google Mediapipe](https://ai.google.dev/edge/mediapipe/solutions/vision/gesture_recognizer?hl=es-419)
- [Hand Landmarker - AI Google Mediapipe](https://ai.google.dev/edge/mediapipe/solutions/vision/hand_landmarker?hl=es-419)
- [PyAutoGUI - Read The Docs](https://pyautogui.readthedocs.io/en/latest/index.html)
- [Psutil - Read The Docs](https://psutil.readthedocs.io/en/latest/)
- [App status checking - Geeks For Geeks](https://www.geeksforgeeks.org/how-to-check-if-an-application-is-open-in-python/)

Adicionalmente, nos hemos apoyado en [ChatGPT,](https://openai.com/index/chatgpt/) para buscar alternativas y formas de optimizar el código, esto nos ha permitido identificar errores, posibles fallos de concepto y cálculos redundantes.

### Organización del equipo

Este periodo no ha estado exento de problemas. Lamentablemente, no contamos con uno de los ordenadores puesto que se había estropeado una de las últimas semanas de clase. Como resultado, mientras que ambos miembros del grupo picábamos código, éste se ejecutaba únicamente desde un ordenador.

No obstante, realizamos una gran labor de comunicación. Intercambiando ideas, proponiendo herramientas y resolviendo cada inconveniente que se presentaba. En total nos reunimos por medios telemáticos en 4 ocasiones y mantuvimos el dialogo de forma regular y constante por chat.

### Conclusión y pasos a seguir

Finalmente, presentamos un proyecto funcional, capaz de realizar hasta 14 acciones diferentes incluyendo el control en tiempo real de periféricos. 

En los últimos compases del proyecto, planteamos una refactorización del programa, la cual se encuentra [disponible](/Trabajo_Curso/app.ipynb) en el repositorio. Se intentó optimizar el programa, buscando el equilibrio entre rendimiento y eficiencia. Una de las medidas fue identificar y extraer aquellas variables constantes a lo largo de la ejecución. 

Como objetivo a futuro, queremos plantear nuevos esquemas, hacer uso de la librería os de python para obtener programas predeterminados y ejecutarlos en ciertas tareas como, por ejemplo, usar el navegador predeterminado de un usuario, registrado en HKEY_CURRENT_USER para abrir una URL. También nos gustaría probar la detección desde distintos ángulos de la cámara y aplicar mejoras, incluso mezclar el uso de este modelo con el de *gesture recognizer.*
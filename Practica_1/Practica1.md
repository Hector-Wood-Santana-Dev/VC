# Práctica 1 - Primeros pasos con OpenCV

A continuación se describe el desarrollo de la práctica por parte de los miembros del grupo 21.


## Tarea 1. Creación de una imagen con la textura de un tablero de ajedrez.

Para la realización de esta tarea, se genera un _array_ de tres dimensiones (800x800 y un canal de color) rellenado de ceros llamado ``tablero``, a continuación se recorre el ancho y altura de la imagen que forma este _array_ con dos bloques _for_ y se procede a intercambiar bloques de 0 por 1 según si la suma de los valores iterados es par o no. 

Finalmente, se usa la librería ``matplotlib.pyplot`` para mostrar por pantalla el resultado.

## Tarea 2. Crear una imagen estilo Mondrian

Se genera la imagen a partir de un _array_ como en la tarea anterior (haciendo uso del método ``numpy.zeros()``) pero con 3 canales de color (RGB). 

Se establece a 255 todos los canales de color para obtener un fondo blanco y se procede a modificar los valores en los canales deseados sobre distintos segmentos para obtener la imagen deseada.

## Tarea 3. Resuelve una de las tareas previas con las funciones de OpenCV

Buscando tener más soltura en el uso de la librería, se decidió resolver ambas tareas previas con las funciones de OpenCV. En primer lugar, el tablero de ajedrez mantiene la misma estructura, cambiando la modificación directa del _array_ por una llamada al método de dibujo ``cv2.rectangle()``.

Por otro lado, la imagen estilo Mondrian fue realizada a base de rectángulos y líneas de OpenCV.

## Tarea 4. Modifica de forma libre los valores de un plano de la imagen

Para esta práctica se ha usado la siguiente [imagen](Material_P1/music_cat.jpeg).

Se carga la imagen desde disco y se muestra mediante ``matplotlib``, se puede observar que se encuentra en formato BGR (por defecto de carga de OpenCV). Por lo que realizamos una conversión al formato RGB y procedemos a modificar planos de la imagen.

En primer lugar, se separan los distintos canales que forman la imagen ``cv2.split()``, se modifican los canales rojo y azúl y se establece el tipo para evitar valores que excedan el rango [0,255]. Finalmente se vuelve a generar la imagen con los canales modificados y se muestra por pantalla.

En esta ocasión hemos reducido la intensidad de los canales rojo y azúl, lo cual resulta en una mayor presencia del canal verde. Destacando sobre los otros dos.

## Tarea 5. Pintar círculos en las posiciones del píxel más claro y oscuro de la imagen.

Para dar con el píxel más claro y más oscuro, será necesario recorrer la imagen en búsqueda de los umbrales. Sin embargo, esto es tedioso en un formato de color, ya sea BGR o RGB. Por lo tanto, tras cargar la imagen, se convierte a un formato en escala de grises, donde sea más facil discernir las diferencias de intensidad.

A continuación, se realiza un recorrido por ancho y altura de la imagen y se almacenan los valores máximos y mínimos así como las posiciones donde se encuentran estos límites.

Finalmente, se hace uso de la función ``cv2.circle()`` para destacar estas posiciones. Siendo la zona más oscura redondeada de azúl y la zona más clara de verde.

## Tarea 6. Llevar a cabo una propuesta propia de pop art.

Para realizar nuestra propuesta de Pop Art, se ha decidido utilizar la entrada por cámara web ya que consideramos que es más entretenido. 

Para la tarea se han aplicado 4 filtros diferentes a la entrada de la cámara, haciendo uso de la función ```cv2.merge()`` y modificando los parámetros de los planos de forma que al multiplicarlos por el valor deseado en uno de los 3 canales, al normalizar el array ``(value).astype(np.uint8)`` logramos que ese canal quede cerca de ese valor, no importa su valor original. 

Por último se han colocado en una matriz 2x2 y se ha agrandado la imagen a 1.5 su tamaño original.  
# Práctica 5. Detección y caracterización de caras

A continuación se describe el desarrollo de la práctica por parte de los miembros del grupo 21.

Para la resolución de las distintas tareas se ha realizado un desglose en subtareas más simples para una mejor comprensión de las soluciones desarrolladas.

## Tarea:

En primer lugar se importan las librerías necesarias para realizar la práctica, además de los ficheros aportados como material, los cuales son necesarios para la detección de características faciales.

Estas librerías importadas son ``cv2, time, FaceDetectors y NumPy``. Además de las dependencias ``FaceNormalizationUtils``.

Una vez importadas las herramientas necesarias, se procede a comprobar las fuentes de video y se indican los modelos que serán usados para la detección de cara y de ojos. En esta ocasión, los modelos con mejores resultados han sido el 'DNN' y el 'DLIB68' respectivamente.

Se cargan las imágenes que se usarán más adelante como máscaras aplicadas al video. Siendo unas gafas y un sombrero. Comprobamos que se ha cargado correctamente.

Puesto que las imágenes tienen transparencia, se añade manualmente el canal alfa a las variables que contienen las imágenes.

![](/Practica_5/images/img_load.png)


A continuación, se da comienzo a la captura de video y se implementa la lógica detrás de los filtros.

Se ha decidido incluir tres modos de visualización además de la visualización del detector de caras por defecto. 

1. En el primer modo que puede ser accedido en ejecución pulsando la tecla '1', se añade la primera máscara, que consta de unas gafas posicionadas según la posición de los ojos detectados por el modelo 'DLIB68'.

2. En el segundo modo, se sigue un método similar al anterior. En este caso, se aplica la máscara en forma de sombrero, cuya posición es relativa al recuadro (_bounding box_) de la cara detectada por el modelo 'DNN'. Se accede pulsando la tecla '2'.

3. Finalmente, se aplican ambos métodos en el tercer modo de visualización, siendo una máscara doble. Este modo es accesible pulsando la tecla '3'.

Véase el gif de ejemplo a continuación:
![](/Practica_5/filter_used.gif)

## Fases de la implementación: 

Para aplicar las máscaras se ha seguido el siguiente procedimiento (con diferencias respecto a los posicionamientos para cada máscara).

### Reescalado de máscaras:

En primer lugar, se reescalan las imágenes que serán usadas posteriormente como máscaras, para coincidir con las dimensiones de la imagen. En el caso de las gafas, se usa de referencia la distancia entre los ojos que detecta el modelo 'DLIB68', mientras que en el caso del sombrero se usan las dimensiones del _bounding box_ de la cara detectada por el modelo 'DNN'.

## Posicionamiento:

Una vez se han reescalado las máscaras, se calcula la posición relativa de las máscaras, teniendo en cuenta las referencias anteriormente mencionadas.

- Gafas: 
![](/Practica_5/images/img_offset1.png)

- Sombrero:
![](/Practica_5/images/img_offset2.png)

Finalmente, se guardan los índices de los bordes y se actualiza la imagen añadiendo la máscara.


# Apuntes clase 03


## 1.- Cómo añadir imágenes a los archivos 
### Debemos crear una carpeta de imágenes y subirl el aechvo ahí

Para añadir imágenes, agregar signo de exclamación (!), seguido por texto alternativo en corchetes y la ubicación del archivo (que será nuestra carpeta de imágenes. 
### Ejemplo --> ![cualquier texto](./imagenes/imagen.jpg) 

Es buena práctica comenzar a escribir la ubicación del archivo con un punto+slash (./) que significa "aquí"

![aqui hay un gato](./imagenes/200.jpg)

# Arduino

Para usar las placas chinas tipo a es necesario descargar un 'crack' que se llama driver CH340

Funciona con el chip Atmega328P

Lo primero que debo hacer para programar en aeduino es saber si mis pines son outputs o inputs

El LED interno de arduino está conectado al Pin 13

Para encender el led hay que 'escribir en él se usa la función digitalWrite(numerodepin,estado0o1);

Un cable USB (Universal Serial Bus) tiene cuatro cables por dentro. Uno corresponde al Vcc (5V), GND (Tierra), Data + y Data -. Estas dos últimas patas hacen que la comunicación sea bidireccional.

Qué es un baudio? representa la cantidad de datos por segundo. 9600 es aprox. la velocidad del mouse del computador.

Serial.begin() es la función que inicia la comunicación serial, es decir, mantiene el arduino conectado al computador

"ALÉJENSE DE LOS POETAS"


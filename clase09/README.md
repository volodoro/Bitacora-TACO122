### Tips Clase anteproyectos 23.10.25

Lissajous figure ---> Osciloscopio
Módulo para reproducir mp3 ---> https://www.mercadolibre.cl/modulo-reproductor-mp3-con-lector-micro-sd-y-usb--arduino/up/MLCU30324120?matt_tool=16931662&utm_source=google_shopping&utm_medium=organic&pdp_filters=item_id%3AMLC440246708&from=gshop


# Ideas anteproyecto 



Herramientas ---> Processing, Pd, Video Feedback

Utilizar las visuales del feedback de video realizado en un televisor analógico para determinar el color de las letras en pantalla haciendo que processing detecte el color proyectado en video.
Referencia de las visuales que he logrado anteriormente con este método: https://www.youtube.com/watch?v=SNE7g9V4ggM

Referente: Jaime Cid - Colores minerales, una síntesis (obra audiovisual, 2025) ---> https://youtu.be/_Z9uiDLq_TU?si=aBgdQfzhamxI3q_b&t=2837. Lo que me interesa extraer de aquí es el método a través del cual se extrae la información de color de la imagen que luego es mapeada para controlar parámetros del sonido.

Podría hacer que los colores de cada letra (o cada palabra para hacerlo más fácil) varíen de forma independiente.

Mapear la info recogida desde el punto detector de colores en processing a info en Pure Data, me hace sentido usar un sonido de sintetizador tipo diente de sierra porque creo que es coherente con la estética de video analógico. Tal vez el parámetro obtenido desde processing pueda controlar la altura (pitch) de un acorde, y podría incorporar un secuenciador para generar cambios de acordes.

Podría usar la librería de video de processing para proyectar la imagen desde la cámara al televisor, y escriibir las letras sobre eso, las letras también serían parte del loop de feedback en ese caso.

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

# Imagen propuesta montaje:
![feedback2](imagenes/tele2.jpg)

Cámara Hi-8 ---> Processing in (Acá se incorporan las palabras) ----> Processing Out (De HDMI a Componente o Video Compuesto) ---> Tele CRT

# Processing
### Código de processing para extraer info de color a través de cámara


```processing
import processing.video.*;

Capture cam;

PVector pos;
PVector dir;
float velocidad = 1;

// Colores interpolados
float hueActual = 0;
float hueObjetivo = 0;
float sActual = 100;
float sObjetivo = 100;
float bActual = 100;
float bObjetivo = 100;

String[] versos = {
  "Todo está bien",
  "Todo está muy bien",
  "Todo está cada vez mejor"
};

void setup() {
  size(640, 480);
  colorMode(HSB, 360, 100, 100);
  textAlign(CENTER, CENTER);
  textSize(48);
  smooth();

  // --- Inicializar cámara ---
  String[] cameras = Capture.list();
  if (cameras.length == 0) {
    println("No se encontró cámara :(");
    exit();
  } else {
    cam = new Capture(this, cameras[0]);
    cam.start();
  }

  // --- Inicializar punto ---
  pos = new PVector(random(width), random(height));
  dir = PVector.random2D();
}

void draw() {
  // --- Actualizar cámara ---
  if (cam.available()) {
    cam.read();
  }

  // Mostrar imagen de cámara como fondo
  image(cam, 0, 0, width, height);
  // después de image(cam, 0, 0, width, height);

int rango = 5;
float hSum = 0, sSum = 0, bSum = 0;
int count = 0;

for (int dx = -rango; dx <= rango; dx++) {
  for (int dy = -rango; dy <= rango; dy++) {
    int x = constrain(int(pos.x) + dx, 0, width - 1);
    int y = constrain(int(pos.y) + dy, 0, height - 1);
    color cTemp = get(x, y);
    hSum += hue(cTemp);
    sSum += saturation(cTemp);
    bSum += brightness(cTemp);
    count++;
  }
}

float hProm = hSum / count;
float sProm = sSum / count;
float bProm = bSum / count;

hueObjetivo = hProm;
sObjetivo = sProm;
bObjetivo = bProm;


  // --- Movimiento del punto ---
  pos.add(PVector.mult(dir, velocidad));

  // Rebote en bordes
  if (pos.x < 0 || pos.x > width) dir.x *= -1;
  if (pos.y < 0 || pos.y > height) dir.y *= -1;

  // --- Obtener color del punto desde la cámara ---
  color c = get(int(pos.x), int(pos.y));
                 

  hueObjetivo = hue(c);
  sObjetivo = saturation(c);
  bObjetivo = brightness(c);

  // --- Interpolación suave ---
  hueActual = lerpHue(hueActual, hueObjetivo, 0.05);
  sActual = lerp(sActual, sObjetivo, 0.05);
  bActual = lerp(bActual, bObjetivo, 0.05);

  float hueContorno = (hueActual + 180) % 360;

  // --- Dibujar punto rojo (referencia visual) ---
  noStroke();
  fill(0, 100, 100);
  ellipse(pos.x, pos.y, 14, 14);

  // --- Dibujar texto con contorno complementario ---
  pushMatrix();
  translate(width/2, height/2 - 80);

  for (int i = 0; i < versos.length; i++) {
    float y = i * 70;
    float borde = 4; // grosor del contorno

    // Contorno simulado
    fill(hueContorno, 100, 100);
    for (float dx = -borde; dx <= borde; dx += borde) {
      for (float dy = -borde; dy <= borde; dy += borde) {
        if (dx != 0 || dy != 0) {
          text(versos[i], dx, y + dy);
        }
      }
    }

    // Texto principal
    fill(hueActual, 100, 100);
    text(versos[i], 0, y);
  }

  popMatrix();
}

// --- Interpolación de hue circular (para evitar saltos entre 359 y 0) ---
float lerpHue(float a, float b, float amt) {
  float diff = b - a;
  if (diff > 180) diff -= 360;
  if (diff < -180) diff += 360;
  return (a + diff * amt + 360) % 360;
}


```
# La teoría del caos

Al remontarse a finales del siglo XVII luego de que Isaac Newton postulara su segunda ley y su teoría de gravitación universal, pareciera que todo se volvió predecible y que en teoría seríamos capaces de indicar con certeza el movimiento de planetas y cometas con siglos de anticipación.

Determinismo total ---> El futuro ya está fijado, sólo debemos esperar que se manifieste ---> Demonio de Laplace, un ser con el conocimiento del comportamiento de todo el universo que a través de esa información descifra el pasado y el futuro.

## EL Caos

Edward Lorenz ---> En 1963 ejecutó un código para predecir las condiciones atmosféricas a partir de 12 variables que la computadora imprimía graficando una curva. Cuando Lorenz corrió el código una segunda vez, fue por un café mientras la computadora procesaba los datos, y al volver notó que la segunda curva tendía a lo mismo que la primera en un comienzo, sin embargo con el paso del tiempo empezaba a divergir hasta comportarse de manera completamente distinta a al primer resultado. Las diferencias de las curvas se debían a que en su segunda ejecución, Lorenz había ingresado solo tres decimales en lugar de seis con los que trabajó originalmente. Este fenómeno se denominó "sensibilidad a las condiciones iniciales".


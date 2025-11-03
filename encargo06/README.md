# Encargo 6

Acá se encuentran los códigos de Arduino y Processing que cumplen las funciones del encargo 6. Esto es, utilizar un encoder para alternar entre los versos de un poema ('Solo' de Nicanor Parra). Además
el color del texto y el fondo cambian utilizando el botón incorporado en el encoder.

```java
#define CLK 2
#define DT 3
#define SW 4

int contador = 0;
int CLK_last = 0;
int botonPrev = HIGH;

void setup() {
  Serial.begin(9600);
  pinMode(CLK, INPUT);
  pinMode(DT, INPUT);
  pinMode(SW, INPUT_PULLUP);
  CLK_last = digitalRead(CLK);
}

void loop() {
  int CLK_state = digitalRead(CLK);
  int DT_state = digitalRead(DT);

  // Detectar giro
  if (CLK_state != CLK_last) {
    if (DT_state != CLK_state) contador++;
    else contador--;

    // Loop infinito 0-74
    if (contador < 0) contador = 0;
    if (contador > 74) contador = 0; // reinicio al superar 74

    Serial.println(contador);
  }
  CLK_last = CLK_state;

  // Botón
  int botonActual = digitalRead(SW);
  if (botonPrev == HIGH && botonActual == LOW) {
    Serial.println("BOTON");
    delay(200); // debounce
  }
  botonPrev = botonActual;

  delay(1); // pequeño retraso para estabilidad
}

```

El código de Arduino de arriba envía los valores del encoder divididos en tres secciones de 25 unidades cada una. Cada sección se relaciona a un verso del poema. A partir de esto processing sabe qué verso
debe mostrar.

Código de processing:

```processing
import processing.serial.*;

Serial puerto;

int valorEncoder = 0;
int lineaActiva = 0;
int hueTexto = 60;
int lineaPrev = -1;

String lineasPoema[] = {"Poco a poco","Me fui quedando","Solo"};

int separacion = 50;
int totalLineasPantalla;
float numRepeticiones = 0;
float incremento = 0.2;
int intervalo = 30;
int ultimoTiempo = 0;

void setup() {
  size(400, 600);
  textAlign(CENTER, CENTER);
  textSize(36);
  colorMode(HSB, 360, 100, 100, 100);

  puerto = new Serial(this, "COM5", 9600);
  puerto.bufferUntil('\n');

  totalLineasPantalla = ceil(height / separacion);
}

void draw() {
  // --- Fondo con colores complementarios en realción al texto, saturación 100 y brillo 75 ---
  float satFondo = 100; // la mitad del máximo
  background(hueTexto + 180, satFondo, 75);

  // Determinar línea activa según valor del encoder
  if (valorEncoder < 25) lineaActiva = 0;
  else if (valorEncoder < 50) lineaActiva = 1;
  else lineaActiva = 2;

  if (lineaPrev != lineaActiva) {
    numRepeticiones = 0;
    lineaPrev = lineaActiva;
  }

  // Dibujar repeticiones de la línea
  String linea = lineasPoema[lineaActiva];
  float ultimaY = 0;
  for (int i = 0; i < floor(numRepeticiones); i++) {
    float y = i * separacion + separacion/2.0;
    if (y > height) break;
    float sat = map(i, 0, totalLineasPantalla - 1, 0, 100); // letras con saturación 0-100
    fill(hueTexto, sat, 100);
    text(linea, width/2, y);
    ultimaY = y;
  }

  // Triángulos acompañando la última línea
  if (numRepeticiones > 0) {
    float satUltima = map(floor(numRepeticiones)-1, 0, totalLineasPantalla - 1, 0, 100);
    fill(hueTexto, satUltima, 100);
    noStroke();
    float tamTriangulo = textAscent();

    triangle(width/2 - textWidth(linea)/2 - tamTriangulo, ultimaY,
             width/2 - textWidth(linea)/2 - tamTriangulo*0.5, ultimaY - tamTriangulo/2,
             width/2 - textWidth(linea)/2 - tamTriangulo*0.5, ultimaY + tamTriangulo/2);

    triangle(width/2 + textWidth(linea)/2 + tamTriangulo, ultimaY,
             width/2 + textWidth(linea)/2 + tamTriangulo*0.5, ultimaY - tamTriangulo/2,
             width/2 + textWidth(linea)/2 + tamTriangulo*0.5, ultimaY + tamTriangulo/2);
  }

  // Incrementar animación
  if (millis() - ultimoTiempo > intervalo) {
    numRepeticiones += incremento;
    if (numRepeticiones >= totalLineasPantalla) numRepeticiones = 0;
    ultimoTiempo = millis();
  }
}

// --- Leer datos seriales ---
void serialEvent(Serial puerto) {
  String dato = puerto.readStringUntil('\n');
  if (dato != null) {
    dato = trim(dato);
    if (dato.equals("BOTON")) {
      hueTexto += 60;
      if (hueTexto > 360) hueTexto = 60;
    } else {
      try {
        valorEncoder = int(dato);
      } catch(Exception e) {
        println("Error al leer número: " + e.getMessage());
      }
    }
  }
}

```

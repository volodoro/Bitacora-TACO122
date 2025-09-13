# Encargo 04

### Programar con la función 'if' dos o más rangos para valores del potenciómetro donde en cada uno el LED se comporte de distinta manera.


Le pedí ayuda a chatGPT para implementar lo que tenía en mente, que consistía en lo siguiente:

Dividir el rango del potenciómetro -desde 100 a 2000- en tres segmentos similares, donde en cada uno y según el siguiente orden, ocurre:

- Zona 1 (hasta 666) ---> Aquí lo que sucede es que el Led se enciende y se apaga con mayor frecuencia a medida que avanza el valor del potenciómetro.

- Zona 2 (hasta 1332) ---> Lo que aquí sucede es que aprovechamos la función de Pulse-Width-Modulation (PWM~) para hacer que el LED se encienda
  progresivamente desde lo más tenue a lo más intenso.
  
- Zona 3: Aquí lo que sucede es que según el valor de una variable declarada como "brillo" el LED realiza un fade in u out cuando ésta alcanza los valores 0 (mínimo) o 255 (máximo). Lo que modifica el valor de brillo es otra variable que se llama dir (por dirección), la cual suma o resta a brillo según lo que plantea la operación lógica del paréntesis  " (brillo <= 0 || brillo >= 255) ". 

  



```ruby
int pinLed = 6;
int potPin = A0;
int valorPot = 0;
int potMapeado = 0;
int intervalo = 1000;
bool estadoLed = 0;
unsigned long tiempoActual = 0;
unsigned long tiempoAnterior = 0;

// Variables para el fade
int brillo = 0;
int dir = 5; // velocidad del cambio en el fade

void setup() {
  pinMode(pinLed, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  tiempoActual = millis();
  valorPot = analogRead(potPin);
  potMapeado = map(valorPot, 0, 1023, 100, 2000);

  // --- Zona 1: Parpadeo normal ---
  if (potMapeado < 666) {
    intervalo = potMapeado; // velocidad depende del potenciómetro
    if (tiempoActual - tiempoAnterior > intervalo) {
      estadoLed = !estadoLed;
      tiempoAnterior = tiempoActual;
    }
    digitalWrite(pinLed, estadoLed);
  }

  // --- Zona 2: Brillo controlado por PWM ---
  else if (potMapeado < 1332) {
    int brilloPWM = map(potMapeado, 666, 1332, 0, 255);
    analogWrite(pinLed, brilloPWM);
  }

  // --- Zona 3: Fade in/out automático ---
  else if (potMapeado > 1400) {
    brillo += dir;
    if (brillo <= 0 || brillo >= 255) {
      dir = -dir; // invierte la dirección del fade
    }
    analogWrite(pinLed, brillo);
    delay(10); // ajusta la suavidad del fade
  }

  // Monitor serie para depuración
  Serial.print("PotMapeado: ");
  Serial.println(potMapeado);
}
```

Para clase 07 ---> 02/10/25

Usar el encoder con arduino y que su valor sea alojado en una variable

Hacer que en processing aparezcan números con palabras

El encoder no es un potenciómetro, es decir, no es una resistencia variable. Funciona en base a contactos metálicos 

como no traje el encoder, incorporé un potenciómetro y un botón pulsador.

Aquí está el código:

```processing
// Pines
const int pinPotenciometro = A0;   // Potenciómetro
const int pinBoton = 2;            // Botón pulsador

void setup() {
  Serial.begin(9600);

  // Configuración de pin del botón como entrada con resistencia pull-up interna
  pinMode(pinBoton, INPUT_PULLUP);
}

void loop() {
  // Leer valor del potenciómetro
  int valorPot = analogRead(pinPotenciometro);
  Serial.print("Valor del potenciometro: ");
  Serial.println(valorPot);

  // Leer estado del botón (LOW = presionado, HIGH = no presionado)
  int estadoBoton = digitalRead(pinBoton);
  if (estadoBoton == LOW) {
    Serial.println("Botón presionado!");
  }
  delay(100);
}
```
Para utilizar el encoder hay que utilizar una librería

# PROXIMA CLASE TRAER ACELERÓMETRO SOLDADO
# COMPRAR MOTOR PASO A PASO
# TRAER CABLES - MACHO/HEMBRA

MOTOR PASO A PASO.

Por qué se llama paso a paso? Por que cada vez que recibe una señal, avanza un 'paso' que será una cantidad concreta de grados.

El motor siempre necesitará un DRIVER para comunicarse con arduino, ya que no basta con arduino por sí solo para hacerlo funcionar.
El driver necesita usar cables hembra-macho

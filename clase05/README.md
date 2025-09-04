# Clase 05

### Array
 
Se puede explicar comparándolo con una cajonera? En el fondo es una sucesión de datos donde todos tienen asociados a un índice, al cual se puede acceder luego para así acceder a un dato específico

### LCD ----> Liquid Crystal Display
### OLED ----> 

La pantalla que estamos usando ahora es de 0.96 pulgadas (en su diagonal)
tiene 128x64 pixeles. Lo que significa que su relación de aspecto es de 2:1 


## Protocolos

x Ejemplo ---> USB (Universal Serial Bus)
La pantalla Usa el controlador SSD1306 y se comunica via interfax I2C (IIC), lo que simplifica el cableado y reduce la ocupación de pines

            IIC
[Arduino]---------[controlador SSD1306] ------[pantalla]

Cómo funciona IIC: Usa 4 pines

○ GND
○ VCC 
○ SDA
○ SCL

¿Cómo saber cuál es el SDA y el SCL?

El aeropuerto de Stgo se llama SCL, que es lo más grande cuando se ve la cordillera así que este será el A5
SDA (sea lo que sea) no es tan grande así que será el A4

SCL ---> S.Clock
SDA ---> S.Data

Estos pines permiten conectar la cantidad de módulos que sea solo con dos patas. Puedo conectar múltiples Clocks al pin A5 y múltiples Data al A4

Para usar una pantalla debemos descargar una biblioteca de Arduino. Una biblioteca es un conjunto de códigos que se pueden incorporar en nuestros códigos. En este caso usaremos la biblioteca Adafruit SSD1306

Para añadir imágenes en la pantalla podemos ir a la página 
        

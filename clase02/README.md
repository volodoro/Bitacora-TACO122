# Clase 02

## Processing 

Para acceder a la consola necesitamos comandos tipo "print"

ej print("aquí va el texto que se desea escribir");. Para comentar usar doble slash (//). ¿Qué es print dentro del universo de programación?

Es una función.

Todas las funciones de processing se escriben en forma funcion("argumento"). Ej: Print("hola mundo");

Cada línea debe cerrarse con un punto y coma, así el programa entiende cuándo termina una línea de comandos;

## Variables: Son números o datos.

Se crean siempre definiendo primero el tipo de variable. Una variable guarda información.

Por ej: 
// Si me compro un chicle en el super --> Me alcanza con un bolsillo para llevarlo

//si me compro 20 planchas de MDF --> necesito un flete

Para crear caracteres se usa la variable tipo char
Ej: 
char letra = 'a'
print(letra) ---> la consola deberia imprimir 'a'

en el caso del chicle seróa

bolsillo chile = bigtime;
cada vez que pregunte por el chicle. Para la variable char se debe escribir el caracter con las comillas individuales.

char tambien puede guardar números, pero los interpreta como ASCII, así que en verdad no sirve.

Para los números existe otro tipo de variable.
//int para declarar numeros enteros

ej: 

int a = 21;

print(a); me debería arrojar un 21 en la consola. La gracia de guardar números en variables es que puedo modificar las los números involucradas en un código, pero estructuralmente seguirá siendo lo mismo
pero con los nuevos valores.

Mati dijo "un github es como una constitución" y me pareció brillante.

int a = 17;
int b = 12;
int suma = 0;

suma = a + b;

print(suma);

las variables solo deben ser inicializadas o definidas una sola vez, cuando se vuelven a llamar, el computador ya sabe el tipo de variables.

###¿Por qué suma es 0 y después 29? 

Porque primero se definió, y al darle el varlos a + b el programa 'olvida' el valor anterior y se queda con el último que le dimos ANTES DE CADA PRINT. EL PRINT DICE UN MOMENTO DE LA VARIABLE!!!!!!

Existe un segundo tipo de variable numérica: float, que puede almacenar valores no enteros.

Hay una manera de convertir números de punto flotante en entero, y es usando "int" COMO FUNCIÓN, no como variable declarada.

Ej:

a = 6,1;
b = int(a);
print(a);

### Como hacer una linea en processing:

Con una función llamada line, donde debo decir la coordenada x e y de un extremo y la coordenada del otro. Se declara line(x1,y1,x2,y2);
ej:
line(10,20,80,60);

# ENCARGO 02
### Usar la frase del encargo pasado en processing de alguna manera

## Formas primitivas en processing

// Sobre las primitivas

//colores

//incestigar mouseX y mouseY




# Comunicación del Arduino con el ordenador

La comunicación serial es el método que utiliza Arduino para enviar y recibir información **bit a bit** a través de un solo canal.  
En lugar de transmitir varios bits al mismo tiempo (como ocurre en la comunicación paralela), los datos se envían en serie, de forma secuencial.

Este tipo de comunicación es **simple, eficiente y universal**, y permite que el Arduino se comunique con el ordenador mediante el **puerto USB**, que internamente actúa como una conexión **serial UART** (Universal Asynchronous Receiver-Transmitter).

## El objeto Serial

El objeto Serial representa la conexión UART integrada en la placa. Coniene distintos métodos para facilitar el envío y recepción de información.

# Métodos del objeto Serial

| Método | Descripción | Tipo de retorno | Ejemplo de uso |
|---------|--------------|------------------|----------------|
| `Serial.begin(velocidad)` | Inicia la comunicación serial con la velocidad indicada (en baudios). Es necesario llamarla en `setup()`. | `void` | `Serial.begin(9600);` |
| `Serial.end()` | Detiene la comunicación serial. Se usa raramente, pero puede liberar el puerto. | `void` | `Serial.end();` |
| `Serial.available()` | Devuelve el número de bytes disponibles para leer del búfer de entrada. | `int` | `if (Serial.available() > 0)` |
| `Serial.read()` | Lee el siguiente byte recibido. Si no hay datos, devuelve `-1`. | `int` | `char dato = Serial.read();` |
| `Serial.peek()` | Devuelve el siguiente byte recibido **sin eliminarlo** del búfer. | `int` | `char proximo = Serial.peek();` |
| `Serial.flush()` | Espera a que se complete el envío de los datos pendientes. | `void` | `Serial.flush();` |
| `Serial.print(valor)` | Envía datos al puerto serial **sin salto de línea**. Puede enviar texto, números o variables. | `size_t` | `Serial.print("Temperatura: ");` |
| `Serial.println(valor)` | Igual que `Serial.print()`, pero añade un salto de línea (`\r\n`). | `size_t` | `Serial.println(25.3);` |
| `Serial.write(valor)` | Envía datos en **formato binario** (bytes puros). Útil para comunicación con otros microcontroladores o dispositivos. | `size_t` | `Serial.write(0x41);` *(envía la letra A)* |
| `Serial.print(valor, base)` | Envía números en distintas bases: decimal (`DEC`), hexadecimal (`HEX`), octal (`OCT`), binaria (`BIN`). | `size_t` | `Serial.print(255, HEX); // FF` |
| `Serial.readBytes(buffer, longitud)` | Lee múltiples bytes del puerto y los guarda en un buffer. | `size_t` | `Serial.readBytes(datos, 10);` |
| `Serial.readString()` | Lee los caracteres recibidos hasta que haya un timeout y los devuelve como `String`. | `String` | `String texto = Serial.readString();` |
| `Serial.readStringUntil(caracter)` | Lee caracteres hasta que se encuentre un delimitador (por ejemplo, `'\n'`). | `String` | `String linea = Serial.readStringUntil('\n');` |
| `Serial.setTimeout(tiempo)` | Define el tiempo máximo de espera (en ms) para operaciones de lectura. | `void` | `Serial.setTimeout(2000);` |
| `Serial.find(cadena)` | Busca una cadena específica en los datos entrantes. Devuelve `true` si la encuentra. | `bool` | `if (Serial.find("OK")) { ... }` |
| `Serial.findUntil(cadena, fin)` | Busca una cadena hasta que se encuentre otra que la detenga. | `bool` | `Serial.findUntil("OK", "ERROR");` |
| `Serial.parseInt()` | Lee un número entero de los datos entrantes. | `long` | `int valor = Serial.parseInt();` |
| `Serial.parseFloat()` | Lee un número decimal (float) de los datos entrantes. | `float` | `float temp = Serial.parseFloat();` |
| `Serial.availableForWrite()` | Indica cuántos bytes libres quedan en el búfer de salida. | `int` | `if (Serial.availableForWrite() > 10)` |
| `Serial.attachInterrupt(handler)` *(solo en algunos modelos)* | Asigna una función que se ejecutará al recibir datos por serial. | `void` | `Serial.attachInterrupt(recibirDatos);` |
| `Serial.detachInterrupt()` | Desactiva la función asignada a la interrupción serial. | `void` | `Serial.detachInterrupt();` |


## Inicialización de la conexión

Antes de enviar o recibir datos, es necesario **inicializar la comunicación serial** dentro de la función `setup()` mediante el comando:

```c++
Serial.begin(9600);
```

El número 9600 indica la velocidad de transmisión en baudios (frecuencia de cambios de señal).

> [!NOTE]
> Las velocidades más comunes son 9600, 19200, 38400, 57600 y 115200 baudios. A mayor velocidad, mayor cantidad de datos por segundo, pero también mayor riesgo de errores si hay ruido o mala conexión.

## Envío de datos de Arduino a PC

El envío de información se realiza mediante los métodos `Serial.print()` y `Serial.println()`

``` c++
int c = 0;

void setup() {
  Serial.begin(9600);
}

void loop() {
  Serial.println("Contador " + String(c));
  delay(1000);
  c++;
}
```

En este ejemplo el Arduino envía un frase que concatena la palabra `Contador` con el valor de un contador que en cada ejecución del `loop()` incrementa su valor en uno.

> [!NOTE]
> Como se observa en el código la conversión entre `int` y `String` no se realiza de manera automática. En su lugar se utiliza un constructor de String para ello.

## Recepción de datos. Envío de PC a Arduino

Para la recepción de datos se utiliza una combinación del método `Serial.available()`, que inidica si hay bytes disponibles para leer, con otros como `Serial.read`, `Serial.readBytes()`, `Serial.readString()` o `Serial.parseInt()`.

``` c++
int option;
int led = 13;

void setup(){
  Serial.begin(9600);
  pinMode(led, OUTPUT); 
}

void loop(){
  //Comprobamos que haya más de 0 bytes a la espera de ser leidos
  if (Serial.available()>0){
    option=Serial.read();
    if(option=='0') {
      digitalWrite(led, LOW);
      Serial.println("OFF");
    }
    if(option=='1') {
      digitalWrite(led, HIGH);
      Serial.println("ON");
    }
  }
}
```

En el ejemplo el arduino se encuentra en espera activa. El controlador no tiene la capacidad de bloquearse esperando una lectura, o tener un listener, si no que dedica su procesamiento a esperar. 

> [!NOTE]
> Puesto que estamos utilizando C++ hay que tener precaución a la hora interpretar la información que se lee, en relación a su tipo de dato:
> 
> En el ejemplo:
> - Declaramos la variable de lectura como int
> - `Serial.read()` devuelve un número.
> - El número se corresponde con el valor ASCII del carcter transmitido.
> - Por lo que podemos comparar opción con el caracter '0' o con el número 48 y el resultado sería el mismo.

### Ejemplo
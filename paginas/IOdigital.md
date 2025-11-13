# Entradas y salidas digitales

Arduino dispone de **pines digitales** que pueden configurarse como **entradas o salidas** para interactuar con el entorno.  
Estos pines permiten **detectar señales** (por ejemplo si se ha presionado un pulsador o si se ha activado un interruptor) o **activar dispositivos** (como LEDs, relés o motores).

Los pines digitales solo pueden tener **dos estados posibles**:
- **HIGH (1 lógico)** → nivel alto (5V en la mayoría de placas (Como UNO), 3.3V en algunas).  
- **LOW (0 lógico)** → nivel bajo (0V o conexión a tierra).

Hay que tener en cuenta que en el mundo real las tensiones son continuas, y tienen infinitos valores entre los 0V y los 5V. En la lectura de valores se lleva a cabo un proceso de discretización en el que interviene una **tensión umbral**, que ronda el valor medio entre LOW y HIGH. Las tensiones por encima del umbral se trataran como **HIGH** y las que se encuentren por debajo como **LOW**.

> [!NOTE]
> En la realidad nada es perfecto y cuando hablamos de tensión umbral nos referimos a un rango de valores. Si hiciesemos una medición dentro de ese rango umbral, los valores obtenidos oscilarían azarosamente entre HIGH y LOW.

Los pines digitales pueden configurarse como entradas o como salida. Para ello se utiliza la función `pinMode(pin, MODO)`, a la que se le indicarán dos parámetros: el número del pin y el modo de trabajo. Los distintos modos se muestran en la tabla.
| Modo del pin           | Descripción breve                                                                 | Uso típico |
|-------------------------|----------------------------------------------------------------------------------|-------------|
| `INPUT`                 | Configura el pin como **entrada digital**. Permite leer señales externas (HIGH/LOW). | Leer pulsadores o sensores digitales. |
| `INPUT_PULLUP`          | Entrada digital con una **resistencia interna de pull-up** activada. El pin está en HIGH por defecto. | Leer pulsadores sin necesidad de resistencia externa. |
| `OUTPUT`                | Configura el pin como **salida digital**. Permite enviar señales HIGH o LOW. | Encender LEDs, activar relés o controlar dispositivos. |
|

## Salidas digitales

Las salidas digitales permiten que Arduino controle dispositivos externos enviando señales eléctricas que pueden estar en dos estados: HIGH (5 V) o LOW (0 V).

Con ellas se pueden encender y apagar LEDs, activar relés o motores, y en general comunicar al entorno las decisiones del programa.

Las salidas digitales, por lo general, no están diseñadas para otorgar potencia, solo para interactuar con los componentes. La intensidad que da un pin es de 40mA, aunque el valor recomendado es de 20mA, que es suficiente para encender un LED o activar un servo pequeño, pero no lo es para alimentar actuadores que requieran de mayor potencia. Para cargas mayores se empleará una etapa de amplificación con transistores y fuentes de alimentación externas al Arduino.

No es conveniente forzar la placa con potencias altas. Para respetar el límite de intensidad de 20mA, con un voltaje de 5V, la resistencia del dispositivo que queremos alimentar no debe ser inferior a 200Ω.

> [!NOTE]
> Siempre que no sepamos lo que estamos haciendo, deberemos conectar los dispositivos a cualquier salida a través de una resistencia de 300Ω.

Para configurar un pin como salida digital, en la función setup incluiremos una llamada a la función `pinMode()` donde configuraremos los pines correspondientes como `OUTPUT`.

``` c++
const int pin = 13;
 
void setup() {
  pinMode(pin, OUTPUT);  //definir pin como salida
}
```

Luego, para seleccionar si sale o no corriente por el pin, usaremos la función `digitalWrite()`, a la cual le indicaremos el pin en cuestión junto con el valor `HIGH` en caso de querer que salga corriente o `LOW` en caso contrario.

``` c++
// Ejemplo: Semáforo simple con tres salidas digitales

const int ledVerde = 8;
const int ledAmarillo = 9;
const int ledRojo = 10;

void setup() {
  // Configurar los pines como salidas
  pinMode(ledVerde, OUTPUT);
  pinMode(ledAmarillo, OUTPUT);
  pinMode(ledRojo, OUTPUT);
}

void loop() {
  // Verde encendido (avanza)
  digitalWrite(ledVerde, HIGH);
  digitalWrite(ledAmarillo, LOW);
  digitalWrite(ledRojo, LOW);
  delay(3000);

  // Amarillo encendido (precaución)
  digitalWrite(ledVerde, LOW);
  digitalWrite(ledAmarillo, HIGH);
  digitalWrite(ledRojo, LOW);
  delay(1000);

  // Rojo encendido (detente)
  digitalWrite(ledVerde, LOW);
  digitalWrite(ledAmarillo, LOW);
  digitalWrite(ledRojo, HIGH);
  delay(3000);

  // Amarillo encendido (precaución)
  digitalWrite(ledVerde, LOW);
  digitalWrite(ledAmarillo, HIGH);
  digitalWrite(ledRojo, LOW);
  delay(1000);
}
```

Montaje

![Semáforo](/imagenes/montajes/IOdigital/semaforo.png)

## Entradas digitales

Si la salida digital del Arduino nos permitía actuar en el mundo, las entradas nos facilitarán el leer su estado.

Por ejemplo, podemos realizar mediciones de temperatura, humedad, luminosidad, aceleración que nos servirán para tomar decisiones desde el código.

Pero antes de ponernos a realizar mediciones en sensores, empezaremos detectando la pulsación de un botón.

Lo primero será configurar un pin como entrada utilizando la función `pinMode()` y configurando el modo `INPUT`.

``` c++
const int pin = 8;
 
void setup() {
  pinMode(pin, INPUT);  //definir pin como entrada
}
```

El primer montaje que construiremos para la lectura de un pin es el siguiente.

![Conexión directa](/imagenes/esquemas/entrada1.png)

Sin embargo existe un problema. Cuando el pulsador está activado la corriente pasa desde los 5V hasta el pin de entrada, sin embargo, ¿Qué ocurre en reposo? En este estado el pin de entrada no recibe corriente por lo que no puede realizar una medición, se encuentra en un estado indeterminado.

Podemos mejorar la solución, conectando el pin de 5V a tierra y haciendo la medición en el medio, de la siguiente forma.

![Cortocircuito](/imagenes/esquemas/entrada2.png)

En esta situación, con circuito abierto, el pin de lectura está conectado a tierra, recibiendo 0V mientras que cuando el circuito se cierra recibe los 5V.

El problema de esta solución es que con el circuito cerrado el pin de 5V pasa sin resitencia a tierra, lo que provoca un **cortocircuito**.

El montaje correcto pasaría por incluir una resistencia para que los pines de 5 y 0 voltios no esten conectados directamente, de la siguiente manera.

![Entrada correcta](/imagenes/esquemas/entrada3.png)

En este caso el pin de lectura recibe `LOW` cuando el pulsador está abierto y recibe `HIGH` con el pulsador cerrado. Como hemos incluido una resistencia antes de la llegada a tierra, la intensidad que circulará por esa rama estará limitada, evitando el cortocircuito.

Para recibir la información en código usaremos la función `digitalRead()` donde incluiremos como parámetro el pin a leer.

En combinación con el montaje anterior podemos utilizar el siguiente código para leer cuando un pulsador esta activo y encender el LED del pin 13 en consecuencia.

``` c++
#include <Arduino.h>

const int pinI = 8;
const int pinO = 13;
 
void setup() {
  pinMode(pinI, INPUT);
  pinMode(pinO, OUTPUT);
}

void loop() {
  digitalWrite(pinO, digitalRead(pinI));
}

```

montaje

![Entrada correcta](/imagenes/montajes/entrada.png)
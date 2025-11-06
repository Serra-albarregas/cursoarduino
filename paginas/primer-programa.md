# Nuestro primer programa

En esta sección entenderemos el entorno de programación y crearemos nuestro primer programa sin tener que realizar un montaje.

## El entorno de desarrollo

Como hemos dicho, trabajaremos con el simulador [Wokwi](https://wokwi.com/), que permite trabajar tanto la parte de programación como la parte de montaje de manera cómoda. Veámoslo:

![Entorno de trabajo de Wokwi](/imagenes/teoria/entorno-wokwi.png)

En el entorno de Wokwi encontramos distintas secciones:

- **Editor de código**: donde se escribe el código del programa.
- **Simulador**: donde se realiza el montaje y se ven los resultados de la simulación.
- **Botón de ejecutar programa**: para lanzar el programa del editor de código.
- **Botón de añadir componente**: para agregar sensores, actuadores o controladores al simulador.
- **Opciones del componente**: permite editar opciones en los componentes, como los colores o la rotación.
- **sketch.ino**: `.ino` es la extensión de los programas de Arduino, que suelen llamarse *sketches*.
- **diagram.json**: representa el montaje en formato textual.

## En un Arduino real

Para programar en un Arduino real debe usarse el **Arduino IDE**, que puede encontrarse en su [página oficial](https://www.arduino.cc/en/software/).

## Estructura básica de un programa

Los programas de Arduino tienen siempre la misma estructura:

```c
// Zona DECLARACIONES

void setup() {
  // Zona función SETUP
}

void loop() {
  // Zona función LOOP
}
```

Donde las distintas secciones tienen las funciones siguientes:

- **Zona de declaraciones**: para declarar variables, funciones, objetos y estructuras.
- **Función setup**: función que se ejecuta una sola vez tras encender el programa. Se utiliza para inicializar los pines, variables, etc.
- **Función loop**: se ejecuta constantemente, una y otra vez.

## El hola mundo de Arduino: programa Blink

El programa *Blink* simplemente enciende y apaga un LED con un retardo. Su código es el siguiente:

```c
const int pinLED = 13;          // asignar variable LED como 13

void setup() {
  pinMode(pinLED, OUTPUT);      // definir pin 13 como salida  
}

void loop() {
  digitalWrite(pinLED, HIGH);   // encender LED
  delay(1000);                  // esperar un segundo
  digitalWrite(pinLED, LOW);    // apagar LED
  delay(1000);                  // esperar un segundo
}
```

Este código enciende y apaga un diodo LED. Sin embargo, como utilizamos para ello el pin 13, asociado al indicador **L**, no hace falta realizar ninguna conexión.
Observa cómo en la función `setup` se configura el pin 13 como pin de salida y cómo en la función `loop` se le otorga una salida HIGH para encender el LED y una salida LOW para apagarlo.

## Primer montaje: Blink con un diodo LED

En el caso anterior aprovechamos el indicador **L** para observar la salida del programa, pero es interesante también construir el circuito con un componente real. Añadiendo un LED, una resistencia de 220 Ω y haciendo un par de conexiones, el montaje queda como sigue:

![Montaje Blink](/imagenes/montajes/blink/blink-correcto.png)

Aunque parezca algo sencillo, hay más complejidad de la que parece, todo relacionado con el mundo de la electrónica. Aclaremos algunos puntos:

### Voltaje de los pines y conexión a tierra

Los pines digitales del Arduino entregan una señal de **5 V (HIGH)** o **0 V (LOW)**.  
Esto significa que cuando indicamos en el código `digitalWrite(pin, HIGH);`, el pin entrega 5 voltios respecto al punto de referencia, que es **GND (tierra)**.

Por eso, toda conexión eléctrica debe tener una **referencia común**: el terminal negativo del LED o cualquier componente debe estar conectado a **GND**.  
Si no se comparte esa referencia, el circuito no se completa y la corriente no fluye; por lo tanto, el LED no encenderá.

### Uso de resistencias

El **LED necesita una resistencia en serie** para limitar la corriente que circula por él.  
Sin resistencia, el pin del Arduino entregaría toda la corriente disponible, lo que podría **dañar tanto el LED como el microcontrolador**.

La resistencia actúa como un **“freno” al paso de corriente**, garantizando que el LED reciba solo lo necesario para iluminarse.  
El valor más común es **220 Ω o 330 Ω**, aunque puede variar según el color y tipo del LED.

El cálculo básico se hace con la **Ley de Ohm**:

$$
R = (V_{fuente} - V_{LED}) / I
$$

Por ejemplo, para un LED rojo (2 V, 20 mA) con una fuente de 5 V:

$$
R = (5 - 2) / 0.02 = 150 \, \Omega
$$

> [!NOTE]  
> Se suele usar un valor estándar superior (220 Ω) para proteger mejor el circuito sin perder demasiada luminosidad.

### Orientación del diodo

El **LED es un diodo**, y los diodos **solo permiten el paso de corriente en una dirección**.  
Por eso, su orientación en el circuito es fundamental.

El LED tiene dos patillas:

- **Ánodo (+)**: la patilla **más larga**, que debe conectarse al **pin de salida del Arduino** (donde sale la corriente).  
- **Cátodo (−)**: la patilla **más corta**, que se conecta a **GND** (tierra).

En el cuerpo del LED también hay una **zona plana** junto al cátodo, que ayuda a identificarlo si las patillas están cortas.  
Si conectas el LED al revés, simplemente **no encenderá**, pero no se dañará.

![Blink con LED conectado al revés](/imagenes/montajes/blink/blink-incorrecto.png)

# Introducción a Arduino

## ¿Qué es Arduino?

Arduino es una plataforma de hardware y software libre diseñada para que cualquier persona pueda crear proyectos electrónicos y de programación de forma sencilla. Está basada en una placa con un microcontrolador, es decir, un pequeño chip capaz de ejecutar instrucciones y controlar dispositivos externos (como LEDs, sensores o motores).

Nació en 2005 en Italia, en el Interaction Design Institute Ivrea, con la idea de ofrecer una herramienta accesible para artistas, estudiantes y aficionados a la electrónica. Desde entonces, Arduino se ha convertido en una de las herramientas más populares para aprender robótica, automatización, IoT y prototipado electrónico.

## ¿Para qué sirve?

Arduino sirve para controlar el mundo físico a través del código.  
Permite leer datos del entorno mediante sensores y responder con acciones a través de actuadores.

Por ejemplo:
- Encender una luz cuando oscurece.
- Medir temperatura y mostrarla en una pantalla.
- Activar una alarma al detectar movimiento.
- Controlar un motor o un sistema de riego automático.

## Desde un punto de vista sostenible

Arduino, al ser una herramienta abierta, económica y de bajo consumo, permite desarrollar soluciones sostenibles para problemas tanto cotidianos como de PYMES.  
Gracias a su flexibilidad, cualquier persona puede prototipar sistemas inteligentes que ayuden a:

- Reducir el consumo de energía.  
- Optimizar el gasto de agua.  
- Monitorear la calidad del aire o del suelo.  
- Promover la agricultura eficiente.

## Modelos de Arduino

Existen varios modelos de placas en el mercado. Todas ellas comparten el mismo lenguaje de programación y la misma filosofía de hardware libre, pero se diferencian en tamaño, cantidad de pines, memoria y potencia.  
Pueden revisarse en su [web oficial](https://www.arduino.cc/en/hardware/).

Además de los modelos que mencionaremos, existen otros con controladores adicionales integrados, como los **Arduino Leonardo** (que puede emular un teclado o ratón), **Arduino Due** (basado en un procesador ARM de 32 bits) o los **Arduino con conectividad WiFi o Bluetooth**, como el **Arduino Nano 33 IoT** o el **Arduino MKR WiFi 1010**, pensados para proyectos de **Internet de las Cosas (IoT)**.

Los modelos de Arduino se dividen en familias, entre las que destacan las siguientes.

### Uno

![Arduino UNO](/imagenes/teoria/arduino-uno.webp)

El **Arduino Uno** es la placa más conocida y utilizada de la familia Arduino. Es el modelo ideal para principiantes gracias a su simplicidad, estabilidad y gran compatibilidad con sensores y módulos. Utiliza el microcontrolador **ATmega328P**, funciona a **5 V** y ofrece **14 pines digitales** (6 de ellos PWM) y **6 entradas analógicas**.  
Es perfecta para proyectos educativos y prototipos básicos, como controlar LEDs, botones o pequeños motores.

### Nano

![Arduino NANO](/imagenes/teoria/arduino-nano.webp)

El **Arduino Nano** es una versión más compacta del Uno, pensada para integrarse directamente en una protoboard. A pesar de su pequeño tamaño, mantiene casi las mismas características técnicas que el Uno (mismo microcontrolador y velocidad de reloj).  
Es ideal para proyectos portátiles o embebidos, donde el espacio es limitado. Su conexión se realiza mediante un puerto **Mini-USB**, y ofrece **8 entradas analógicas** en lugar de 6.

### Mega

![Arduino MEGA](/imagenes/teoria/arduino-mega.webp)

El **Arduino Mega 2560** es una versión más potente, diseñada para proyectos grandes y complejos. Utiliza el microcontrolador **ATmega2560**, con mucha más memoria (**256 KB de Flash**) y **54 pines digitales**, además de **16 entradas analógicas**.  
También dispone de **cuatro puertos seriales**, lo que permite comunicar varios dispositivos al mismo tiempo.  
Es ideal para proyectos de robótica, impresoras 3D, automatización y sistemas de monitoreo con múltiples sensores o pantallas.

### Comparativa

| Característica                 | **Arduino Uno**            | **Arduino Nano**           | **Arduino Mega 2560**       |
|-------------------------------|-----------------------------|-----------------------------|------------------------------|
| **Microcontrolador**          | ATmega328P                 | ATmega328P                 | ATmega2560                  |
| **Voltaje de operación**      | 5 V                        | 5 V                        | 5 V                         |
| **Voltaje de entrada (recomendado)** | 7–12 V              | 7–12 V                     | 7–12 V                      |
| **Pines digitales de E/S**    | 14 (6 PWM)                 | 14 (6 PWM)                 | 54 (15 PWM)                 |
| **Entradas analógicas**       | 6                          | 8                          | 16                          |
| **Corriente por pin de E/S**  | 20 mA máx.                 | 20 mA máx.                 | 20 mA máx.                  |
| **Memoria Flash**             | 32 KB (0.5 KB usada por bootloader) | 32 KB (0.5 KB usada por bootloader) | 256 KB (8 KB usada por bootloader) |
| **SRAM**                      | 2 KB                       | 2 KB                       | 8 KB                        |
| **EEPROM**                    | 1 KB                       | 1 KB                       | 4 KB                        |
| **Velocidad del reloj**       | 16 MHz                     | 16 MHz                     | 16 MHz                      |
| **Conector USB**              | Tipo B estándar            | Mini-USB                   | Tipo B estándar             |
| **Tamaño físico aprox.**      | 68.6 × 53.4 mm             | 45 × 18 mm                 | 101.5 × 53.3 mm             |
| **Peso aprox.**               | 25 g                       | 7 g                        | 37 g                        |
| **Puertos UART (Serial)**     | 1                          | 1                          | 4                           |
| **Uso típico**                | Proyectos educativos y básicos | Proyectos compactos o portátiles | Proyectos grandes o con múltiples sensores |

## ¿Qué vamos a usar?

Por comodidad, en nuestras clases utilizaremos un simulador de Arduino. De esta forma evitaremos tener que realizar una inversión para un trabajo académico.

El simulador que emplearemos será [Wokwi](https://wokwi.com/), con capacidad para emular los tres modelos de Arduino mencionados, así como otros controladores.  
Lo único que necesitaremos será tener una cuenta en la aplicación para poder trabajar en el entorno en la nube que ofrece.

Aquellos que seáis más curiosos o tengáis ganas de verlo en el mundo real, podéis compraros un **kit de iniciación de Arduino**.  
Existen muchos kits en el mercado que se diferencian en el tipo de placa que incluyen y la cantidad de sensores y actuadores.

![Ejemplo de kit de Arduino](/imagenes/teoria/kit-arduino.jpg)

Estos kits no son especialmente baratos y puede darse el caso de que mezclen componentes muy útiles con otros que rara vez se usan.  
Un buen kit debería incluir:

- Un **Arduino**, obviamente. Lo normal sería empezar por un **Arduino UNO**.  
- Una **breadboard**, utilizada para hacer conexiones cómodamente.  
- Un buen puñado de **cables con terminal Dupont**, que puedan conectarse a la placa y a los pines del controlador.  
- Componentes habituales: **LEDs, pulsadores, resistencias**.  
- **Sensores**, para realizar mediciones y tomar medidas.  
- **Actuadores**, como motores o servos, transistores y relés.  
- Otros dispositivos como **potenciómetros, joysticks, teclados o displays**.

> [!NOTE]
> Es muy posible que un kit de Arduino no contenga los sensores y actuadores necesarios para el proyecto de la asignatura.
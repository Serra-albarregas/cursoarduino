# Componentes de Arduino

Antes de comenzar a programar y conectar sensores, es importante conocer las partes que forman una placa Arduino. Cada componente cumple una función específica y todos trabajan en conjunto para que el microcontrolador pueda recibir información del entorno, procesarla y actuar sobre distintos dispositivos.

A lo largo de esta sección exploraremos los elementos más importantes del hardware, como el microcontrolador, los pines de entrada y salida, los reguladores de voltaje, los conectores USB y de alimentación, las memorias y otros detalles que hacen de Arduino una plataforma tan versátil y accesible.

> [!NOTE]
> Es importante conocer la placa para poder usarla correctamente y hacer buen uso de cada pin.

## Componentes de la placa

![Componentes de Arduino R3](/imagenes/teoria/esquema-arduino.png)

### Botón de reset

Al presionarlo se conecta el pin de reset a tierra, lo que reinicia el microcontrolador y ejecuta el código de nuevo desde el principio.

### USB

Tiene tres funciones principales:
1. Cargar el programa desde el ordenador a la placa.  
2. Alimentar el Arduino con 5 V cuando está conectado al ordenador.  
3. Comunicación serial entre la placa y el ordenador, para enviar datos durante la ejecución.

### ATmega16U2

Controlador USB. Funciona como un traductor entre el puerto USB del ordenador y el microcontrolador principal.

### Cristal oscilador

El oscilador de cristal de cuarzo proporciona una señal de reloj precisa al microcontrolador. Determina la velocidad a la que el microcontrolador ejecuta las instrucciones y hace que los eventos de tiempo sean más precisos.

### Regulador de voltaje

Controla y estabiliza la cantidad de voltaje que entra en la placa. Evita el exceso de voltaje que puede dañarla.

### Jack de alimentación

Se trata de la conexión eléctrica dedicada a una fuente externa como pilas o baterías.

### Indicadores LED

Indicadores visuales que muestran la actividad del Arduino:
- **TX (Transmisión):** Se enciende cuando la placa está enviando datos al ordenador.  
- **RX (Recepción):** Se enciende cuando la placa recibe datos del ordenador.  
- **L (Pin 13):** Se enciende cuando pasa corriente por el pin 13. Se utiliza para pruebas rápidas.  
- **ON:** Se enciende cuando la placa recibe corriente.

### Entrada ICSP

Se usa para grabar programas en el controlador sin utilizar el puerto USB.

### ATmega328

Es un microcontrolador de 8 bits de alto rendimiento y bajo consumo. Se encarga de mantener los datos y ejecutar los programas. Cuenta con:
- Una **memoria Flash de 32 KB** para el almacenamiento de los programas.  
- Una **memoria SRAM de 2 KB** de acceso aleatorio. Actúa como la memoria de trabajo del controlador, almacenando variables temporales, datos y la pila de ejecución. Es una memoria volátil.  
- Una **memoria EEPROM de 1 KB** reprogramable. Es una memoria persistente usada para guardar los datos de configuración, estado del sistema o información que no debe perderse tras un reinicio.

> [!NOTE]
> Técnicamente la memoria EEPROM no es de solo lectura, puesto que se puede escribir en ella, pero debido a su velocidad de escritura más lenta y a sus limitados ciclos de escritura, se debe usar con moderación.

### Pines

Se trata de las entradas y salidas del Arduino. Se dividen en las siguientes categorías:

#### Pines de alimentación

Agrupan los pines que suministran energía:
- **5V:** salida para alimentar sensores o módulos.  
- **3.3V:** salida de menor voltaje para componentes más sensibles.  
- **GND:** conexión a tierra.  
- **VIN:** entrada de voltaje externo (si no se usa conector jack).  
- **IOREF:** referencia de voltaje del microcontrolador.

#### Pines digitales

Son pines que pueden funcionar como entradas o salidas digitales, es decir, que solo admiten dos valores posibles: HIGH o LOW. Se usan para conectar LEDs, sensores, relés, etc.  
Los pines marcados con una virgulilla (~) pueden generar señales **PWM** (simulación de señales analógicas), muy útiles para controlar la intensidad de un LED o la velocidad de un motor.

#### Pines analógicos

Estos pines sirven para leer valores analógicos (por ejemplo, de un sensor de luz o temperatura).  
El microcontrolador los convierte a valores digitales mediante un **ADC** (convertidor analógico-digital) de 10 bits, que permite lecturas de 0 a 1023.

## Esquema de patillaje

El esquema de patillaje o **pinout** de un dispositivo es un documento de referencia a la hora de realizar un montaje. Se trata de un esquema que detalla las funciones y capacidades de cada uno de los pines.  
Puedes encontrar los pinouts de Arduino en su [documentación oficial](https://docs.arduino.cc/hardware/), pero aquí te dejo el del **UNO R3**:

![Pinout del Arduino UNO R3](/imagenes/teoria/pinout-arduinor3.png)
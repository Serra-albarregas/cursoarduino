# Entradas y salidas digitales

Además de las entradas digitales, Arduino cuenta con un conjunto de entradas **analógicas**.

Una señal analógica es aquella que puede tomar cualquier valor dentro de un intervalo -Vcc y +Vcc. En nuesto arduino esto significará que las señales tendrán valores entre los 0V y los 5V (2.34 podría ser un dato válido).

Esta es la diferencia fundamental entre los pines analógicos y digitales. Mientras los primeros pueden trabajar con distintos voltajes en el rango establecido (0-5), los segundos solo aceptan valores **LOW** y **HIGH**.

> [!NOTE]
> La otra diferencia es que los pines analógicos son más caros que los digitales.

Aunque hemos hablado de entrada y salida analógica indistintamente, en realidad no tienen nada que ver, como veremos a continuación.

## Entradas analógicas

En Arduino, las entradas analógicas se utilizarán comunmente para leer valores de sensores. La sensorización que pueda realiarse a una vivienda o a una empresa por lo general no va a aportar valores discretos, si no que van a ser continuos. La medición de temperatura supondrá distintos valores, también la de humedad y la de presencia. Por ello, con sólo las entradas digitales no es suficiente, necesitamos las **entradas analógicas**

Una señal analógica, como es la de la corriente eléctrica, debe digitalizarse para poder trabajar con ella mediante código. Cuando trabajamos con las señales digitales hablamos de un muestreo de dos valores (LOW y HIGH) y en el caso de las lecturas analógicas el proceso es muy parecido. Para convertir la señal analógica en una señal digital que parezca analógica debemos realizar un muestreo de más valores.

En el caso de nuestro arduino se utilizan 10 bits para codificar el resultado del muestreo, dando lugar a 1024 valores posibles. Como trabajamos con 5 voltios, si hacemos la división:

$$
r = V / 2^n = 5 / 1024 = 4.88mV
$$

El resultado que obtenemos es que los 1024 grupos que podemos formar están separados por 4.88mV entre ellos, lo que supone una precisión de la medición de **+-2.44mV**.

Hay que tener en cuenta que, con esta configuración, si dispusiesemos de un sensor cuya salida otorgase voltajes entre los 0V y 1V la precisión de la medición se reduciría puesto que no estariamos aprovechando los 1024 valores disponibles. En este caso solo estaríamos usando 1024/5 = 204 valores, perdiendo precisión.

Para solucionar esta situación, arduino permite cambiar la tensión tomada como referencia por en conversor analógico digital. Esto se realiza mediante la función `AnalogRef`, donde se le pueden indicar los siguientes valores:

- `DEFAULT`: Valor por defecto, para nosotros 5V.
- `INTERNAL`: Corresponde con 1.1V.
- `EXTERNAL`: Corresponde con el voltaje aplicado al pin Vref (que debe estar siempre entre 0V y 5V).

### Lectura de un potenciómetro

Un potenciómetro es una resistencia variable mediante un dial. Por lo general toma valores de entre aproximadamente 0Ω y 5kΩ, 10kΩ o 20kΩ. Tiene 3 patillas.

![Potenciómetro](/imagenes/componentes/potenciometro.png/)

| Nombre | Descripción |
|-|-|
| GND | Conexión a tierra |
| SIG | Salida variable, conexión a entrada analógica |
| VCC | Alimentación |

El montaje para la lectura de un potenciómetro es el siguiente:

![Lectura de potenciómetro](/imagenes/montajes/IOanalogica/lecturaPotenciometro.png)

En código, utilizaremos la función `analogRead` para realizar la lectura, indicando por parámetro el pin de lectura y obteniendo un valor entre 0 y 1023. Para la lectura del potenciómetro podemos emplear el siguiente código.

``` c++
const int pinI = 14;

void setup() {
  pinMode(pinI, INPUT);
  Serial.begin(9600);
}

void loop() {
  Serial.println(analogRead(pinI));
  delay(200);
}
```

Este programa simplemente muestra en la consola a través del serial el valor leído. Al ejecutar el programa se puede apreciar como los valores mostrados en la consola están comprendidos entre 0 y 1023 (1024 valores en total).

## Salidas analógicas

Las salidas analógicas son tal vez las mas extrañas de todas, pues realmente no existen en Arduino. La placa no tiene la capacidad de otrogar valores que no sean de 0V o de 5V y la solución que aporta es la de realizar una simulación con las capacidades que tiene.

Las salidas analógicas en Arduino funcionan mediante PWM. Los pines que admiten esta configuración son los que están marcados con una virgulilla (~) y se basan en el siguiente truco:

- Primero se divide la señal en pulsos (duty cycle) que duren un cierto tiempo (el tiempo es muy pequeño, por lo que esto pasa muy rápido).
- En cada pulso, se enciende la señal durante un tiempo y se apaga durante el resto.
- Con la variación del tiempo de encendido y apagado durante el duty cicle se consiguen los distintos valores pseudo-analógicos.

![PWM](/imagenes/teoria/pwm.svg)

Esta señal es suficiente para simular una señal analógica que puede ser util en muchas aplicaciones:

- Para variar la intensidad de un LED.
- - 0% duty: LED apagado.
- - 50% duty: LED enciendiendose y apagandose muy rápido. Se percibe como la mitad del brillo.
- - 100% duty: LED encendido.
- Control de velocidad de motores de corriente continua.
- - 0% duty: motor parado.
- - 50% duty: gira a mitad de su velocidad.
- - 100% duty: gira a toda velocidad.
- Control de servomotores.
- Audios simples y tonos en un buzzer.

Sin embargo, no podemos olvidar que estamos alimentando con 5V, por lo que si algún componente está diseñado para un voltaje menor podremos dañarlo. Además, debemos tener en cuenta que las continuas conexiones y desconexiones sobre algunos componentes. Los LEDs no tienen ningún riesgo, los motores se acompañan de un transistor para que sean seguros pero componentes mecánicos como los relés sufren enormemente con PWM.

### Modificando el brillo de un LED

Para tener un LED que brille con una intensidad variable utilizaremos PWM. El montaje será el siguiente:

![PWM](/imagenes/montajes/IOanalogica/ledVariable.png)

Utilizamos simplemente un LED rojo conectado a través de una resistencia de 220Ω con el pin 3, que es uno de los que admiten PWM.

Para el código emplearemos la función `analogWrite()` que aceptará como parámetro un número que represente el duty cicle. Este número tomará los valores comprendidos entre 0 y 255, siendo 0 el 0% del duty cicle y 255 el 100%.

``` c++
const int pinO = 3;
int duty = 126;

void setup() {
  pinMode(pinO, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  analogWrite(pinO, duty);

  if (Serial.available()) {
    String linea = Serial.readStringUntil('\n');
    int leido = linea.toInt();

    if (leido > 255) leido = 255;
    if (leido < 0) leido = 0;

    duty = leido;
    Serial.print("Nuevo duty: ");
    Serial.println(duty);
  }
}
```

> ![NOTE]
> Aunque en el programa estamos realizando una verificación del valor leído para que se ajuste al rango 0-255 la función `analogWrite` ya hace esa comprobación por lo que no sería necesario. En nuestro caso solo lo incluimos para mostrar el valor en la terminal.

## Combinando entradas y salidas analógicas

### Control de la intensidad de un LED mediante un potenciómetro

En este ejemplo combinaremos el funcionamiento de los dos ejemplos anteriores vistos en esta página. Controlaremos mediante una resistencia variable la intensidad de un diodo LED. El montaje es el siguiente.

![PWM](/imagenes/montajes/IOanalogica/potenciometroyled.png)

El código que emplearemos realizará una lectura en el pin 14 (A0) de valores analógicos convertidos en un rango entre 0 y 1023 mediante la función `analogRead`.

Como la salida que necesitamos debe situarse en el rango 0-255 debemos mapear el valor de la entrada. Para evitarnos las matemáticas de la conversión podemos utilizar la funcion `map`. Esta función cuenta con la siguiente cabecera:

`long map(long x, long in_min, long in_max, long out_min, long out_max)`

Donde:

- x: el número a transformar.
- in_min: el valor mínimo de la entrada, 0 en nuestro caso.
- in_max: el valor máximo de la entrada, 1023 en nuestro caso.
- out_min: el valor mínimo de la salida, 0 en nuestro caso.
- out_max: el valor máximo de la salida, 255 en nuestro caso.

Una vez mapeado el valor de entrada para localizarse en el rango correcto utilizamos la función `analogWrite` en el pin 3 para utilizar PWM y darle una intensidad variable al LED.

``` c++
const int pinO = 3;
const int pinI = 14;

void setup() {
  pinMode(pinO, OUTPUT);
  pinMode(pinI, INPUT);

  Serial.begin(9600);
}

void loop() {
  int intensidad = map(analogRead(pinI), 0, 1023, 0, 255);
  analogWrite(pinO, intensidad);

  Serial.println("Intensidad: " + String(intensidad));
}
```

### Control del color de un LED RGB


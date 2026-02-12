# Sensor de luz: LDR

Un LDR (Light-dependent resistor) es una resistencia cuyo valor cambia en función de la luz que reciba. Mediante esta variación de la luz y algo de matemática podemos tener en nuestro arduino un sistema para medir la cantidad de luz de un lugar.

El fotoresistor disminuye su resistencia a medida que aumenta la luz recibida, siendo sus valores típicos de 1MΩ en oscuridad absoluta y de unos 50-100Ω en luz brillante.

Matemáticamente, la relación entre la iluminancia y la resistencia de un LDR sigue la siguiente función:

$$
I/I_{0} = (R/R_{0})^{-γ}
$$

Donde:
- I: La iluminación que se quiere medir.
- I<sub>0</sub>: La iluminación en un momento conocido, ya medido.
- R: La resistencia del LDR en el momento que se quiere medir.
- R<sub>0</sub>: La resistencia del LDR con la iluminación I<sub>0</sub>
- γ: La constante que mide la relación logarítmica entre iluminación y resistencia, sus valores típicos rondan los 0.5-0.8.

Estos datos se pueden obtener del fabricante del componente, sin embargo, existen pequeñas variaciones que dependen de la fabricación del dispositivo por lo que, de ser posible, conviene realizar un proceso de calibraciación previo a su uso.

En el simulador de [wokwi](https://wokwi.com), disponemos del siguiente fotoresistor:

![LDR](/imagenes/componentes/ldr.png)

Este componente cuenta con los siguientes conectores:

| Nombre | Descripción |
|--------|-------------|
| VCC | Alimentación |
| GND | Conexión a tierra |
| DO | Salida digital |
| AO | Salida analógica |

Con este componente, la iluminación se calcula con la siguiente función:

``` c++
float calcularLux(int lecturaAnalogica) {
  const float GAMMA = 0.7;
  const float RL10 = 50;

  float voltaje = lecturaAnalogica / 1024. * 5;
  float resistencia = 2000 * voltaje / (1 - voltaje / 5);
  float lux = pow(RL10 * 1e3 * pow(10, GAMMA) / resistencia, (1 / GAMMA));

  return lux;
}
```

Donde, sin entrar en mucho detalle, se está calculando el voltaje correspondiente a la lectura analógica, la resistencia de un divisor de tensión interno del componente y se están aplicando los valores γ y resistencia de referencia a 10 lumenes.

Con toda esta información, podemos realizar el primer montaje.

![LDR](/imagenes/montajes/ldr/ldr1.png)

Y lo controlamos mediante el siguiente código:

``` c++
const int pinI = 14;


void setup() {
  pinMode(pinI, INPUT);
  Serial.begin(9600);
}

void loop() {
  int lectura = analogRead(pinI);
  float lux = calcularLux(lectura);

  Serial.println("Valor leído: " + String(lectura) + " - Lux: " + String(lux));
  delay(100);
}


float calcularLux(int lecturaAnalogica) {
  const float GAMMA = 0.7;
  const float RL10 = 50;

  float voltaje = lecturaAnalogica / 1024. * 5;
  float resistencia = 2000 * voltaje / (1 - voltaje / 5);
  float lux = pow(RL10 * 1e3 * pow(10, GAMMA) / resistencia, (1 / GAMMA));

  return lux;
}
```

Podemos hacer click en el LDR de la simulación y modificar mediante un slider el valor de lumenes recibido, para comprobar que se muestra por la terminal del serial un valor similar.


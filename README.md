# Logger-low-power
### Un repositorio para un registrador de datos de campo basado en el ATMega 328.

----------------------------------------------
# Problemática

Un registrador de datos es un aparato electrónico que logra que un sensor tome medidas con cierto *intervalo de tiempo* prefijado, guardando luego el resultado de cada medida, junto con una etiqueta de tiempo. Si bien registradores de señales hay muchos (en algunos casos, muy complejos), los ***registradores ambientales*** deben poseer ciertas características típicas:

1. La frecuencia de muestreo es habitualmente baja.
   
1. No siempre puede estar conectado a una red eléctrica (se toman datos a campo), lo que condiciona el consumo de corriente del aparato: hablamos de un producto *de bajo consumo*, que pueda vivir al menos un año con alimentación a pilas o alguna otra opción sencilla de transportar a campo.

1. Debe funcionar bajo grandes amplitudes térmicas y condiciones de humedad ambiente. En en el mejor de los casos, debe ser *sumergible*.


Los registradores de datos privativos son una opción testeada (cerrada). Generalmente, estos registradores tienen cierta especificidad del diseño para diferentes sensores, integrando sensores y registradores en el mismo producto ([OnSet](https://www.onsetcomp.com/), que posee las marcas HOBO e InTemp); algunos fabricantes disponen de registradores multipropósito ([Campbell Scientific](https://www.campbellsci.com/data-loggers)), que disponen de diferentes protocolos para conectar sensores varios.

Uno de los problemas asociados a los registradores privativos es que presentan pocas posibilidades de modificación y habitualmente vienen acompañados por software específico (cerrado). Además, el precio en USD resulta elevado --o elevadísimo, según el registrador-- comparados con los presupuestos que se manejan para investigación en los países en vías de desarrollo, como los países sudamericanos. 

Por otra parte, los sensores electrónicos de bajo costo son una realidad que tiene al menos 15 años. Los hay de muchos tipos y para diferentes magnitudes. Este tipo de sensores, sumados a una placa Arduino, podrían ser una opción realmente asequible para utilizar con los presupuestos de investigación disponibles, logrando hacer realidad medidas ambientales de bajo costo. 

## El tiempo es tirano: estrategias habituales en la academia para la utilización de sensores de bajo costo y hardware abierto

A fines de utilizar los sensores de bajo costo en lugar de los privativos, habitualmente se recurre a placas Arduino y algún sistema de alimentación. Sin embargo:

 * Si utilizamos Arduino UNO, dado el alto consumo, estamos condenados a periodos de muestro cortos o anclajes a la red eléctrica.

 * Si se utilizan modelos *weareable*, como la Pro Mini, también se presentan problemas de consumo, como leds, reguladores de tensión de bajo rendimiento a baja corriente, etc.

Una de las soluciones habituales a estos problemas de consumo reside en el recambio de baterías en periodos cortos, que involucran una gran catidad de logística asociada si se tienen muchos puntos de muestreo; la utilización de red eléctrica implica un gran limitante a la hora de medir variables a campo, limitando los puntos de muestreo a lugares que tengan acceso a la red eléctrica. 

Otra posibilidad es la adquisición de diferentes módulos (placas con diferentes circuitos integrados listas para usarse con Arduino). Si bien apoyarse en módulos es en principio una buena idea, presenta algunos inconvenientes:


1. Los módulos son, en general, más caros que la compra de los integrados individuales.

1. Los módulos no siempre tienen estabilidad en el diseño, o es complicado conseguir algunos por las vías regulares. Eso conlleva que se tenga que pensar en diseños que cambian según los módulos conseguidos.

1. Los módulos, muchas veces, tienen ciertos problemas de diseño. Esto requiere la modificación --soldador y pinza en mano-- de la placa, cambiando o quitando componentes, a fines de bajar el consumo.

Si bien las soluciones anteriores permiten obtener medidas en algunas locaciones y mantener el bajo costo, tienden a consumir mucho tiempo de trabajo. En el caso de emplear módulos, los diseños de registrador para cada sensor son modificados dada la discontinuidad de los módulos. Esto complica la acumulación de experiencia en un solo diseño de registrador y dificulta la utilización de nuevos sensores.

Este proyecto busca crear un registrador de datos que pueda sea de bajo costo (en componentes) y abierto, que tenga un diseño de bajo consumo y que permita la acumulación de experiencia en un diseño, en lugar de recomenzar cada vez que se quiere probar un sensor a campo.

--------------------------------
--------------------------------
# LoggeArg

El registrador de datos propuesto tiene algunos componentes que pueden conseguirse fácilmente:

1. Microcontrolador ATMega 328. 
2. RTC DS 3231.
3. Serial Flash Memory Windbond W25Q64FV (8 Mbytes de almacenamiento).
4. CP2102
Este diseño tiene que completarse con un CP2102 para programar y obtener los datos de cada registrador.


Una de las grandes diferencias con los registradores diseñados a partir de módulos es el reemplazo total de la tarjeta SD flash en favor de un integrado que es una pequeña memoria FLASH, con comunicación serial.


## Conexiones RTC DS 3231

|3231 Nombre|Pin ATmega  328 | Pin Arduino IDE |Detalle|
|:-:|:-:|:-:|:-:|
| GND  | |      |      GND|
|Bat | - | - | Vcc|
|SQW(Alarma)  | 4 | D2(INT0)  | Interrupción por hardware para despertar el ATMega328|
|SDA | 27 |A4(SDA)  |SDA, Comunicación i2C|
|SCL |  28 | A5(SCL)  | SCL, Comunicación i2C|

## Conexiones Serial Flash Memory Windbond W25Q64FV

|PinFlash| Nombre Flash   |ATMega328 |PinATMega|PinArduinoIDE|
|:-:|:-: | :-: |:-:|:-:|
|1|/CS|/CS|6|D4|
|2|SDO(ex MISO)|SDI(*ex MISO*)|18|D12|
|3|WP|VCC|13|D7|
|4|GND|GND|GND|GND|
|5|SDI(*ex MOSI*)|SDO(*ex MOSI*)|17|D11|
|6|CLK|SCK|19|D13|
|7|/HOLD|VCC|VCC|VCC|
|8| Vcc |Vcc |Vcc|Vcc|

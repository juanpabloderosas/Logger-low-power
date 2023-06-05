# Logger-low-power

### Un repositorio para un registrador de datos de campo basado en el ATMega 328.

Un registrador de datos ambientales es un aparato electrónico que logra que un sensor mida en un determinado *intervalo de muestreo*, y guarda esos datos, junto con el tiempo de la medida. La especificidad de ***registrador ambiental*** está dada en general por dos factores:

1. Un registrador de datos ambientales no siempre puede estar conectado a una red eléctrica, lo que condiciona el consumo: hablamos de un producto *low-power*, que pueda vivir al menos un año a baterías.

1. Un registrador de datos ambientales debe funcionar bajo grandes amplitudes térmicas y condiciones de humedad ambiente, en el mejor de los casos, que sea *sumergible*.

Los sensores comerciales siempre son una buena opción y están testeados. Sin embargo, el precio en USD resulta elevado --o elevadísimo, según el registrador-- para los presupuestos que se manejan en la investigación y desarrollo nacional.

Por otra parte, los sensores electrónicos de bajo costo son una realidad que tiene al menos 15 años. En efecto, es posible conseguir sensores muy buenos a un precios bajos. Las placas de desarrollo tipo Arduino, sin embargo, presentan grandes consumos (debido a la versatilidad del diseño, y no estar pensadas en general para bajo consumo), que hacen que los sensores de bajo costo tengan que ser probados a campo utilizando grandes baterías o enchufándolos a la red eléctrica.

Existen varios proyectos involucrados en conseguir subsanar los requerimientos de consumo y condiciones de campo, entre ellos, el descomunal proyecto de Ed. Mallon, [Cave Pearl Project](https://thecavepearlproject.org/). Este proyecto busca registradores de datos abiertos, basados no en chips, sino en una Arduino ProMini y controladores montados en placas de desarrollo (módulos).

Si bien apoyarse en módulos es en principio una buena idea, presenta algunos inconvenientes:

1. Los módulos, muchas veces, tienen ciertos problemas de diseño. Esto hace que haya que *operarlos* para sacarles algunas partes o cambiar otras.

1. Los módulos son, en general, más caros que la compra de los componentes individuales.

1. Los módulos no siempre tienen estabilidad en el diseño, o es complicado conseguir algunos por las vías regulares. Eso conlleva que se tenga que pensar en diseños que cambian según los módulos conseguidos.

Es por esto que buscamos una solución de desarrollo abierto, pero más estable y serializable que las soluciones basadas en módulos. Pensando en lo anterior, es hora de explotar las posibildiades de serialización de una impresora 3D y una máquina de fresado con control CNC. Mientras que la primera puede producir *holders* (receptáculos?), la segunda puede permitir obtener placas simples a partir del fresado de una placa PCB virgen.

## Componentes

El registrador de datos propuesto tiene algunos componentes que pueden conseguirse fácilmente:

1. Microcontrolador ATMega 328. 
2. RTC DS 3231.
3. Serial Flash Memory Windbond W25Q64FV (8 Mbytes de almacenamiento).

Este diseño tiene que completarse con un CP2102 para programar.










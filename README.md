# Piscina automatizada

Nuestro proyecto consiste en automatizar una piscina para facilitar al usuario su uso y manejo ante cambios de luz y adversidades meteorólogicas (lluvia y cambios de temperatura y presión); de modo que, en función de cada situación, pueda ser tapada, expulsar agua y activar la iluminación.


## Integrantes del grupo

 1. Jimena López Maldonado - jimenalopez02 .
 2. Aitana Martínez Gayà -  aitana-martinez .
 3. Natalia Miguel Cuenca -  nataliamiguel .
 4. Carolina Plaza -  carolplaza .
 5. Marta Sánchez Ferrer-  marta-sferrer .

## Objetivos del trabajo
Desarrollar un programa en lenguaje C con objetivo de automatizar una piscina de manera que, según los valores de distintos parámetros físicos (presión, temperatura e iluminación), se lleven a cabo funciones tales como el vaciado parcial de su interior, el encendido o apagado de la iluminación y cubrirla con una lona. 
Además, el usuario tendrá acceso a un menú para poder llevar a cabo todas estas funciones manualmente.

## Maqueta de la piscina
Para llevar a cabo nuestro trabajo construiremos una maqueta que simule una piscina real y algunas de sus funciones.
Para ello utilizaremos un recipiente de plástico, el cual perforaremos para que exista una trampilla que pueda expulsar el agua cuando sobrepase el nivel deseado por el usuario.
Llevará incorporada:
- Un termómetro (sonda de temperatura arduino  DS18B20).
- Un sensor de iluminación (sensor LDR).
- Un sensor de presión barométrica (módulo BMP180).


## Dinámica de la aplicación
| Menú |
| --- |
| 1. *Abrir* o *Cerrar* toldo  |
| 2. Temperatura |
| 3. *Encendido* o *Apagado* de la iluminación |
| 4. Control del nivel del agua |
| 5. *Automático* |

**Opción 1 - *Abrir* o *Cerrar* toldo**
El usuario puede manipular la apertura y el cierre del toldo.


**Opción 2 - Temperatura**
Mediante el acceso a está opción, el usuario puede conocer la temperatura del interior de la piscina.


**Opción 3 - *Encendido* o *Apagado* de la iluminación**
Manualmente, el usuario puede manejar la iluminación de la piscina.


**Opción 4 - Control del nivel del agua**
El usuario puede conocer el nivel del agua, para así cambiarlo a su elección.


**Opción 5- *Automático***
Esta opción es el principal propósito de nuestro programa, ya que facilita al usuario el manejo de la piscina y le ahorra tiempo. 
Primeramente, tendrá que introducir los baremos correctos entre los que se puedan encontrar los valores de la temperatura, la presión y la intensidad luminosa. 
Así, si los valores de presión superan los fijados, se cerrará el toldo (por si fuera debido a la lluvia) y se abrirá la trampilla que expulsa agua para reestablecer los valores de presión. 
Del mismo modo, cuando los sensores de iluminación detecten que la intensidad luminosa no es suficiente (durante la noche o si está nublado), se encenderán las luces de la piscina y, por el contrario, si hay luz suficiente, la iluminación permanecerá apagada; pero esto solo ocurre cuando el sensor de movimiento detecta movimiento a una distancia también introducida por el usuario, para evitar el gasto de luz si no hay nadie en las inmediaciones.


 ## Sensores
 **Medida de temperatura- Sonda de temperatura DS18B20:**
*Fuente*: https://programarfacil.com/blog/arduino-blog/ds18b20-sensor-temperatura-arduino/
El sensor de temperatura DS18B20 es un dispositivo que se comunica de forma digital. Cuenta con tres terminales: Vcc, GND y el pin Data y utiliza comunicación por protocolo serial digital OneWire; este protocolo de comunicación permite enviar y recibir datos utilizando un solo cable (a diferencia de otros, que utilizan dos o más líneas de comunicación digital).
Para leer el sensor con un Arduino es necesario utilizar dos librerías que deben ser instaladas antes de cargar el código a nuestra placa de desarrollo; son las siguientes:
- Dallas Temperature.
- OneWire
El DS18B20 tiene errores debido a factores externos, al ruido inherente en los circuitos eléctricos y alteraciones en el medio físico.

**Medida de intensidad luminosa- Sensor de luz LDR GL55:**
*Fuente*: https://www.luisllamas.es/medir-nivel-luz-con-arduino-y-fotoresistencia-ldr/
El sensor LDR es un dispositivo formado por un semiconductor y su funcionamiento es tal que, al aumentar la incidencia de la luz sobre él, disminuye su resistencia.
Este cambio de resistencia se debe a que, cuando recibe luz, el semiconductor que lo forma absorbe fotones y los electrones pasan a la banda de conducción y así disminuye su resistencia.
Su mayor desventaja es la poca precisión y su uso limitado, ya que no puede ser usado para medir la intesidad lumínica, solo detecta los valores de "oscuridad" y "luminosidad".

 **Medida de cambios de presión del agua- Módulo de presión barométrica BMP180**
*Fuente*: https://naylampmechatronics.com/blog/43_tutorial-sensor-de-presion-barometrica-bmp180.html
Como su nombre indica, este sensor mide la presión atmosférica. Para su uso es necesario descargar la librería desarrollada por Sparkfun (se puede descargar desde el link de la fuente).
Su mayor inconveniente en nuestro proyecto es que se ve afectado por los cambios de temperatura.
Las funciones que vamos a utilizar con este sensor son: 
- *begin ()*: nos permite inicializar el sensor,
- *startPressure(Sobremuestreo);*: permite iniciar la medición de presión e indica el tiempo que tarda en ofrecer el resultado,
- *getPressure(P, T);*: da como resultado el valor de presión; hay que indicarle la temperatura, ya que ésta influye en los cálculos de la presión.
Todas las funciones devuelven un 1, si se realizan con éxito, o un 0, si no funcionan y hay algún error.


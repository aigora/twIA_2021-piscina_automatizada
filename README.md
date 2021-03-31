# Piscina automatizada

Automatizar una piscina de manera que en función de la humedad y la temperatura del exterior sea tapada. Permitir que la piscina expulse el agua de la lluvia si se supera el nivel de agua deseado, y activar iluminación interna mediante sensores.

## Integrantes del grupo

 1. Jimena López Maldonado - jimenalopez02 .
 2. Aitana Martínez Gayà -  aitana-martinez .
 3. Natalia Miguel Cuenca -  nataliamiguel .
 4. Carolina Plaza -  carolplaza .
 5. Marta Sánchez Ferrer-  marta-sferrer .

## Objetivos del trabajo
Simulación de una piscina, cuya programación permita al usuario elegir entre: abrir y cerrar un toldo según la temperatura y humedad que haya, la iluminación mediante sensores que estén estimulados por factores como la oscuridad o el movimiento (utilizando tecnología LED) y controlar el nivel del agua si este aumenta por factores externos como la lluvia.
## Maqueta de la piscina
Nuestro trabajo consiste en realizar una maqueta de una piscina, de tal manera que simule una real con algunas de sus funciones.
Para ello utilizaremos un recipiente de plástico el cual perforaremos para que exista una trampilla que pueda expulsar el agua cuando se pase el nivel deseado por el usuario.
Llevará incorporada :
-un termómetro ( sensor de temperatura arduino  DS18B20 ) que permita al usuario saber la temperatura del agua.
- un sensor de iluminación ( sensor LDR )
- sensor de nivel del agua,* el cual construiremos nosotras a través de un sensor de ultrasonidos ( HC-SR04 ) y una pantalla LCD 16x2 *

**Dinámica de la aplicación**
| Menú |
| --- |
| 1. *Abrir* o *Cerrar* toldo  |
| 2. *Encendido* o *Apagado* de la iluminación |
| 3. Control del nivel del agua |

## Opción 1 - *Abrir* o *Cerrar* toldo
El programa cuenta con diferentes funciones, entre las cuales, el usuario tendrá que insertar los datos con los que desea que dicho programa se ejecute. 
Primero, deberá establecer para que límites de temperatura y humedad exije que el toldo se cierre, por el contrario, permanecerá abierto. 

## Opción 2 - *Encendido* o *Apagado* de la iluminación
El usuario deberá determinar el grado de oscuridad para el que pretende que la piscina se ilumine, a la vez que la sensibilidad del sensor de movimiento, es decir, hasta que distancia detecta movimiento para activar la iluminación. 

## Opción 3 - Control del nivel del agua
Por último, fijará el nivel del agua a la cual debe permanecer siempre para poder controlarla en caso de que ésta aumente a causa de una situación adversa, como por ejemplo, la lluvia. 
 
 
 
 
 
 
 
 
 ## SENSORES :
 ## sensor de temperatura
 El sensor de temperatura para arduino que vamos a utilizar es el DS18B20 :
*fuente*:https://programarfacil.com/blog/arduino-blog/ds18b20-sensor-temperatura-arduino/
El sensor de temperatura DS18B20 es un dispositivo que se comunica de forma digital. Cuenta con tres terminales: Vcc, GND y el pin Data. Este sensor utiliza comunicación por  protocolo serial digital OneWire. Esté protocolo de comunicación permite enviar y recibir datos utilizando un solo cable. A diferencia de otros, que utilizan dos o más líneas de comunicación digital. Para leer el sensor con un Arduino es necesario utilizar dos librerías que deben ser instaladas antes de cargar el código a nuestra placa de desarrollo. Entonces las librerías son las siguientes:

Dallas Temperature.
OneWire
El DS18B20 tiene errores debido a factores externos, al ruido inherente en los circuitos eléctricos y alteraciones en el medio físico.

## Sensor de iluminación
El sensor que iluminación para arduino que vamos a utilizar es el LDR.
Para ello se utilizará un sensor de luz LDR (light-dependent resistor), una resistencia eléctrica y un LED. La idea es que cuando la intensidad luminosa disminuya un cierto umbral, el LED se active
El sensor LDR es un sensor resistivo (fotoresistor), es decir que su resistencia eléctrica varía en función de la luz que recibe
 ## Sensor de nivel del agua

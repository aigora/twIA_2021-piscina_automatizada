# Piscina automatizada

Nuestro proyecto consiste en automatizar una piscina para facilitar al usuario su uso y manejo ante cambios de luz y adversidades meteorólogicas (lluvia y cambios de temperatura y presión); de modo que, en función de cada situación, pueda ser tapada, expulsar agua y activar la iluminación.


## Integrantes del grupo

 1. Jimena López Maldonado - jimenalopez02 .
 2. Aitana Martínez Gayà -  aitana-martinez .
 3. Natalia Miguel Cuenca -  nataliamiguel .


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
Por último, si el agua alcanza una termperatura determinada, el toldo cubirá la piscina para evitar un aumento en el cambio de temperatura.


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

# Código de Arduino
//Jimena
#include <Servo.h>
//Natalia
#include <OneWire.h>
#include <DallasTemperature.h>

//Jimena
#define pinTrigger 10
#define pinEcho 9
//Nat temperatura
OneWire ourWire(2); //Comunico el programa con el pin 2 del Arduino
DallasTemperature sensor(&ourWire);


//Natalia Luces
int analogInPin = A0;
int analogValue = 0;
int led = 13;


//Jimena
int TRIG = 10; //Variable que contiene el número del pin al cual conectemos la señal "trigger"
int ECO = 9; //Variable que contiene el número del pin al cual conectamos la señal "echo"
int estado = 0;
int altura = 26; //Constante altura a la que se encuentra el sensor ultrasonido de la base de la piscina
int DURACION; //Variable duración para la función nivel_agua_consigna
float DISTANCIA; //Variable distancia para la función nivel _agua_consigna
Servo servo1 ;
float distancia; //Variable distancia para la función mostrar_nivel_agua, de tipo float que contendrá la distancia en cm
int numero2;
long tiempo; //Variable tiempo para la función mostrar_nivel_agua

//Aitana:
Servo servoMotor;

void setup()
{
  delay(1000);
  Serial.begin(9600);

  //Natalia
  sensor.begin();
  pinMode(led, OUTPUT);

  //Jimena
  pinMode(pinTrigger, OUTPUT); //Configuramoms el pin de "trigger" como salida
  pinMode(pinEcho, INPUT);  //Configuramoms el pin de "echo" como entrada
  digitalWrite(pinTrigger, LOW);//Ponemos en voltaje bajo(0V) el pin de "trigger"
  // servo1.attach(8);
  //Aitana:
  servoMotor.attach(4); //Iniciamos el servo para que empiece a trabajar con el pin 4
  Serial.begin(9600);
  servoMotor.write(0); //Inicializamos al ángulo 0 el servomotor;


}

void loop()
{
  /* //CREO Q ESTO LO PUEDO BORRAR
    //PARA TEMPERATURA
    sensor.requestTemperatures();
    float temp = sensor.getTempCByIndex(0);

    //PARA LUCES AUTOMATICAS!!!!:
    analogValue = analogRead(analogInPin);
    Serial.println(analogValue);
    delay(10);
    if (analogValue < 800) {
    digitalWrite(led, HIGH);
    } else {
    digitalWrite(led, LOW);
    }
  */


  if (Serial.available() > 0)  // Si hay mensajes procedentes del PC
    procesar_mensaje();
  //Jime:
  switch (estado) {
    case 0: break;
    case 1 :
      if (medir_distancia() > numero2) {
        servo1.write(0);
        estado = 0;
      }
  }


}
// Resto de acciones


//JIMENA:
float medir_distancia(void)
{
  float DISTANCIA;
  long DURACION;

  digitalWrite(TRIG, HIGH);
  delay(1);
  digitalWrite(TRIG, LOW);
  DURACION = pulseIn(ECO, HIGH);
  DISTANCIA = DURACION / 58.2;
  return (DISTANCIA);
}


void procesar_mensaje(void)
{
  int numero1;
  int numero2;
  int numero3;
  float temp;
  String cadena = Serial.readStringUntil('\n'); // Lee mensaje
  String valor = Serial.readStringUntil('\n');  // Lee valor
  numero1 = valor.toInt();  // Transforma valor a entero
  numero2 = valor.toInt();
  numero3 = valor.toInt();

  if (cadena.equals("MOSTRAR_TEMP")) // Entre las comillas se pone el texto del mensaje que se espera
  {
    sensor.requestTemperatures();
    temp = sensor.getTempCByIndex(0);
    temp = temp * 100;
    numero1 = int(temp);
    Serial.println("TEMPERATURA_ACTUAL ");
    Serial.println(numero1);
  }
  else if (cadena.equals("ILUMINACION")) // Y así sucesivamente con todos los posibles mensajes
  {
    /*analogValue = analogRead(analogInPin);
      numero1 = (int)analogValue;
      Serial.println(numero1);*/
    numero1 = digitalRead(led);
    Serial.println(numero1);

  }
  else if (cadena.equals("AUTOMATICO"))
  {
    analogValue = analogRead(analogInPin);
    Serial.println(analogValue);
    delay(10);
    if (analogValue < 800) {
      digitalWrite(led, HIGH);
    } else {
      digitalWrite(led, LOW);
    }

  }
  else if (cadena.equals("APAGA"))
  {
    digitalWrite(led, LOW);
  }
  else if (cadena.equals("ENCIENDE"))
  {
    digitalWrite(led, HIGH);
  }
  //JIMENA:
  else if (cadena.equals("MOSTRAR_NIVEL_AGUA")) // Entre las comillas se pone el texto del mensaje que se espera
  {
    digitalWrite(pinTrigger, HIGH);//Ponemos en voltaje alto(5V) el pin de "trigger"
    delayMicroseconds(10);//Esperamos en esta línea para conseguir un pulso de 10us
    digitalWrite(pinTrigger, LOW);//Ponemos en voltaje bajo(0V) el pin de "trigger"

    tiempo = pulseIn(pinEcho, HIGH);//Utilizamos la función  pulseIn() para medir el tiempo del pulso/echo
    distancia = tiempo * 0.01715; //Obtenemos la distancia considerando que la señal recorre dos veces la distancia a medir y que la velocidad del sonido es 343m/s
    if (distancia >= 140) {
      distancia = 26;
    }
    distancia = distancia * 100;
    numero2 = int(distancia);
    Serial.println("distancia ");
    Serial.println(numero2);
    delay(10);          //Nos mantenemos en esta línea durante 100ms antes de terminar el loop
  }

  //JIMENA:
  else if (cadena.equals("NIVEL_AGUA_CONSIGNA")) // Y así sucesivamente con todos los posibles mensajes
  {
    digitalWrite(TRIG, HIGH);
    delay(1);
    digitalWrite(TRIG, LOW);
    DURACION = pulseIn(ECO, HIGH);
    DISTANCIA = DURACION / 58.2;
    DISTANCIA = DISTANCIA * 100;
    //Serial.println(DISTANCIA);
    delay(200);
    // numero= altura- numero2;
    //Serial.println(numero2);
    if (DISTANCIA <= numero2 && 0 <= DISTANCIA)
    {
      estado = 1;
      servo1.write(90);
    }
    else {
      servo1.write(0);
    }
  }
  //AITANA
  else if (cadena.equals("CERRAR_TOLDO"))
  {
    for (int i = 0; i <= 180; i++)
    {
      servoMotor.write(i);
      delay(3000);
    }
  }
  else if (cadena.equals("ABRIR_TOLDO"))
  {
    for (int i = 179; i > 0; i--)
    {
      servoMotor.write(i);
      delay(3000);
    }
  }
  else if (cadena.equals("TEMP_TOLDO"))
  {
    sensor.requestTemperatures();
    temp = sensor.getTempCByIndex(0);
    temp = temp * 100;
    numero3 = int(temp);
    if (numero3 < 2000) {

    }

  }




}


# Código  Visual
#define MAX_BUFFER 200
#define MAX_INTENTOS_READ 4
#define MS_ENTRE_INTENTOS 250
#define SI 1
#define NO 0

#include <stdio.h>
#include <stdlib.h>
#include <locale.h>
#include <windows.h>
#include <string.h>
#include <conio.h>
#include "SerialClass/SerialClass.h"

// Funciones prototipo
int menu_principal(void);
void configura(void);

void Talk_with_Arduino(Serial* Arduino);
void Send_to_hw(Serial*, char*);
int Receive_from_hw(Serial* Arduino, char* BufferEntrada);
int Send_and_Receive(Serial* Arduino, const char* msg_out, int valor_out, char* msg_in, int* valor_in);
void monitorizar_aforo(Serial*);

void mostrar_temp(Serial*);
void luces(Serial*);
void automatico(Serial*);
void apagar_luces(Serial*);
void encender_luces(Serial*);
//JIMENA
void mostrar_nivel_agua(Serial*);
void elegir_nivel_agua(Serial*);

//AITANA
void toldo(Serial*);
void abrir_toldo(Serial* );
void cerrar_toldo(Serial* );
void temp_toldo(Serial* );


int main(void)
{
	Serial* Arduino;
	char puerto[] = "COM5"; //Puerto serie al que está conectado Arduino
	int opc;  // Opción del menú principal seleccionada

	// Tareas de configuración y carga
	configura();
	Arduino = new Serial((char*)puerto);  // Establece la conexión con Arduino

	// Bucle principal de la aplicación
	do
	{
		opc = menu_principal();
		switch (opc)
		{
		case 1:
			toldo(Arduino);
			break;  
		case 2:
			mostrar_temp(Arduino);
			break;
		case 3:
			luces(Arduino);
			break;
		case 4:
			mostrar_nivel_agua(Arduino);
			break;
		case 5:
			automatico(Arduino);
			break;
		case 6:break;
		}
		printf("\n\n");
	} while (opc != 6);

	// Tareas de desconexión y cierre 
	return 0;
	
}

int menu_principal(void)
{
	int opcion;
	do
	{
		printf("1 - Abrir o cerrar el toldo\n");
		printf("2 - Leer temperatura\n");
		printf("3 - Encendido/Apagado de luces\n");
		printf("4 - Leer nivel del agua\n");
		printf("5 - Automatico\n");
		printf("6 - Cerrar Aplicación\n");
		printf("Seleccione opción: ");
		scanf_s("%d", &opcion);
		if (opcion < 1 || opcion>6)
			printf("\nOpción inexistente.\n\n");
	} while (opcion < 1 || opcion>6);
	return opcion;
}

void mostrar_temp(Serial* Arduino) {
	int bytes_recibidos;
	char mensaje_in[200];
	char mensaje_out[] = "MOSTRAR_TEMP";
	int temp;
	float temp2;
	bytes_recibidos=Send_and_Receive(Arduino, "MOSTRAR_TEMP",1 , mensaje_in, &temp);
	if (bytes_recibidos != 0) {
		temp2 = (float)temp / 100;
		printf("\nLa temperatura es %.2fºC",temp2);
	}
}
void toldo(Serial* Arduino) {
	int opc;
	printf("\nDesea:\n1.Abrir el toldo\n2.Cerrar el toldo");
	scanf_s("%d", &opc);
	switch (opc) {
	case 1:
		abrir_toldo(Arduino);
		break;
	case 2:
		cerrar_toldo(Arduino);
		break;
	}
}

void cerrar_toldo(Serial* Arduino)
{
	int bytes_recibidos;
	char mensaje_in[200];
	char mensaje_out[] = "CERRAR_TOLDO";
	int var;
	bytes_recibidos = Send_and_Receive(Arduino, " CERRAR_TOLDO", 1, mensaje_in, &var);
}

void abrir_toldo(Serial* Arduino)
{
	int bytes_recibidos;
	char mensaje_in[200];
	char mensaje_out[] = "ABRIR_TOLDO";
	int var;

	bytes_recibidos = Send_and_Receive(Arduino, " ABRIR_TOLDO", 1, mensaje_in, &var);
}

void luces(Serial* Arduino) {
	int bytes;
	char mensaje_in[200];
	char mensaje_out[]="ILUMINACION";
	int luz;
	bytes= Send_and_Receive(Arduino, "ILUMINACION", 1, mensaje_in, &luz);
	if (bytes != 0) {
		if (mensaje_in[0]=='1') {
			int siono;
			printf("\nLas luces están encendidas, desea apagarlas?\n\t1.Si\n\t2.No\n");
			scanf_s("%d", &siono);
			switch (siono) {
			case 1: apagar_luces(Arduino);
				break;
			case 2:
				break;
			}
		}
		else {
			int siono;
			printf("\nLas luces están apagadas, desea encenderlas?\n\t1.Si\n\t2.No\n");
			scanf_s("%d", &siono);
			switch (siono) {
			case 1: encender_luces(Arduino);
				break;
			case 2:
				break;
			}
		}
	}
}

void apagar_luces(Serial* Arduino) {
	int bytes;
	char mensaje_entr[200];
	char mensaje_sali[] = "APAGA";
	int valor;
	bytes= Send_and_Receive(Arduino, "APAGA", 1, mensaje_entr, &valor);
}
void encender_luces(Serial* Arduino) {
	int bytes;
	char mensaje_entr[200];
	char mensaje_sali[] = "ENCIENDE";
	int valor;
	bytes = Send_and_Receive(Arduino, "ENCIENDE", 1, mensaje_entr, &valor);
	
}

//Jimena
void mostrar_nivel_agua(Serial* Arduino)
{
	int bytes_recibidos;
	char mensaje_in[255];
	char mensaje_out[] = "MOSTRAR_NIVEL_AGUA";
	float altura = 9.5;
	int distancia;
	float nivel2;
	bytes_recibidos = Send_and_Receive(Arduino, "MOSTRAR_NIVEL_AGUA", -1, mensaje_in, &distancia);
	if (bytes_recibidos != 0)
	{
		nivel2 = (float)distancia / 100;
		nivel2 = altura - nivel2;

		printf("\nEl nivel del agua de su piscina es %.2f cm", nivel2);

	}
}


void automatico(Serial* Arduino) {
	//elegir_nivel_agua(Arduino);
	//temp_toldo(Arduino);
	int bytes_recibidos;
	char mensaje_entr[200];
	char mensaje_sal[] = "AUTOMATICO";
	int var;
	bytes_recibidos = Send_and_Receive(Arduino, "AUTOMATICO", 1, mensaje_entr, &var);
	do {
		automatico(Arduino);
	} while (mensaje_entr[0] < '8');
	
}


void temp_toldo(Serial* Arduino) {
	int bytes_recibidos;
	char mensaje_entr[200];
	char mensaje_sal[] = "TEMP_TOLDO";
	int var;
	bytes_recibidos = Send_and_Receive(Arduino, "TEMP_TOLDO", 1, mensaje_entr, &var);
}


//Jimena
void elegir_nivel_agua(Serial* Arduino)
{
	int bytes_recibidos;
	char mensaje_in[255];
	char mensaje_out[] = "NIVEL_AGUA_CONSIGNA";
	float nivel, x;
	int volumen, altura = 26;
	int a = 12, b = 12, c = 6;
	volumen = a * b * c;
	printf("\nEl volumen de su piscina es %d cm3", volumen);
	printf("\nElija el nivel de agua maximo al que quiere que este su piscina(en cm) :");
	scanf_s("%d", &x);
	nivel = altura - x;
	bytes_recibidos = Send_and_Receive(Arduino, "MOSTRAR_NIVEL_AGUA", nivel, mensaje_in, &altura);

}

void configura(void)
{
	// Establece juego de caracteres castellano
	// Para que funcione hay que partir de un proyecto vacío
	// No utilice la plantilla Aplicación de consola C++
	setlocale(LC_ALL, "spanish");
}

// Ejemplo de función de intercambio de datos con Arduino
void Talk_with_Arduino(Serial* Arduino)
{
	//char BufferSalida[MAX_BUFFER];
	char BufferEntrada[MAX_BUFFER];
	int bytesReceive,numero_recibido;
	
	if (Arduino->IsConnected()) // Si hay conexión con Arduino 
	{

		// Para enviar un mensaje y obtener una respuesta se utiliza la función Send_and_Receive
		// El mensaje está formado por un texto y un entero
		// El mensaje que se recibe está formado también por un texto y un entero.
		// Parámetros de la función:
		// El primero es la referencia a Arduino
		// El segundo es el mensaje que se desea enviar
		// El tercero es un entero que complementa al mensaje que se desea enviar
		// El cuarto es el vector de char donde se recibe la respuesta
		// El quinto es la referencia donde se recibe el entero de la respuesta
		// La función devuelve un entero con los bytes recibidos. Si es cero no se ha recibido nada.

		bytesReceive = Send_and_Receive(Arduino, "GET_AFORO_MAX", -1, BufferEntrada, &numero_recibido);
		if (bytesReceive == 0)
			printf("No se ha recibido respuesta al mensaje enviado\n");
		else
			printf("Mensaje recibido %s %d\n", BufferEntrada, numero_recibido);
	}
	else
		printf("La comunicación con la plataforma hardware no es posible en este momento\n"); // Req 3
}

// Protocolo de intercambio mensajes entre Pc y platforma hardware
// Envío Mensaje valor
// Recibe Mensaje valor
// Retorna bytes de la respuesta (0 si no hay respuesta)
int Send_and_Receive(Serial* Arduino, const char* msg_out, int valor_out, char* msg_in, int* valor_in)
{
	char BufferSalida[MAX_BUFFER];
	char BufferEntrada[MAX_BUFFER];
	char* ptr;
	int bytesReceive;

	sprintf_s(BufferSalida, "%s\n%d\n", msg_out, valor_out);
	Send_to_hw(Arduino, BufferSalida);
	bytesReceive = Receive_from_hw(Arduino, BufferEntrada);
	if (bytesReceive != 0)
	{
		ptr = strpbrk(BufferEntrada, " ");
		if (ptr == NULL)
			*valor_in = -1;
		else
		{
			*valor_in = atoi(ptr);
			*ptr = '\0';
		}
		strcpy_s(msg_in, MAX_BUFFER, BufferEntrada);
	}
	return bytesReceive;
}


// Envía un mensaje a la plataforma hardware
void Send_to_hw(Serial* Arduino, char* BufferSalida)
{
	Arduino->WriteData(BufferSalida, strlen(BufferSalida));
}

//Recibe (si existe) un mensaje de la plataforma hardware
//Realiza MAX_INTENTOS_READ para evitar mensajes recortados
int Receive_from_hw(Serial* Arduino, char* BufferEntrada)
{
	int bytesRecibidos, bytesTotales = 0;
	int intentos_lectura = 0;
	char cadena[MAX_BUFFER];

	BufferEntrada[0] = '\0';
	while (intentos_lectura < MAX_INTENTOS_READ)
	{
		cadena[0] = '\0';
		bytesRecibidos = Arduino->ReadData(cadena, sizeof(char) * (MAX_BUFFER - 1));
		if (bytesRecibidos != -1)
		{
			cadena[bytesRecibidos] = '\0';
			strcat_s(BufferEntrada, MAX_BUFFER, cadena);
			bytesTotales += bytesRecibidos;
		}
		intentos_lectura++;
		Sleep(MS_ENTRE_INTENTOS);
	}
	return bytesTotales;
}

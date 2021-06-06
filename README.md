# PR61: Procedimiento

Empezamos el programa añadiendo 2 nuevas librerias:
-  SPI -> Permite establecer conexiones con dispositivos SPI
-  SD -> Nos permite leer y escribir en SD
```
#include <Arduino.h>
#include <SPI.h>
#include <SD.h>
```
Después empieza el void setup() , este en la primera línea inicializa una comunicación en serie a una velocidad de 9600 bauds, y en la segunda se hace una impresión por pantalla.
```
Serial.begin(9600);
Serial.print("Iniciando SD ...");
```
A continuación tenemos que indicar los pines de conexión para el bus SPI.
```
SPI.begin(18,19,23,4);
```
Añadimos un if el cual comprueba si la función SD.begin() en el pin 4 se ha iniciado, se imprime por pantalla "No se pudo inicializar".
```
if (!SD.begin(4)) {
Serial.println("No se pudo inicializar");
return;
}
```
En la siguiente línea imprimimos lo siguiente:
```
Serial.println("inicializacion exitosa"); 
```
Después abrimos un archivo con SD.open() . En caso de que no exista el fichero, el parámetro FILE_WRITE permite que se cree el archivo y se abra immediatamente.
```
myFile = SD.open("/archivo.txt",FILE_WRITE);
```
Añadimos algunas lineas de texto con myFile.println() y lo cerramos
```
myFile.println("Hola mundo!!");
myFile.close();
```
Añadimos un segundo if en el que detecte si se ha abierto ya el archivo para, asi, poder imprimir el texto por pantalla con el while(myFIle.available()) y por ultimo cerramos este archivo con myFile.close para que no quede abierto y desprotegido.
```
if (myFile) {
Serial.println("archivo.txt:");
while (myFile.available()) {
Serial.write(myFile.read());
}
myFile.close();
```
Añadimos, finalmente, un else al programa de manera que si surge algun error al abrir el archivo, el programa muestre este mensaje por pantalla:
```
else {
Serial.println("Error al abrir el archivo");
}
```
### Resultado de la simulación

Al compilar el programa de manera correcta, el mensaje que deberiamos poder ver sería el siguiente:
```
Iniciando SD ...sd init ok
inicializacion exitosa
Hola mundo!!
```
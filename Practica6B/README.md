## PRACTICA 6B
En esta practica utilizaremos un lector de tarjetas RFID a trabes del bus de conexion SPI 
**Codigo**
```cpp
#include <SPI.h>
#include <MFRC522.h>

#define RST_PIN	9    //Pin 9 para el reset del RC522
#define SS_PIN	10   //Pin 10 para el SS (SDA) del RC522
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522

void setup() {
	Serial.begin(9600); //Iniciamos la comunicación  serial
	SPI.begin();        //Iniciamos el Bus SPI
	mfrc522.PCD_Init(); // Iniciamos  el MFRC522
	Serial.println("Lectura del UID");
}

void loop() {
	// Revisamos si hay nuevas tarjetas  presentes
	if ( mfrc522.PICC_IsNewCardPresent()) 
        {  
  		//Seleccionamos una tarjeta
            if ( mfrc522.PICC_ReadCardSerial()) 
            {
                  // Enviamos serialemente su UID
                  Serial.print("Card UID:");
                  for (byte i = 0; i < mfrc522.uid.size; i++) {
                          Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
                          Serial.print(mfrc522.uid.uidByte[i], HEX);   
                  } 
                  Serial.println();
                  // Terminamos la lectura de la tarjeta  actual
                  mfrc522.PICC_HaltA();         
            }      
	}	
}

```
**Librerias**

```cpp
#include <SPI.h>
#include <MFRC522.h>
```

En este programa como en el anterior apartado utilizamos la libreria SPI  ademas y la libreria MFRC552 para poder utilizar el lector RFID

**Declaraciones**
```cpp
#define RST_PIN	9    //Pin 9 para el reset del RC522
#define SS_PIN	10   //Pin 10 para el SS (SDA) del RC522
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522
```
Definimos como variables los pines a los cuales conectaremos el sensor y ademas crearemos un objeto MFRC22 que nos permitira trabajar con el sensor

**SETUP**
```cpp
void setup() {
	Serial.begin(9600); //Iniciamos la comunicación  serial
	SPI.begin();        //Iniciamos el Bus SPI
	mfrc522.PCD_Init(); // Iniciamos  el MFRC522
	Serial.println("Lectura del UID");
}
```
En el void setup inicializamos el Serial ,el bus SPI ,El sensor , y mostramos por pantalla un mensaje de cabezera antes de entrar en el main loop 


**LOOP**

```cpp
void loop() {
	// Revisamos si hay nuevas tarjetas  presentes
	if ( mfrc522.PICC_IsNewCardPresent()) 
        {  
  		//Seleccionamos una tarjeta
            if ( mfrc522.PICC_ReadCardSerial()) 
            {
                  // Enviamos serialemente su UID
                  Serial.print("Card UID:");
                  for (byte i = 0; i < mfrc522.uid.size; i++) {
                          Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " ");
                          Serial.print(mfrc522.uid.uidByte[i], HEX);   
                  } 
                  Serial.println();
                  // Terminamos la lectura de la tarjeta  actual
                  mfrc522.PICC_HaltA();         
            }      
	}	
}
```

Eel loop el primer paso es revisar si hay tarjetas nuevas seleccionamos una tarjeta y nos mostrara por pantalla los datos de esta(las tarjetas rfid cuentan con una id que se mostrara al ser leida por el sensor).

Video DEMO: https://drive.google.com/file/d/1Hr52LumNnmWtXBqdcvVoYp45VV22vtV2/view?usp=sharing
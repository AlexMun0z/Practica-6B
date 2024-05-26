# Practica-6B
## Part B: Lectura d'etiqueta RFID 
### Codi complet
```cpp
#include <Arduino.h>
#include <MFRC522.h>

#define RST_PIN 9    //Pin 9 para el reset del RC522 
#define SS_PIN 10   //Pin 10 para el SS (SDA) del RC522 
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
### Funcionament per parts del codi

__1. Inclusió de Biblioteques__
```cpp
#include <Arduino.h>
#include <MFRC522.h>

```
- Arduino.h:
- MFRC522.h:

__2. Definició de pins i creació de l'objecte MFRC522__
```cpp
#define RST_PIN 9    //Pin 9 para el reset del RC522 
#define SS_PIN 10   //Pin 10 para el SS (SDA) del RC522 
MFRC522 mfrc522(SS_PIN, RST_PIN); //Creamos el objeto para el RC522 
```
Aquesta part del codi verifica si les configuracions de Bluetooth estan habilitades (CONFIG_BT_ENABLED i CONFIG_BLUEDROID_ENABLED).
Si no es el cas, es genera un error i comunica a l'usuari que ha d'executar en el terminal `make menuconfig` per habilitar les configuracions de Bluetooth.

__3. Setup__
```cpp
void setup() {
  Serial.begin(9600); //Iniciamos la comunicación  serial 
  SPI.begin();        //Iniciamos el Bus SPI 
  mfrc522.PCD_Init(); // Iniciamos  el MFRC522 
  Serial.println("Lectura del UID"); 
}
```
S'estableix la configuració inicial del programa:
- Serial.begin(9600): Inicialitza la comunicació sèrie en 9600 bauds.
- SPI.begin();: Inicia el bus SPI, que és el protocol de comunicació utilitzat per interactuar amb el mòdul MFRC522.
- mfrc522.PCD_Init();: Inicia el mòdul MFRC522.
- Serial.println("Lectura del UID"): Imprimeix un missatge inicial al monitor sèrie indicant que es començarà la lectura de UID.
  
__4. Loop__
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
- if (mfrc522.PICC_IsNewCardPresent()): Verifica si hi ha una nova targeta present a prop del lector RFID.
- if (mfrc522.PICC_ReadCardSerial()): Si hi ha una targeta present, intenta llegir el seu número de sèrie (UID).
- Serial.print("Card UID:"): Imprimeix el missatge "Card UID:" al monitor sèrie.
- for (byte i = 0; i < mfrc522.uid.size; i++): Itera a través de cada byte del UID de la targeta.
- Serial.print(mfrc522.uid.uidByte[i] < 0x10 ? " 0" : " "): Imprimeix un espai i, si el byte és menor que 0x10, també imprimeix un 0 addicional per assegurar que tots els bytes s'imprimeixin amb dos dígits.
- Serial.print(mfrc522.uid.uidByte[i], HEX): Imprimeix el byte del UID en format hexadecimal.
- Serial.println(): Imprimeix una nova línia després d'imprimir tot el UID.
- mfrc522.PICC_HaltA(): Finalitza la comunicació amb la targeta actual.

En resum, el codi configura el lector RFID MFRC522 per llegir la UID de qualsevol targeta que es presenti i envia aquesta informació al monitor sèrie.

## Arduino 1 :

```c
#include <SoftwareSerial.h>
SoftwareSerial mySerial(10, 11); // RX, TX//on informe le microcontrôleur que l'on utilise ses broches pour une connexion série
String readString;
 
int led = 2;//variable led connectée sur la pin 2 de la carte
 
const byte bouton = A0;//variable du bouton connecté sur la pin A0 de la carte
const byte bouton1 = A1;//variable du bouton connecté sur la pin A1 de la carte
int etatOUV;// variable lecture de la position du bouton
int etatFER;// variable lecture de la position du bouton
 
 
void setup() {
 
  mySerial.begin(9600);
 
  Serial.begin (57600);
  Serial.println ("Lycée Charles Carnus");
  delay (1000);
  Serial.println("");
  Serial.println ("Je dois recevoir la lettre a ou b pour que ma led puisse s'allumer ou s'eteindre");
 
  pinMode(led , OUTPUT);
  pinMode (bouton, INPUT_PULLUP);// ne pas oublier la résistance de PULLUP Attention!!!
  pinMode (bouton1, INPUT_PULLUP);//les commandes sont inversés
}
void loop() {
 
  etatOUV = digitalRead(bouton);//etatOUV lit la position du bouton
 
  if (etatOUV == 0) {//si elle est égale à 0 (donc à 1)
 
    mySerial.write("c");//on envoie la lettre c sur la connexion série 10,11
    Serial.println("c");
 
  } delay (50);
 
  etatFER = digitalRead(bouton1);//etatFER lit la position du bouton
 
  if (etatFER == 0) {//si elle est égale à 0 (donc à 1)
 
    mySerial.write("d");//on envoie la la lettre d sur la connexion série 10,11
    Serial.println("d");
  }  delay (50);
 
  while (mySerial.available()) {
    delay(10);
    char c = mySerial.read();
    readString += c;
  }
 
  if (readString.length() > 0) {//si je reçois des valeurs
    Serial.println(readString);//j'imprime ses valeurs
 
    if (readString.indexOf("a") != -1)//si je reçois a
    {
      digitalWrite(led , HIGH);//j'allume ma led
    }
 
    if (readString.indexOf("b") != -1)//si je reçois b
    {
      digitalWrite(led , LOW);//j'éteinds ma led
    }
    readString = "";
  }
}
```


## Arduino 2 :

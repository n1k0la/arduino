/////////////
/* INCLUDES:*/

#include <Wire.h>                   // Library for I2C communication
#include <LiquidCrystal_I2C.h>      // Library for LCD, https://github.com/johnrickman/LiquidCrystal_I2C

// Wiring: SDA pin is connected to A4 and SCL pin to A5. Connect to LCD via I2C, default address 0x27 (A0-A2 not jumpered)


///////////////
/* VARIABLES:*/
LiquidCrystal_I2C lcd = LiquidCrystal_I2C(0x27, 16, 2); // Change to (0x27,16,2) for 16x2 LCD.


// Variables pour le modules: 
int trig = 8;// yellow wire
int echo = 9;// green wire
long lecture_echo;
long mesure = 0;
long m3 = 0;

//Variables pour module voltmetre:
int value = 0;
float R1 = 12352.0;
float R2 = 17647.0;
float voltage = 0;



///////////////////////////////
/* PROTOTYPES & DECLARATIONS: */
// Affiche les informations matriels. 
void infoHardware(){
   lcd.begin(16,2); // (20,4) Ecran 20 caracteres, de 4 lignes)
   lcd.setCursor(0,0);
   lcd.print ("Water tank level"); // 16 caractères.
   lcd.setCursor(0,1);
   lcd.print ("Firmware v1.3"); // 14 caracteres.
   delay (2500);
   lcd.setCursor(0,0);
   lcd.print ("Hware: proto"); // 16 caracteres
   lcd.setCursor(0,1);
   lcd.print ("A DEWULF"); // 14
  delay (2500);
  lcd.clear();
}

// Mesure volume
void mesureVolume(){
  digitalWrite(trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(trig, LOW);
  lecture_echo = pulseIn(echo,HIGH);
  mesure = (((lecture_echo /58)-53)/18.2);
  m3 = (10-mesure);
}

// Mesure volt
void mesureVolt(){
  value = analogRead(A0);
  voltage = (value * (5.0/1024)*((R1 + R2)/R2))-0.1;
}

////////////
/* SETUP: */

void setup(){

   // Initialisation des broches pour le modules HC-SR04. 
     pinMode(echo, INPUT);
     pinMode(trig, OUTPUT);
     digitalWrite(trig, LOW);
   
   // Initialisation du port serie, futur base pour la fonction LoRa. 
     Serial.begin(9600);
     Serial.println ("Adrien Dewulf");

   // Initiate the LCD:
     lcd.init();        //I2C 
     lcd.backlight();   //I2C
     lcd.begin(16,2);   
}


///////////
/* LOOP: */

   void loop() {
     infoHardware();
      
  do{
     // Effectue les mesures.
     mesureVolt();
     mesureVolume();
                                                                                                                                       //  printResultat();
     lcd.setCursor(0, 0);        // Set the cursor on the first column and first row.
     lcd.print("Vol:");          // Print the string "Vol: pour indiquer le volume de la citerne.
     lcd.setCursor(0, 7);
     lcd.print(m3);
  
     lcd.setCursor(0, 1);        //Set the cursor on the third column and the second row (counting starts at 0!).
     lcd.print("U: ");
     lcd.setCursor(4, 1); // 
     lcd.print(voltage);
     lcd.setCursor(9,1);
     lcd.print("V"); 
     Serial.print("U= "); Serial.print(voltage);  Serial.print("V");  
     delay(500);
  
     lcd.clear();
 
  } while (1);
}

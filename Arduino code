#include "LedControl.h"

#define LDR_PIN         A0
#define MAX_ADC_READING 1023
#define ADC_REF_VOLTAGE   5.0
#define REF_RESISTANCE    10000
#define LUX_CALC_SCALAR   12518931
#define LUX_CALC_EXPONENT -1.405

//int ldrRawData;
int lightLevel;
float resistorVoltage, ldrVoltage;
float ldrResistance;
float ldrLux;
float OD;
float I0 = 35.5; //lux incident
/*Now we need a LedControl to work with.
* These pin numbers will probably not work with your hardware *
pin 12 is connected to the DataIn
pin 11 is connected to the CLK
pin 10 is connected to LOAD
We have only a single MAX72XX.*/
LedControl lc=LedControl(12,11,10,1);
/* we always wait a bit between updates of the display */
unsigned long delaytime=500;
float duration; //micro second

//const int lightPin = A0;
//const int ledPin = 9;

void setup() {

  /* The MAX72XX is in power-saving mode on startup, we have to do a wakeup call */
  lc.shutdown(0,false);
  /* Set the brightness to a medium values */
  lc.setIntensity(0,2);
  /* and clear the display */
  lc.clearDisplay(0);

// initialize serial communication:
  Serial.begin(9600);
  delay(delaytime);
  MyDisplay(0); //to show 0000

}

void loop() {
  lightLevel = analogRead(LDR_PIN);
  resistorVoltage = (float)lightLevel / MAX_ADC_READING * ADC_REF_VOLTAGE;   // convert digital back to voltage
  ldrVoltage = ADC_REF_VOLTAGE - resistorVoltage;           // supply - resistance
  ldrResistance = ldrVoltage / resistorVoltage * REF_RESISTANCE;        // resistance per voltage
  ldrLux = LUX_CALC_SCALAR * pow(ldrResistance, LUX_CALC_EXPONENT);     // resistance converted to lux
  OD = log10(I0/ldrLux);
  
  
//  lightLevel = map(lightLevel, 0, 900, 0, 255);
//  lightLevel = constrain(lightLevel, 0, 255);
  //analogWrite(ledPin, lightLevel);

  
//  Serial.println(lightLevel);
//    MyDisplay(lightLevel);
  Serial.print("Resistance : ");
  Serial.println(ldrResistance);
  Serial.print("    Lux : ");
  Serial.println(ldrLux);
  Serial.print("  OD : ");
  Serial.println(OD);
    MyDisplay(OD);
      delay(1200);
  }

void MyDisplay(int VALUE){
    int v =VALUE;
    int ones;
    int tens;
    int hundreds;
    int thousands;
 
    ones=v%10;
    v=v/10;
    tens=v%10;
    v=v/10;
    hundreds=v%10;   
    v=v/10;
    thousands=v%10;  
 
    lc.clearDisplay(0);
    //Now print the number digit by digit
    lc.setDigit(0,3,(byte)thousands,false);
    lc.setDigit(0,2,(byte)hundreds,false);
    lc.setDigit(0,1,(byte)tens,false);
    lc.setDigit(0,0,(byte)ones,false);

//    lc.setDigit(1,3,(byte)0,false);
//    lc.setDigit(1,2,(byte)0,false);
//    lc.setDigit(1,1,(byte)0,false);
//    lc.setDigit(1,0,(byte)0,false);    


}

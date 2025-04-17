# Fire-Fighting
/*------ Arduino Fire Fighting Robot Code by hobby project---- */
 
#include <Servo.h>  //include servo.h library
Servo myservo;
 
int pos = 0;    
boolean fire = false;
#define Left 4    // left sensor
#define Right 2    // right sensor
#define Forward 3  //front sensor
#define M1 11     // left motor
#define M2 10       // left motor
//#define RM1 4       // right motor
//#define RM2 5       // right motor
#define pump 13
void setup()
{
  pinMode(Left, INPUT);
  pinMode(Right, INPUT);
  pinMode(Forward, INPUT);
  pinMode(M1, OUTPUT);
  pinMode(M2, OUTPUT);
 // pinMode(RM1, OUTPUT);
  //pinMode(RM2, OUTPUT);
  digitalWrite(M1,LOW);
  digitalWrite(M2,LOW);
  pinMode(pump, OUTPUT);
  //digitalWrite(pump, HIGH);
  delay(2000);
  myservo.attach(12);
  myservo.write(90);
  Serial.begin(9600); 
}
 
void put_off_fire()
{
    delay (500);
 
   // digitalWrite(LM1, HIGH);
    //digitalWrite(LM2, HIGH);
    //digitalWrite(RM1, HIGH);
    //digitalWrite(RM2, HIGH);
    
   digitalWrite(pump, HIGH);
   delay(500);
    
    for (pos = 60; pos <= 150; pos += 1) { 
    myservo.write(pos); 
    delay(10);  
  }
  for (pos = 150; pos >= 60; pos -= 1) { 
    myservo.write(pos); 
    delay(10);
  }
  
  digitalWrite(pump,LOW);
  myservo.write(90);
  
  fire=false;
}
 
void loop()
{

  //Serial.println(digitalRead(4));
  //Serial.println(digitalRead(8));
  Serial.print("Left = ");
  Serial.println(digitalRead(4));

  delay(50);
   myservo.write(90); //Sweep_Servo();  
 
    if (digitalRead(Left) ==1&& digitalRead(Right)==1 && digitalRead(Forward) ==1) 
    {
    
    digitalWrite(M1, LOW);
    digitalWrite(M2, LOW);
    //digitalWrite(RM1, HIGH);
    //digitalWrite(RM2, HIGH);
    }
    
    else if (digitalRead(Forward) ==0) 
    {
   digitalWrite(M1, HIGH);
   digitalWrite(M2, HIGH);
   delay(500);
    digitalWrite(M1, LOW);
    digitalWrite(M2, LOW);
    fire = true;
    }
    
    else if (digitalRead(Left) ==0)
    {
     digitalWrite(M1, HIGH);
     digitalWrite(M2, LOW);
    // digitalWrite(RM1, HIGH);
    // digitalWrite(RM2, HIGH);
    }
    
    else if (digitalRead(Right) ==0) 
    {
     digitalWrite(M1, LOW);
     digitalWrite(M2, HIGH);
    // digitalWrite(RM1, HIGH);
    // digitalWrite(RM2, LOW);
    }
    
delay(300);//change this value to increase the distance
 
     while (fire == true)
     {
      put_off_fire();
     }
}

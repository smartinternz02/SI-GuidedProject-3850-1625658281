// C++ code
//
#include<Servo.h>

#define Trig 2
#define Echo 4
#define Door 11 // Servo signal

 float duration,distance;

Servo door;

void door_Open()
{
  delay(750);
  door.write(255);
}

void door_Close()
{
  delay(750);
  door.write(0);
}

float dis_Calc()
{
  
  digitalWrite(Trig,LOW);
  digitalWrite(Trig,HIGH);
  delayMicroseconds(10);
  
  duration = pulseIn(Echo,HIGH);
  
  distance = (duration * 0.034)/2; // calculating distance btw door and car
   
  return distance;
  
}

void setup()
{
  door.attach(Door);
  pinMode(Trig ,OUTPUT);
  pinMode(Echo ,INPUT );
  Serial.begin(9600);
}

void loop()
{
  float pre_Dis;
 
  dis_Calc();
  
  if(distance < 100)
  {
    door_Open();
    Serial.print("\n Door Status : 'OPEN' ");
  }
 else if(distance <= 0)
 {
   if(pre_Dis < 100)
   {
   		Serial.print("\n Door Status : 'OPEN' ");
   }
   else
   {
     Serial.print("\n Door Status : 'CLOSED' ");
   }
     
 }
  else
  {
    door_Close();
    Serial.print("\n Door Status : 'CLOSED' ");
  }
  
  pre_Dis = dis_Calc();
  
  
}
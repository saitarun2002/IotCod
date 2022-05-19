# IotCod
#temp Code
#include <ESP8266WiFi.h>
#include<Adafruit_MQTT_Client.h>
              
#define wifi "projects"
#define password "projects1234"
#define server "io.adafruit.com"

#define port 1883
#define username "Sai_Keerthana"
#define key "aio_QzKf72HzI895NoYt4BEM5y8M8Fdk"

WiFiClient esp;
Adafruit_MQTT_Client mqtt(&esp,server,port,username,key);

// publishing feed
Adafruit_MQTT_Publish T_data=Adafruit_MQTT_Publish(&mqtt,username"/feeds/Temp_data");
float vref = 3.3;
float resolution = vref/1023;
void setup()
{
Serial.begin(9600); 
delay(10);
  Serial.println("Connecting to ");
  Serial.println(wifi);
  WiFi.begin(wifi,password);
  while (WiFi.status()!=WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }

  Serial.println("WiFi Connected");
  Serial.println("IP Address:");
  Serial.println(WiFi.localIP());
  Serial.print("Connecting to MQTT");

  while(mqtt.connect())
  {
    Serial.print(".");
  }
} 
void loop()
{

 float temperature = analogRead(A0);
 temperature = (temperature*resolution);
 temperature = temperature*100;
 Serial.println(temperature);
 delay(1000);
 if(mqtt.connected())
   {
    int T_stat=(int)temperature;
    Serial.println("\n T_stat");
    Serial.println(T_stat);
    Serial.print("....");
    if(T_data.publish(T_stat))
    {
      Serial.println("Success");
    }
    else
    {
      Serial.println("Fail!");
    }
    delay(2000);
  }
}

#ultra sonic
#include <ESP8266WiFi.h>
#include<Adafruit_MQTT_Client.h>
              
#define wifi "wifi "
#define password "password "
#define server "io.adafruit.com"

#define port 1883
#define username "your name in adafruit "
#define key "your key link"

WiFiClient esp;

Adafruit_MQTT_Client mqtt(&esp,server,port,username,key);

// publishing feed
Adafruit_MQTT_Publish US_data=Adafruit_MQTT_Publish(&mqtt,username"/feeds/US_data");

const int pingPin =D1; // Trigger Pin of Ultrasonic Sensor
const int echoPin =D0; // Echo Pin of Ultrasonic Sensor


void setup()
{
  Serial.begin(9600);
  delay(10);
  Serial.println("Connecting to ");
  Serial.println(wifi);
  WiFi.begin(wifi,password);       // establish wifi connection

  while (WiFi.status()!=WL_CONNECTED)
  {
    delay(500);
    Serial.print(".");
  }

  Serial.println("WiFi Connected");
  Serial.println("IP Address:");
  Serial.println(WiFi.localIP());
  Serial.print("Connecting to MQTT");

  while(mqtt.connect())
  {
    Serial.print(".");
  }
}

void loop()
{
  long duration, inches, cm;
   pinMode(pingPin, OUTPUT);
   digitalWrite(pingPin, LOW);
   delayMicroseconds(2);
   digitalWrite(pingPin, HIGH);
   delayMicroseconds(10);
   digitalWrite(pingPin, LOW);
   pinMode(echoPin, INPUT);
   duration = pulseIn(echoPin, HIGH);
   inches = microsecondsToInches(duration);
   cm = microsecondsToCentimeters(duration);
   Serial.print(inches);
   Serial.print("in, ");
   Serial.print(cm);
   Serial.print("cm");
   Serial.println();
   delay(100);
  if(mqtt.connected())
  {
    int US_stat=(int)inches;
    Serial.println("\n US_stat");
    Serial.println(US_stat);
    Serial.print("....");
    if(US_data.publish(US_stat))
    {
      Serial.println("Success");
    }
    else
    {
      Serial.println("Fail!");
    }
    delay(2000);
  }
}
long microsecondsToInches(long microseconds) {
   return microseconds / 74 / 2;
}

long microsecondsToCentimeters(long microseconds) {
   return microseconds / 29 / 2;
}

# IOT based flood monitoring system

A flood is an overflow of water that submerges land that is usually dry. In the sense of "flowing water", the word may also be applied to the inflow of the tide. Floods are an area of study of the discipline hydrology and are of significant concern in agriculture, civil engineering and public health. Human changes to the environment often increase the intensity and frequency of flooding, for example land use changes such as deforestation and removal of wetlands, changes in waterway course or flood controls such as with levees, and larger environmental issues such as climate change and sea level rise. In particular climate change's increased rainfall and extreme weather events increases the severity of other causes for flooding, resulting in more intense floods and increased flood risk.

The purpose of this project is to sense the water level in river beds and check if they are in normal condition. If they reach beyond the limit, then it alerts people through LED signals with buzzer sound as well as internet applications.

This project firmware is based on Arduino.




## Components required for making this project

Hardware requirement

•	Node MCU ESP8266 Development Board

•	Ultrasonic HC-SR04 Sensor

•	Red and Green LEDs

•	Buzzer

•	Jumper wire

•	Breadboard

•	Power Supply

Software requirement

•	Arduino IDE

•	Thingspeak IOT platform

## About ThingSpeak IOT Platform

After the successful interface of the hardware parts according to the circuit diagram above. Now it’s time to set up the IoT platform, where data can be stored for online monitoring. Here we are using ThingSpeak to store data. ThingSpeak is a very popular IoT cloud platform that is used to store, monitor, and process data online. 
ThingSpeak is an IOT analytics platform service that allow you to aggregate, visualize, and analyse live data stream in the cloud. You can send data to ThingSpeak from your devices, create instant visualization of live data, and send alert.

<img src="https://firebasestorage.googleapis.com/v0/b/iot-based-flood-monitor.appspot.com/o/Capture.PNG?alt=media&token=5875b904-9382-4af4-a884-6bb1b79401a1"></img>

<img src="https://firebasestorage.googleapis.com/v0/b/iot-based-flood-monitor.appspot.com/o/Ce.PNG?alt=media&token=a7880898-12b6-4fc8-8eaa-0ba4536b58f0"></img>

<img src="https://firebasestorage.googleapis.com/v0/b/iot-based-flood-monitor.appspot.com/o/2e.PNG?alt=media&token=4da0b4cc-f984-425e-9355-67c61a1ad26b"></img>

## Block diagram & Circuit diagram


<img src="https://firebasestorage.googleapis.com/v0/b/iot-based-flood-monitor.appspot.com/o/Block.png?alt=media&token=491db85c-c5d4-49fe-a425-0b0c3e9b8b75"></img>

The block diagram above shows the working of this IoT based flood monitoring system using the Node MCU and IoT Platform. Here, the Ultrasonic sensor is used to detect river water levels. The raw data from the ultrasonic sensor is fed to the Node MCU, where it is processed and sent to ThingSpeak for graphical monitoring and critical alerts. Here, the red LED and Buzzer is used to send an alert in a flooded condition. While Green LED is used for indicating Normal condition. are used to be alert in severe flood conditions, and green LEDs are used to indicate normal conditions.The block diagram above shows the working of this IoT based flood monitoring system using the Node MCU and IoT Platform. Here, the Ultrasonic sensor is used to detect river water levels. The raw data from the ultrasonic sensor is fed to the Node MCU, where it is processed and sent to ThingSpeak for graphical monitoring and critical alerts. Here, the red LED and Buzzer is used to send an alert in a flooded condition. While Green LED is used for indicating Normal condition. are used to be alert in severe flood conditions, and green LEDs are used to indicate normal conditions.


<img src="https://firebasestorage.googleapis.com/v0/b/iot-based-flood-monitor.appspot.com/o/circuit.PNG?alt=media&token=5cc313bf-8782-4a6d-a9b2-d598d7ec8808"></img>
## Code for this project

```javascript
/*embed*/

#include "ThingSpeak.h"
#include <ESP8266WiFi.h>
const int trigPin1 = D1;
const int echoPin1 = D2;
#define redled D3
#define grnled D4
#define BUZZER D5 
unsigned long ch_no = 1053193;
const char * write_api = "***************";
char auth[] = "****************";
char ssid[] = "*****************";
char pass[] = "*****************";
unsigned long startMillis;
unsigned long currentMillis;
const unsigned long period = 10000;
WiFiClient  client;
long duration1;
int distance1;
void setup()
{
pinMode(trigPin1, OUTPUT);
pinMode(echoPin1, INPUT);
pinMode(redled, OUTPUT);
pinMode(grnled, OUTPUT);
digitalWrite(redled, LOW);
digitalWrite(grnled, LOW);
Serial.begin(115200);
WiFi.begin(ssid, pass);
while (WiFi.status() != WL_CONNECTED)
{
delay(500);
Serial.print(".");
}
Serial.println("WiFi connected");
Serial.println(WiFi.localIP());
ThingSpeak.begin(client);
startMillis = millis();  //initial start time
}
void loop()
{
digitalWrite(trigPin1, LOW);
delayMicroseconds(2);
digitalWrite(trigPin1, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin1, LOW);
duration1 = pulseIn(echoPin1, HIGH);
distance1 = duration1 * 0.034 / 2;
Serial.println(distance1);
if (distance1 <= 4)
{
digitalWrite(D3, HIGH);
tone(BUZZER, 300);
digitalWrite(D4, LOW);
delay(1500);
noTone(BUZZER);
}
else
{
digitalWrite(D4, HIGH);
digitalWrite(D3, LOW);
}
currentMillis = millis();
if (currentMillis - startMillis >= period)
{
ThingSpeak.setField(1, distance1);
ThingSpeak.writeFields(ch_no, write_api);
startMillis = currentMillis;
}
}



# smart-smoke-detector
#define BLYNK_TEMPLATE_ID "TMPL3eyfA8dbJ"
#define BLYNK_TEMPLATE_NAME "Gas Leakage"
#define BLYNK_AUTH_TOKEN "1g4mUfB_2Vp-RiiMJf3SozkyBA17UbXv"
#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#define BUZZER_PIN D1
char auth[] = "1g4mUfB_2Vp-RiiMJf3SozkyBA17UbXv";
char ssid[] = "ASHEEKA"; // type your wifi name
char pass[] = "12345678"; // type your wifi password
int smokeA0 = A0;
int data = 0;
int sensorThres = 100;
BlynkTimer timer;
void sendSensor(){
int data = analogRead(smokeA0);
Blynk.virtualWrite(V0, data);
 Serial.print("Pin A0: ");
 Serial.println(data);
 if(data > 500){
 //Blynk.email("test@gmail.com", "Alert", "Gas Leakage Detected!");
 Blynk.logEvent("gas_alert","Gas Leakage Detected");
 digitalWrite(BUZZER_PIN, HIGH);
 }
}
void setup(){
 pinMode(smokeA0, INPUT);
 Serial.begin(115200);
 Blynk.begin(auth, ssid, pass);
 //dht.begin();
 timer.setInterval(2500L, sendSensor);
 pinMode(BUZZER_PIN, OUTPUT);
}
void loop(){
 Blynk.run();
 timer.run();
}

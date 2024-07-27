# http request in ESP32 code to get the data from database

## prject goal:
use http request in ESP32 code to get the last data enterd the database, i will use a LED to test the process.

## project requirements:
- a simulator: https://wokwi.com
- a webpage retern the last value entered the database(in my case i am using: https://s-m.com.sa/f.html

## start building the circuit 
- connect the ESP32 with the LED
- make sure that the LED's anode (long leg) is connected to the correct digital pin(in my cace it is 25), and the cathode (short leg) is connected to a ground (GND) pin on the ESP32 board.

## the code:
```
#include<WiFi.h>
#include <HTTPClient.h>
const char* ssid = "Wokwi-GUEST";
const char* pass = "";
const char* serverUrl = "https://s-m.com.sa/f.html";
unsigned const long interval = 2000;
unsigned long zero = 0;

const int ledPin = 25; // choose a pin for the LED
void setup(){
  Serial.begin(115200);
  pinMode(ledPin, OUTPUT); // set the LED pin as an output
  WiFi.begin(ssid, pass);
  while(WiFi.status() != WL_CONNECTED){
    delay(100);
    Serial.println(".");
  }
  Serial.println("WiFi Connected!");
  Serial.println(WiFi.localIP());
}
void loop() {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    http.begin(serverUrl);
    int httpResponseCode = http.GET();
    if (httpResponseCode > 0) {
      String response = http.getString();
      Serial.println(response); // print the last inserted value
      if (response == "forward") {
        digitalWrite(ledPin, HIGH); // turn the LED on
      } else {
        digitalWrite(ledPin, LOW); // turn the LED off
      }
    } else {
      Serial.println("Error: " + String(httpResponseCode));
    }
    http.end();
  }
  delay(10000); // wait 10 seconds before sending the next request
} 
```
## finally:
- The code checks the HTTP response code. If it's greater than 0, it means the request was successful.
- The code then checks if the response is equal to "forward". If it is, it turns the LED on by setting the voltage on the LED pin to HIGH. Otherwise, it turns the LED off by setting the voltage to LOW.

## an image of the project

<img width="959" alt="Screenshot 2024-07-27 221902" src="https://github.com/user-attachments/assets/5022244c-d4bb-4936-ab9e-e0060a2cc7c2">


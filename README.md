# If-this-then-that
Code for Physical Computing Project
/*
 * If This, Then That
 * Justice Stacey
 * Course code and title
 * OCAD University
 * March 13, 2017
 * Based on
 * https://learn.adafruit.com/flora-rgb-smart-pixels/run-pixel-test-code
 * Neopixal library example: Simple
 */
#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
  #include <avr/power.h>
#endif

#define PIN            6
#define NUMPIXELS      16
Adafruit_NeoPixel pixels = Adafruit_NeoPixel(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

const int sensorPin = A1; //A1 used to to fear of short circuiting due to small space. 
const int ledPin = 13; //used for calibration set up
int sensorValue = 0;         // the sensor value
int sensorMin = 1023;        // minimum sensor value
int sensorMax = 0;           // maximum sensor value

void setup() {
    // turn on LED to signal the start of the calibration period:
  pinMode(13, OUTPUT);
  digitalWrite(13, HIGH);

  // calibrate during the first five seconds
  while (millis() < 5000) {
    sensorValue = analogRead(sensorPin);

    // record the maximum sensor value
    if (sensorValue > sensorMax) {
      sensorMax = sensorValue;
    }

    // record the minimum sensor value
    if (sensorValue < sensorMin) {
      sensorMin = sensorValue;
    }
  }

  // signal the end of the calibration period
  digitalWrite(13, LOW);

  pinMode (sensorPin, INPUT); 
  Serial.begin (9600);
  pixels.begin(); // This initializes the NeoPixel library.
}

void loop() {
   // read the sensor:
  sensorValue = analogRead(sensorPin);

  // apply the calibration to the sensor reading
  sensorValue = map(sensorValue, sensorMin, sensorMax, 0, 255);

  // in case the sensor value is outside the range seen during calibration
  sensorValue = constrain(sensorValue, 0, 255);

  // fade the LED using the calibrated value:
  analogWrite(ledPin, sensorValue);
 Serial.println(analogRead(0));
 if (analogRead(A0) > 30 ) {
for(int i=0;i<NUMPIXELS;i++){

    // pixels.Color takes RGB values, from 0,0,0 up to 255,255,255
    pixels.setPixelColor(i, pixels.Color(0,150,150)); // Moderately bright green color.

    pixels.show(); // This sends the updated pixel color to the hardware.
} 
}

else {
  for(int i=0;i<NUMPIXELS;i++){
  pixels.setPixelColor(i, pixels.Color(150,150,150)); // Moderately bright green color.

    pixels.show(); // This sends the updated pixel color to the hardware.
}
}
}

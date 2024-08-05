#include <ESP8266WiFi.h>
#include "secrets.h"
#include "ThingSpeak.h"

WiFiClient client;

#define SOUND_SENSOR_PIN A0  // Analog pin for sound sensor

const int SAMPLES = 60;  // Number of samples for averaging (1 minute)
float dbReadings[SAMPLES];  // Array to store dB readings
int readIndex = 0;  // Index for the current reading
unsigned long lastUpdateTime = 0;  // Last update time for ThingSpeak
unsigned long lastMinuteUpdateTime = 0;  // Last update time for one-minute average

float analogToDecibel(int analogReading) {
  // Constants based on calibration
  const float DB_MIN = 40.0;
  const float ANALOG_MIN = 400.0;
  const float DB_PER_ANALOG = 0.1; // 10 dB per 100 analog units

  // Linear mapping
  if (analogReading < ANALOG_MIN) {
    return DB_MIN; // Minimum detectable level
  } else {
    return DB_MIN + (analogReading - ANALOG_MIN) * DB_PER_ANALOG;
  }
}

void setup() {
  Serial.begin(115200);
  while (!Serial) {
    ; // wait for serial port to connect. Needed for Leonardo native USB port only
  }
  
  WiFi.mode(WIFI_STA);
  WiFi.disconnect();
  delay(100);
  
  ThingSpeak.begin(client);  // Initialize ThingSpeak

  // Initialize dbReadings array
  for (int i = 0; i < SAMPLES; i++) {
    dbReadings[i] = 0;
  }
}

void loop() {
  // Connect or reconnect to WiFi
  if (WiFi.status() != WL_CONNECTED) {
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(ssid);
    while (WiFi.status() != WL_CONNECTED) {
      WiFi.begin(ssid, pass);
      Serial.print(".");
      delay(5000);     
    } 
    Serial.println("\nConnected.");
  }
  
  // Read sound level from sound sensor
  int analogReading = analogRead(SOUND_SENSOR_PIN);
  float dB = analogToDecibel(analogReading);
  
  // Store dB reading in array
  dbReadings[readIndex] = dB;
  readIndex = (readIndex + 1) % SAMPLES;

  // Calculate average dB over the last minute
  float avgDB = 0;
  for (int i = 0; i < SAMPLES; i++) {
    avgDB += dbReadings[i];
  }
  avgDB /= SAMPLES;

  Serial.print("Analog Reading: ");
  Serial.print(analogReading);
  Serial.print(", Current dB: ");
  Serial.print(dB, 1);
  Serial.print(", 1-min Avg dB: ");
  Serial.println(avgDB, 1);

  unsigned long currentTime = millis();

  // Update ThingSpeak every 15 seconds with the current reading
  if (currentTime - lastUpdateTime >= 15000) {
    lastUpdateTime = currentTime;

    // Set the fields with the sound values
    ThingSpeak.setField(1, analogReading);
    ThingSpeak.setField(2, dB);
    
    // Write to ThingSpeak
    int x = ThingSpeak.writeFields(myChannelNumber, myWriteAPIKey);
    
    if (x == 200) {
      Serial.println("Channel update successful.");
    } else {
      Serial.println("Problem updating channel. HTTP error code " + String(x));
    }
  }

  // Update ThingSpeak every minute with the 1-minute average
  if (currentTime - lastMinuteUpdateTime >= 15000) {
    lastMinuteUpdateTime = currentTime;

    // Set the field with the 1-minute average dB
    ThingSpeak.setField(3, avgDB);
    
    // Write the 1-minute average to ThingSpeak
    int y = ThingSpeak.writeFields(myChannelNumber, myWriteAPIKey);
    
    if (y == 200) {
      Serial.println("1-minute average update successful.");
    } else {
      Serial.println("Problem updating 1-minute average. HTTP error code " + String(y));
    }
  }

  delay(1000); // Wait 1 second before next reading
}

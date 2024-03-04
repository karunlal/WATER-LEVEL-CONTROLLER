#define BLYNK_TEMPLATE_ID "TMPL2D5X6Qbv2"
#define BLYNK_TEMPLATE_NAME "WATER LEVEL TEMPLATE"
#define BLYNK_AUTH_TOKEN "95cBx2nzZ56Cuk5FSe9U6_08S2HAQg5N"

#define waterLevelTrigPin D5  // Define the trigger pin
#define waterLevelEchoPin D6  // Define the echo pin
#define relayPin D7           // Define the relay pin
#define buttonPin D3          // Define the button pin
//  SCL  D1
//  SDL  D2
#define maxWaterLevel 85
#define tankHeight 105

#include <Ultrasonic.h>
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

char auth[] = BLYNK_AUTH_TOKEN;  // Blynk authentication token

char ssid[] = "Premlal";         // Wi-Fi SSID
char password[] = "62383349";    // Wi-Fi password

Ultrasonic ultrasonic(waterLevelTrigPin, waterLevelEchoPin);
int relayState = HIGH;           // Store the relay state (LOW = ON, HIGH = OFF)
int buttonState = LOW;           // Store the button state
int lastButtonState = LOW;       // Store the previous button state

LiquidCrystal_I2C lcd(0x27, 16, 2); // I2C address 0x27, 16-column and 2-row LCD

BLYNK_WRITE_DEFAULT() {          // Blynk function to handle virtual pin updates
  int pin = request.pin;
  int value = param.asInt();

  Serial.print("Blynk Write Default, pin: ");
  Serial.print(pin);
  Serial.print(", value: ");
  Serial.println(value);

  if (pin == V2) {
    relayState = value;
    int waterblynklevel = getWaterLevel();
    Serial.print("Water level for Blynk: ");
    Serial.println(waterblynklevel);
    
    if (waterblynklevel < maxWaterLevel) {
      digitalWrite(relayPin, relayState);
    } else if (waterblynklevel > maxWaterLevel) {
      relayState = HIGH;
      digitalWrite(relayPin, relayState);
    }
    lcd.setCursor(5, 1);
    if (relayState == LOW) {
      lcd.print("ON ");
      Blynk.virtualWrite(V3, "PUMP IS ON");
    } else if (relayState == HIGH) {
      Blynk.virtualWrite(V3, "PUMP IS OFF");
      lcd.print("OFF");
    }
  }
}

void setup() {
  pinMode(relayPin, OUTPUT);             // Set relay pin as output
  pinMode(buttonPin, INPUT_PULLUP);      // Set button pin as input with internal pull-up resistor

  Serial.begin(115200);                  // Initialize serial communication for debugging
  Serial.println("Starting...");

  WiFi.begin(ssid, password);            // Connect to Wi-Fi network
  Serial.print("Connecting to Wi-Fi");

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println();
  Serial.println("Wi-Fi connected");
  Serial.print("IP address: ");
  Serial.println(WiFi.localIP());

  Blynk.begin(auth, ssid, password);     // Initialize Blynk
  Serial.println("Blynk connected");

  Blynk.virtualWrite(V2, relayState);    // Sync the initial relay state with the Blynk app

  lcd.begin();                           // Initialize the LCD display
  lcd.backlight();                       // Turn on the backlight
  lcd.print("WATER LEVEL:");
  lcd.setCursor(0, 1);
  lcd.print("PUMP: ");
}

void loop() {
  int waterlevel = getWaterLevel();
  Serial.print("Current water level: ");
  Serial.println(waterlevel);

  lcd.setCursor(12, 0);
  lcd.print(waterlevel);
  lcd.print("cm");
  Blynk.virtualWrite(V1, waterlevel);

  buttonState = digitalRead(buttonPin); // Read the current button state
  Serial.print("Button state: ");
  Serial.println(buttonState);

  // Check if the button state has changed
  if (buttonState != lastButtonState) {
    Serial.println("Button state changed.");
    if (buttonState == LOW) {
      Serial.println("Button pressed.");
      if (waterlevel < maxWaterLevel) {
        relayState = !relayState;       // Toggle the relay state
        digitalWrite(relayPin, relayState);  // Update the relay
        Serial.println("Relay state toggled.");
      } else if (waterlevel > maxWaterLevel) {
        relayState = HIGH;
        digitalWrite(relayPin, relayState);  // Update the relay
        Serial.println("Relay turned off due to high water level.");
      }
      lcd.setCursor(5, 1);
      if (relayState == LOW) {
        lcd.print("ON ");
        Blynk.virtualWrite(V3, "PUMP IS ON");
      } else if (relayState == HIGH) {
        lcd.print("OFF");
        Blynk.virtualWrite(V3, "PUMP IS OFF");
      }
      Blynk.virtualWrite(V2, relayState);  // Sync the relay state with the Blynk app
    }
    lastButtonState = buttonState;        // Update the last button state
  }

  // Check if the water level exceeds the maximum threshold
  if (waterlevel > maxWaterLevel) {
    relayState = HIGH;
    digitalWrite(relayPin, relayState);  // Turn off the pump
    lcd.setCursor(5, 1);
    lcd.print("OFF");
    Blynk.virtualWrite(V3, "PUMP IS OFF");
    Serial.println("Pump turned off due to exceeding max water level.");
    Blynk.virtualWrite(V2, relayState);  // Sync the relay state with the Blynk app
  }

  Blynk.run();                           // Run the Blynk library
}


int getWaterLevel() {
  digitalWrite(waterLevelTrigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(waterLevelTrigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(waterLevelTrigPin, LOW);
  
  unsigned long duration = pulseIn(waterLevelEchoPin, HIGH);
  //unsigned int distance = ultrasonic.convert(duration, Ultrasonic::CM);
  int distance = ((duration / 2) * 0.034);
  int wl = tankHeight - distance;
delay (1000); // Delay between readings to stabilize measurements
  //serial.print("distance is " distance)
  return wl;
}








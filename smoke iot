#define BLYNK_TEMPLATE_ ID "MPL60PgAntSk"
#define BLYNK_TEMPLATE_NAME "Smoke Alarm"
#define BLYNK_AUTH_TOKEN "cx3N5nFRfn22b69GqqQX3EjZ7]XbYxF"

#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <LiquidCrystal_I2C.h>
// #include <U8g2lib.h>
LiquidCrystal_I2C lcd(0x27, 16, 2);

char auth[] = “cx3N5nFRfn22b69GqqQX3EjZ7]XbYxF”;
char ssid[] = “Văn Hà”;  
char pass[] = “27052003Aa”;  

BlynkTimer timer;


int SmokeSensor = A0;
float sinVal;
int toneVal;
#define BlueLED D4
#define RedLED D5
#define Buzzer D6

int sensorThreshold = 250;

void sendSensor()
{
    int smokeValue = analogRead(SmokeSensor);
    Blynk.virtualWrite(V0, smokeValue);
    Serial.print("Smoke Value: ");
    Serial.println(smokeValue);

    if (smokeValue < sensorThreshold)
    {
        digitalWrite(BlueLED, HIGH);
        digitalWrite(RedLED, LOW);
        digitalWrite(Buzzer, LOW);
        lcd.setCursor(0, 0);
        lcd.print("Smoke Value: ");
        lcd.print(smokeValue);
        lcd.setCursor(0, 1);
        lcd.print("normal smoke");
        Serial.println("normal smoke");
        delay(1000);
        lcd.clear();
    }
    else
    {
        digitalWrite(BlueLED, LOW);
        digitalWrite(RedLED, HIGH);
         for(int x=0; x<180; x++){
            sinVal = (sin(x*(3.1412/180)));
            toneVal = 2000+(int(sinVal*1000));
            tone(Buzzer, toneVal);
            delay(2); 
     }  
        Blynk.logEvent("smoke_alert", "Smoke is High");
        lcd.setCursor(0, 0);
        lcd.print(smokeValue);
        lcd.setCursor(0, 1);
        lcd.print("high smoke");
        Serial.println("high smoke");
        delay(1000);
        lcd.clear();
    }
}

void setup()
{
    Serial.begin(9600);
    pinMode(SmokeSensor, INPUT);
    pinMode(BlueLED, OUTPUT);
    pinMode(RedLED, OUTPUT);
    pinMode(Buzzer, OUTPUT);
    Blynk.begin(auth, ssid, pass);

    timer.setInterval(1000L, sendSensor);

    lcd.init();
    lcd.backlight(); 
    lcd.setCursor(3, 0);
    lcd.print("Smoke Level");
    lcd.setCursor(3, 1);
    lcd.print("Monitoring");
    delay(2000);
    lcd.clear();
}

void loop()
{
    Blynk.run();
    timer.run();
}

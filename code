#include <Adafruit_Fingerprint.h>
#include <SoftwareSerial.h>
#include <LiquidCrystal.h>
#include <Keypad.h>

// Fingerprint setup
SoftwareSerial mySerial(2, 3);
Adafruit_Fingerprint finger = Adafruit_Fingerprint(&mySerial);

// LCD setup (RS, E, D4, D5, D6, D7)
LiquidCrystal lcd(7, 6, 5, 4, 3, 2);

// Keypad setup
const byte ROWS = 4;
const byte COLS = 4;

char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

byte rowPins[ROWS] = {A0, A1, A2, A3};
byte colPins[COLS] = {A4, A5, A6, A7};

Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

// Vote storage
int candidate1 = 0;
int candidate2 = 0;
int candidate3 = 0;

bool voted[50]; // store voted users (ID based)

void setup() {
  Serial.begin(9600);
  finger.begin(57600);

  lcd.begin(16, 2);
  lcd.print("Voting System");
  delay(2000);
  lcd.clear();

  if (finger.verifyPassword()) {
    lcd.print("Sensor Ready");
  } else {
    lcd.print("Sensor Error");
    while (1);
  }
  delay(2000);
  lcd.clear();
}

void loop() {
  lcd.setCursor(0, 0);
  lcd.print("Place Finger");

  int id = getFingerprintID();

  if (id != -1) {
    if (voted[id] == true) {
      lcd.clear();
      lcd.print("Already Voted");
      delay(2000);
      lcd.clear();
      return;
    }

    voted[id] = true;

    lcd.clear();
    lcd.print("1:A 2:B 3:C");

    while (true) {
      char key = keypad.getKey();

      if (key) {
        if (key == '1') {
          candidate1++;
          break;
        } else if (key == '2') {
          candidate2++;
          break;
        } else if (key == '3') {
          candidate3++;
          break;
        }
      }
    }

    lcd.clear();
    lcd.print("Vote Recorded");
    delay(2000);
    lcd.clear();
  }
}

// Fingerprint function
int getFingerprintID() {
  uint8_t p = finger.getImage();
  if (p != FINGERPRINT_OK) return -1;

  p = finger.image2Tz();
  if (p != FINGERPRINT_OK) return -1;

  p = finger.fingerSearch();
  if (p != FINGERPRINT_OK) return -1;

  return finger.fingerID;
}

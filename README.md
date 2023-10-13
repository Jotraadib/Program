#include <Keypad.h>
#include <LiquidCrystal.h>
#include <Servo.h>
#define LED_RED 2
#define LED_YELLOW 3
#define LED_GREEN 4
#define trigPin 38
#define echoPin 40
Servo motorServo;
#include <DHT.h>
#define DHTPIN 10
#define DHTTYPE DHT22

DHT dht(DHTPIN, DHTTYPE);


const byte ROWS = 4;  //four rows
const byte COLS = 4;  //four columns
//define the cymbols on the buttons of the keypads
char hexaKeys[ROWS][COLS] = {
  { '1', '2', '3', 'A' },
  { '4', '5', '6', 'B' },
  { '7', '8', '9', 'C' },
  { '*', '0', '#', 'D' }
};
byte rowPins[ROWS] = { 47, 49, 51, 53 };  //connect to the row pinouts of the keypad
byte colPins[COLS] = { 46, 48, 50, 52 };  //connect to the column pinouts of the keypad

//initialize an instance of class NewKeypad
Keypad customKeypad = Keypad(makeKeymap(hexaKeys), rowPins, colPins, ROWS, COLS);
int rs = 37, en = 36, d4 = 33, d5 = 32, d6 = 35, d7 = 34;
LiquidCrystal lcd(rs, en, d4, d5, d6, d7);
char correctPasscode[] = "1234";
char enteredPasscode[5];
int passcodeIndex = 0;

int kondisi = 0;
int ulang;
int servoPos = 0;

const int a1 = 45;
const int a2 = 31;
const int b1 = 30;
const int b2 = 44;

byte heart[8] = {
  0b00000,
  0b01010,
  0b11111,
  0b11111,
  0b11111,
  0b01110,
  0b00100,
  0b00000
};

byte smiley[8] = {
  0b00000,
  0b00000,
  0b01010,
  0b00000,
  0b00000,
  0b10001,
  0b01110,
  0b00000
};

void setup() {
  pinMode(2, OUTPUT);
  pinMode(3, OUTPUT);
  pinMode(4, OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  lcd.begin(16, 2);

  lcd.clear();
  delay(2000);
  lcd.setCursor(1, 0);

  lcd.print("WELCOME BACK !");
  delay(2000);
  lcd.setCursor(4, 1);
  lcd.print("AZAZEUS");
  delay(2000);
  lcd.clear();
  step3();
  lcd.setCursor(3, 0);
  lcd.print("PLEASE TRY");
  delay(2000);
  lcd.setCursor(1, 1);
  lcd.print("PUSH SOMETHING");



  Serial.begin(9600);
  delay(500);
  motorServo.attach(9);
  motorServo.write(servoPos);

  pinMode(a1, OUTPUT);
  pinMode(a2, OUTPUT);
  pinMode(b1, OUTPUT);
  pinMode(b2, OUTPUT);

  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  dht.begin();
}

void step1() {

  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(25);
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(25);
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, HIGH);
  delay(25);
  digitalWrite(a1, LOW);
  digitalWrite(a2, HIGH);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(25);
}

void step2() {
  digitalWrite(a1, HIGH);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(25);
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, HIGH);
  digitalWrite(b2, LOW);
  delay(25);
  digitalWrite(a1, LOW);
  digitalWrite(a2, HIGH);
  digitalWrite(b1, LOW);
  digitalWrite(b2, LOW);
  delay(25);
  digitalWrite(a1, LOW);
  digitalWrite(a2, LOW);
  digitalWrite(b1, LOW);
  digitalWrite(b2, HIGH);
  delay(25);
}





//   digitalWrite(a1, LOW);
//   digitalWrite(a2, LOW);
//   digitalWrite(b1, LOW);
//   digitalWrite(b2, HIGH);
// }

// void step4() {

//   digitalWrite(a1, LOW);
//   digitalWrite(a2, HIGH);
//   digitalWrite(b1, LOW);
//   digitalWrite(b2, LOW);
// }

void loop() {
  while (kondisi == 0) {
    char key = customKeypad.getKey();

    switch (key) {
      case '1':
        //lcd.createChar(0, heart);
        lcd.clear();
        lcd.setCursor(3, 0);
        digitalWrite(2, HIGH);
        digitalWrite(3, LOW);
        digitalWrite(4, LOW);
        Serial.println(key);

        lcd.print("LED RED ON");
        //lcd.setCursor(8, 0);
        //lcd.write(byte(0));
        break;


      case '2':
        lcd.clear();
        lcd.setCursor(1, 0);
        digitalWrite(2, LOW);
        digitalWrite(3, HIGH);
        digitalWrite(4, LOW);
        // Serial.println(key);
        lcd.print("LED YELLOW ON");
        break;

      case '3':
        lcd.clear();
        lcd.setCursor(2, 0);
        digitalWrite(2, LOW);
        digitalWrite(3, LOW);
        digitalWrite(4, HIGH);
        // Serial.println(key);
        lcd.print("LED GREEN ON");
        break;

      case 'A':

        kondisi = 1;

        break;

      case 'B':

        kondisi = 2;

        break;

      case 'C':

        kondisi = 3;

        break;

      case '4':
        lcd.clear();
        lcd.setCursor(1, 0);
        lcd.print("MOTOR SERVO ON");

        servoPos += 180;
        servoPos = constrain(servoPos, 0, 180);
        motorServo.write(servoPos);
        for (ulang = 0; ulang < 1000; ulang++) {
          char key = customKeypad.getKey();
          if (key) {
            lcd.clear();
            Serial.println(key);
            lcd.print(key);
            switch (key) {
              case '0':
                lcd.clear();
                lcd.setCursor(0, 0);
                lcd.print("MODE : RESET");
                servoPos = 0;
                motorServo.write(servoPos);
                kondisi = 0;
                break;
            }
          }
          delay(1);
        }


        break;

      case '5':
        lcd.clear();
        lcd.setCursor(0, 0);
        lcd.print("MOTOR CONDITION:");
        lcd.setCursor(0, 1);
        lcd.print("CCW");
        kondisi = 4;
        break;

      case '6':
        lcd.clear();
        lcd.setCursor(0, 0);
        lcd.print("MOTOR CONDITION:");
        lcd.setCursor(0, 1);
        lcd.print("CW");
        kondisi = 5;
        break;
      case '7':
        kondisi = 6;
        break;
    }
    while (kondisi == 1) {
      lcd.clear();
      lcd.setCursor(3, 0);
      lcd.print("MODE   ON");
      lcd.createChar(0, heart);
      lcd.setCursor(8, 0);
      lcd.write(byte(0));

      lcd.createChar(1, smiley);
      lcd.setCursor(7, 1);
      lcd.write(byte(1));

      digitalWrite(2, HIGH);
      digitalWrite(3, LOW);
      digitalWrite(4, LOW);
      for (ulang = 0; ulang < 1000; ulang++) {
        char key = customKeypad.getKey();
        if (key) {
          lcd.clear();
          Serial.println(key);
          lcd.print(key);
          switch (key) {
            case '0':
              lcd.clear();
              lcd.setCursor(0, 0);
              lcd.print("MODE : RESET");
              kondisi = 0;
              break;
          }
        }
        delay(1);
      }

      digitalWrite(2, LOW);
      digitalWrite(3, HIGH);
      digitalWrite(4, LOW);
      for (ulang = 0; ulang < 1000; ulang++) {
        char key = customKeypad.getKey();
        if (key) {
          lcd.clear();
          Serial.println(key);
          lcd.print(key);
          switch (key) {
            case '0':
              lcd.clear();
              lcd.setCursor(0, 0);
              lcd.print("MODE : RESET");
              kondisi = 0;
              break;
          }
        }
        delay(1);
      }

      digitalWrite(2, LOW);
      digitalWrite(3, LOW);
      digitalWrite(4, HIGH);
      for (ulang = 0; ulang < 1000; ulang++) {
        char key = customKeypad.getKey();
        if (key) {
          lcd.clear();
          Serial.println(key);
          lcd.print(key);
          switch (key) {
            case '0':
              lcd.clear();
              lcd.setCursor(0, 0);
              lcd.print("MODE : RESET");
              kondisi = 0;
              break;
          }
        }
        delay(1);
      }
    }
    while (kondisi == 2) {
      digitalWrite(trigPin, LOW);
      delayMicroseconds(38);
      digitalWrite(trigPin, HIGH);
      delayMicroseconds(40);
      digitalWrite(trigPin, LOW);

      long duration = pulseIn(echoPin, HIGH);
      int distance_cm = (duration / 2) / 29.1;
      lcd.clear();
      lcd.setCursor(0, 0);
      lcd.print("Distance:");
      lcd.setCursor(0, 1);
      lcd.print(distance_cm);
      lcd.print(" cm");

      for (ulang = 0; ulang < 1000; ulang++) {
        char key = customKeypad.getKey();
        if (key) {
          lcd.clear();
          Serial.println(key);
          lcd.print(key);
          switch (key) {
            case '0':
              lcd.clear();
              lcd.setCursor(0, 0);
              lcd.print("MODE : RESET");
              kondisi = 0;
              break;
          }
        }
        delay(1);
      }
    }
    while (kondisi == 4) {
      step1();
      char key = customKeypad.getKey();
      if (key) {
        lcd.clear();
        Serial.println(key);
        lcd.print(key);
        switch (key) {
          case '0':
            lcd.clear();
            lcd.setCursor(0, 0);
            lcd.print("MODE : RESET");
            kondisi = 0;
            break;
        }
      }
      // delay(20);
      // step2();
      // delay(20);
      // step3();
      // delay(20);
      // step4();
      // delay(20);
    }
    while (kondisi == 5) {
      step2();
      char key = customKeypad.getKey();
      if (key) {
        lcd.clear();
        Serial.println(key);
        lcd.print(key);
        switch (key) {
          case '0':
            lcd.clear();
            lcd.setCursor(0, 0);
            lcd.print("MODE : RESET");
            kondisi = 0;
            break;
        }
      }
    }
    while (kondisi == 6) {
      lcd.clear();
      int humidity = dht.readHumidity();
      int temperature = dht.readTemperature();

      if (isnan(humidity) || isnan(temperature)) {
        Serial.println("Gagal membaca dari sensor DHT");
      } else {
        lcd.setCursor(0, 0);
        lcd.print("Kelembaban: ");
        lcd.setCursor(0, 1);
        lcd.print(humidity);
        lcd.setCursor(2, 1);
        lcd.print(" %");
        delay(4000);
        lcd.clear();
        lcd.setCursor(0, 0);
        lcd.print("Suhu : ");
        lcd.setCursor(0, 1);
        lcd.print(temperature);
        lcd.setCursor(2, 1);
        lcd.print(" C");
      }
    }
    void step3() {
      char key = customKeypad.getKey();

      if (key) {
        if (key == '#') {
          enteredPasscode[passcodeIndex] = '\0';  // Menambahkan null-terminator
          if (strcmp(enteredPasscode, correctPasscode) == 0) {
            Serial.println("Sandi benar. Akses diberikan.");
            // Lakukan tindakan yang sesuai ketika sandi benar
            clearEnteredPasscode();  // Bersihkan kata sandi yang dimasukkan
          } else {
            Serial.println("Sandi salah. Coba lagi.");
            clearEnteredPasscode();  // Bersihkan kata sandi yang dimasukkan
          }
          passcodeIndex = 0;
        } else if (passcodeIndex < 4) {
          enteredPasscode[passcodeIndex++] = key;
        }
      }
    }
    void clearEnteredPasscode() {
  for (int i = 0; i < 5; i++) {
    enteredPasscode[i] = '\0';  // Reset array kata sandi
  }
}
  }
}

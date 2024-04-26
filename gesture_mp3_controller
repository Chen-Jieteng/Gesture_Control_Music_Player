#include <Wire.h>
#include "paj7620.h"
#define GES_REACTION_TIME		500				
#define GES_ENTRY_TIME			800			
#define GES_QUIT_TIME			1000

int pins[5] = {2, 3, 4, 5, 6}; 
int nextPin = 10;  
int prevPin = 11;  
int playPausePin = 12;  

int currentSong = 1;  

void setup() {
  Serial.begin(9600);
  Wire.begin();
  Wire.setClock(100000);  

  uint8_t error = paj7620Init();
  if (error) {
    Serial.print("INIT ERROR,CODE:");
    Serial.println(error);
  } else {
    Serial.println("PAJ7620 Successfully Instantiated！");
  }
  Serial.println("Please do the gesture:");

  pinMode(nextPin, OUTPUT);
  pinMode(prevPin, OUTPUT);
  pinMode(playPausePin, OUTPUT);
  selectSong(currentSong);
  
  for (int i = 0; i < 5; i++) {
    pinMode(pins[i], OUTPUT);
  }
}

void selectSong(int songNumber) {
    int mask = 1;  
    for (int i = 0; i < 5; i++) {
        digitalWrite(pins[i], ((songNumber & mask) ? LOW : HIGH));
        mask <<= 1;
    }
}

void loop() {
  uint8_t data = 0, error;
  error = paj7620ReadReg(0x43, 1, &data); 

  if (!error) {
    switch (data) {
      case GES_RIGHT_FLAG:
        if (currentSong < 30) currentSong++; 
        else currentSong = 1; 
        selectSong(currentSong);
        Serial.println("Next song");
        break;

      case GES_LEFT_FLAG:
        if (currentSong > 1) currentSong--; 
      else currentSong = 30; 
        selectSong(currentSong);
        Serial.println("Previous song");
        break;

      case GES_UP_FLAG:
        Serial.println("Pause");
        digitalWrite(playPausePin, HIGH);
        delay(100);
        digitalWrite(playPausePin, LOW);
      break;

      case GES_DOWN_FLAG:
        Serial.println("Start");
        digitalWrite(playPausePin, HIGH);
        delay(100);
        digitalWrite(playPausePin, LOW);
        break;
      
      default: 
        break;
    }
  }
  delay(500);
}

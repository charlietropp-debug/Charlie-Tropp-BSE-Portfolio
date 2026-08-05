Self Driving Car
My project is an Arduino-based self-driving car that uses ultrasonic and infrared sensors to detect obstacles and navigate autonomously. In addition to self-driving capabilities, the car will include a remote-control mode, allowing it to switch between manual and autonomous operation. This project combines programming, electronics, and robotics to demonstrate the fundamentals of autonomous vehicle technology.

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| Engineer|School Name| Interests | Grade|
|:--:|:--:|:--:|:--:|
| Charles T| Paul D. Schreiber High School |AI and Software Engineering| Incoming Juior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/shorts/imD-li7K2ZM" title="Charles T Milestone 3" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone


<iframe width="560" height="315" src="https://www.youtube.com/shorts/LZQw0s5oOHk" title="Charles T Milestone 2" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


For My second Milestone I have upgraded the Line Following mode from my first milestone by adding a second line following module. I also added an I2C lCD screen to show what mode the car is in ex Manual/Self-Driving. Finally I improved Remote Control mode by increases reach of the remote. Some previous challanges that I over cam were issues with the line-following mode and manual mode which I fixed with better code and more attention to the coding. For my third milestone I will Improve the Self-Driving Mode and add a bluetooth module so I can controll the Robot from my phone.

# First Milestone


<iframe width="560" height="315" src="https://www.youtube.com/watch?v=zcms080g7NM" title="Charles T Milestone 1" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

* **Project Overview:** I am building an Arduino-powered self-drivingcar that can detect obstacles, navigate around them, and switch between autonomous and remote-controlled driving modes.

* **Components & Integration:** The project uses an Arduino R3, ultrasonic sensor, infrared obstacle sensors, L9110 motor driver, TT motors, IR Transmitter, and a rechargeable battery. The Arduino processes sensor data and controls the motors to safely navigate the environment.

* **Progress So Far:** I have assembled the car, connected the motors and sensors, tested the hardware, and begun programming the obstacle detection and motor control systems as well as the manual mode.

* **Challenges:** My biggest challenge is creating smoother and smarter navigation instead of simply backing up and turning.

* **Next Steps:** I will improve the obstacle avoidance algorithm,test the car in different obstacle courses, and refine the software until it can reliably drive both autonomously and under manual control.

# Schematics 
![HeadstoneImage]([FritzingSChematic.png]
![Headstone Image](logo.svg)

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
<div style="
  height: 350px;
  overflow-y: auto;
  overflow-x: hidden;
  background-color: #1e1e1e;
  color: white;
  padding: 15px;
  border-radius: 8px;
">
  <pre style="
    margin: 0;
    white-space: pre-wrap;
    overflow-wrap: anywhere;
    word-break: break-word;
    font-family: Consolas, monospace;
    font-size: 14px;
    line-height: 1.5;
  "><code>




#include <IRremote.h>
#include <LiquidCrystal_I2C.h>

const int A_1B = 5;
const int A_1A = 6;
const int B_1B = 9;
const int B_1A = 10;

const int IR_RECEIVE_PIN = 12;

const int echoPin  = 4;
const int trigPin  = 3;
const int rightIR  = 7;
const int leftIR   = 8;


const int leftLineTrackPin  = 2;  
const int rightLineTrackPin = 11; 

enum RemoteKey {
  KEY_NONE, KEY_ERROR,
  KEY_0, KEY_1, KEY_2, KEY_3, KEY_4, KEY_5, KEY_6, KEY_7, KEY_8, KEY_9,
  KEY_PLUS, KEY_MINUS, KEY_EQ, KEY_USD, KEY_CYCLE,
  KEY_PLAY_PAUSE, KEY_FORWARD, KEY_BACKWARD, KEY_POWER, KEY_MUTE, KEY_MODE
};


enum Mode { MODE_MANUAL, MODE_SELF_DRIVE, MODE_LINE_TRACK, MODE_HAND_FOLLOW };
Mode currentMode = MODE_MANUAL;


const bool LINE_SENSOR_BLACK_IS_HIGH = true;


float LEFT_TRIM  = 1.15;
float RIGHT_TRIM = 1.00;


const int JOY_CENTER = 512;   
const int JOY_DEADZONE = 40; 
const int SLIDER_MAX = 100;   


bool phoneActive = false;

unsigned long lcdShownAt = 0;  
bool lcdShowingTuning = false;  
const unsigned long LCD_TUNING_SHOW_MS = 2000;


char mbId[6];
char mbVal[14];
byte mbIdLen = 0;
byte mbValLen = 0;
byte mbState = 0;   


LiquidCrystal_I2C lcd(0x27, 16, 2);  


RemoteKey activeMoveKey = KEY_NONE;
unsigned long lastMoveSignalTime = 0;
const unsigned long RELEASE_TIMEOUT_MS = 200;

int driveSpeed = 150;
const int TURN_SPEED    = 150;  
const int REVERSE_SPEED = 130;  

const int RAMP_STEP = 35;                  
const unsigned long RAMP_INTERVAL_MS = 10; 
const int MIN_START_PWM = 70;              

int targetLeftSpeed = 0;
int targetRightSpeed = 0;
int currentLeftSpeed = 0;
int currentRightSpeed = 0;
unsigned long lastRampUpdate = 0;

void drive(int leftSpeed, int rightSpeed) {
  targetLeftSpeed = constrain(leftSpeed, -255, 255);
  targetRightSpeed = constrain(rightSpeed, -255, 255);
}

int rampToward(int current, int target, int step) {
  if (current == 0 && target != 0) {

    if (target > 0) return min(MIN_START_PWM, target);
    else             return max(-MIN_START_PWM, target);
  }
  if (current < target) return min(current + step, target);
  if (current > target) return max(current - step, target);
  return current;
}

int MOTOR_DEADBAND = 45;

int applyDeadband(int pwm) {
  return (pwm > 0 && pwm < MOTOR_DEADBAND) ? 0 : pwm;
}


void writeMotors(int left, int right) {
  float leftMag  = fabs((float)left)  * LEFT_TRIM;
  float rightMag = fabs((float)right) * RIGHT_TRIM;
  float peak = max(leftMag, rightMag);
  if (peak > 255.0f) {
    float scale = 255.0f / peak;
    leftMag  *= scale;
    rightMag *= scale;
  }

  int leftPWM  = applyDeadband(constrain((int)round(leftMag), 0, 255));
  int rightPWM = applyDeadband(constrain((int)round(rightMag), 0, 255));

  if (left >= 0) { analogWrite(A_1B, 0); analogWrite(A_1A, leftPWM); }
  else           { analogWrite(A_1A, 0); analogWrite(A_1B, leftPWM); }

  if (right >= 0) { analogWrite(B_1A, 0); analogWrite(B_1B, rightPWM); }
  else            { analogWrite(B_1B, 0); analogWrite(B_1A, rightPWM); }
}

void updateMotorRamp() {
  unsigned long now = millis();
  if (now - lastRampUpdate < RAMP_INTERVAL_MS) return;
  lastRampUpdate = now;

  currentLeftSpeed = rampToward(currentLeftSpeed, targetLeftSpeed, RAMP_STEP);
  currentRightSpeed = rampToward(currentRightSpeed, targetRightSpeed, RAMP_STEP);
  writeMotors(currentLeftSpeed, currentRightSpeed);
}

void driveNow(int leftSpeed, int rightSpeed) {
  drive(leftSpeed, rightSpeed);
  currentLeftSpeed = targetLeftSpeed;
  currentRightSpeed = targetRightSpeed;
  writeMotors(currentLeftSpeed, currentRightSpeed);
}


void hardStop() {
  targetLeftSpeed = 0;
  targetRightSpeed = 0;
  currentLeftSpeed = 0;
  currentRightSpeed = 0;
  writeMotors(0, 0);
}


void moveForward(int speed)  { drive(speed, speed); }
void moveBackward(int speed) { drive(-speed, -speed); }
void pivotRight(int speed)   { drive(-speed, speed); } 
void pivotLeft(int speed)    { drive(speed, -speed); }  
void singleLeft(int speed)      { drive(speed, 0); }    
void singleRight(int speed)     { drive(0, speed); }   
void singleBackLeft(int speed)  { drive(-speed, 0); }   
void singleBackRight(int speed) { drive(0, -speed); }  
void stopMove() { drive(0, 0); }

void steerToward(int dir, int outerSpeed, int innerSpeed) {

  if (dir > 0) drive(innerSpeed, outerSpeed);  
  else         drive(outerSpeed, innerSpeed);   
}


const unsigned long ULTRASONIC_TIMEOUT_US = 20000; 
const int SAFE_DISTANCE_CM = 30;          
int sdSpeed = 120;                      
const unsigned long BACKUP_MS = 400;     
unsigned long TURN_MS = 450;              
unsigned long ESCAPE_TURN_MS = 900;       
const int SD_MAX_CONSECUTIVE = 3;         
const unsigned long SD_PROGRESS_MS = 1500;

enum SelfDriveState { SD_FORWARD, SD_BACKUP, SD_TURN };
SelfDriveState sdState = SD_FORWARD;
unsigned long sdStateStart = 0;
int sdTurnDir = 1;
int sdConsecutiveTurns = 0; 
unsigned long sdTurnLen = TURN_MS; 

const int LINE_FILTER_SAMPLES = 3;
int leftLineHistory[LINE_FILTER_SAMPLES] = { 1, 1, 1 };
int leftLineHistoryIndex = 0;
int rightLineHistory[LINE_FILTER_SAMPLES] = { 1, 1, 1 };
int rightLineHistoryIndex = 0;


int lineSpeed   = 120;  
int turnSpeed   = 150;  
int searchSpeed = 150;  


const bool INVERT_LINE_STEERING = false;


int cornerTurnDir = -1;   
const unsigned long SWEEP_FIRST_MS = 800;
const unsigned long SWEEP_GROW_MS  = 500;

const unsigned long RECOVER_MAX_MS = 3000;

const unsigned long RECOVER_NUDGE_MS = 2500;

const unsigned long ONE_SENSOR_MIN_MS = 80;

enum LineState { LINE_CENTERED, LINE_LEFT, LINE_RIGHT, LINE_LOST };
LineState lineState = LINE_CENTERED;
int lastSeenDir = -1;          
bool recovering = false;      
int sweepDir = 1;             
unsigned long sweepStart = 0;
unsigned long sweepLen = SWEEP_FIRST_MS;
unsigned long recoverStart = 0;
unsigned long oneSensorSince = 0; 


float handDistanceFiltered = -1;         
unsigned long lastValidHandReading = 0; 

void setup() {
  Serial.begin(9600);

  pinMode(A_1B, OUTPUT);
  pinMode(A_1A, OUTPUT);
  pinMode(B_1B, OUTPUT);
  pinMode(B_1A, OUTPUT);

  pinMode(echoPin, INPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(leftIR, INPUT);
  pinMode(rightIR, INPUT);

  pinMode(leftLineTrackPin, INPUT);
  pinMode(rightLineTrackPin, INPUT);

  IrReceiver.begin(IR_RECEIVE_PIN, ENABLE_LED_FEEDBACK);
  Serial.println(F("READY - starting in MANUAL mode"));

  lcd.init();
  lcd.backlight();
  updateLCD();  
}

void loop() {
  handleIRRemote();  
  handlePhone();    
  checkLcdRestore(); 

  switch (currentMode) {
    case MODE_MANUAL:
      checkManualRelease();
      break;
    case MODE_SELF_DRIVE:
      runSelfDrive();
      break;
    case MODE_LINE_TRACK:
      runLineTrack();
      break;
    case MODE_HAND_FOLLOW:
      runHandFollow();
      break;
  }


  updateMotorRamp();
}


void updateLCD() {
  lcd.clear();
  lcd.setCursor(0, 0);

  lcd.print(F("Mode:"));
  lcd.setCursor(0, 1);
  if (currentMode == MODE_MANUAL) {
    if (phoneActive) lcd.print(F("Bluetooth Mode"));
    else             lcd.print(F("Remote Control"));
  }
  else if (currentMode == MODE_SELF_DRIVE) lcd.print(F("Self-Driving"));
  else if (currentMode == MODE_LINE_TRACK) lcd.print(F("Line Following"));
  else lcd.print(F("Hand Following"));
}


void handleIRRemote() {
  if (!IrReceiver.decode()) return;

  bool isRepeat = (IrReceiver.decodedIRData.flags & IRDATA_FLAGS_IS_REPEAT) != 0;

  RemoteKey key;
  if (isRepeat) {
    key = activeMoveKey;
  } else {
    key = decodeKeyValue(IrReceiver.decodedIRData.command);
    if (key == KEY_ERROR) {
      Serial.print(F("Unrecognized code: 0x"));
      Serial.println(IrReceiver.decodedIRData.command, HEX);
    }
  }


  if (!isRepeat && key == KEY_CYCLE) {
    setMode(currentMode == MODE_LINE_TRACK ? MODE_MANUAL : MODE_LINE_TRACK);
    IrReceiver.resume();
    return;
  }
  if (!isRepeat && key == KEY_USD) {
    setMode(currentMode == MODE_SELF_DRIVE ? MODE_MANUAL : MODE_SELF_DRIVE);
    IrReceiver.resume();
    return;
  }
  if (!isRepeat && key == KEY_EQ) {
    setMode(currentMode == MODE_HAND_FOLLOW ? MODE_MANUAL : MODE_HAND_FOLLOW);
    IrReceiver.resume();
    return;
  }

  if (!isRepeat && key == KEY_0) {
    setMode(MODE_MANUAL);
    IrReceiver.resume();
    return;
  }


  if (!isRepeat && (key == KEY_PLUS || key == KEY_MINUS)) {
    int step = (key == KEY_PLUS) ? +1 : -1;
    if (currentMode == MODE_LINE_TRACK) {

      lineSpeed = constrain(lineSpeed + step * 10, MOTOR_DEADBAND + 40, 255);
      Serial.print(F("lineSpeed = ")); Serial.println(lineSpeed);
    } else {
      driveSpeed = constrain(driveSpeed + step * 50, 0, 255);
      Serial.print(F("driveSpeed = ")); Serial.println(driveSpeed);
    }
  }

  if (currentMode == MODE_MANUAL && key != KEY_ERROR && key != KEY_NONE) {
    if (!isRepeat) { printKeyName(key); Serial.println(); }

    if (key == KEY_2) {
      moveForward(driveSpeed); activeMoveKey = KEY_2; lastMoveSignalTime = millis();
    } else if (key == KEY_1) {
      singleLeft(driveSpeed); activeMoveKey = KEY_1; lastMoveSignalTime = millis();
    } else if (key == KEY_3) {
      singleRight(driveSpeed); activeMoveKey = KEY_3; lastMoveSignalTime = millis();
    } else if (key == KEY_4) {
      pivotLeft(driveSpeed); activeMoveKey = KEY_4; lastMoveSignalTime = millis();
    } else if (key == KEY_6) {
      pivotRight(driveSpeed); activeMoveKey = KEY_6; lastMoveSignalTime = millis();
    } else if (key == KEY_7) {
      singleBackLeft(driveSpeed); activeMoveKey = KEY_7; lastMoveSignalTime = millis();
    } else if (key == KEY_9) {
      singleBackRight(driveSpeed); activeMoveKey = KEY_9; lastMoveSignalTime = millis();
    } else if (key == KEY_8) {
      moveBackward(driveSpeed); activeMoveKey = KEY_8; lastMoveSignalTime = millis();
    }
  }

  IrReceiver.resume();
}

void checkManualRelease() {
  if (activeMoveKey != KEY_NONE && millis() - lastMoveSignalTime > RELEASE_TIMEOUT_MS) {
    stopMove();
    activeMoveKey = KEY_NONE;
  }
}

void setMode(Mode newMode) {
  hardStop();
  activeMoveKey = KEY_NONE;
  phoneActive = false; 
  lcdShowingTuning = false;

  lineState = LINE_CENTERED;
  recovering = false;
  sweepDir = cornerTurnDir;
  sweepStart = 0;
  sweepLen = SWEEP_FIRST_MS;
  recoverStart = 0;
  oneSensorSince = 0;

  sdState = SD_FORWARD;
  sdConsecutiveTurns = 0;
  sdStateStart = millis();

  handDistanceFiltered = -1;
  lastValidHandReading = 0;



  currentMode = newMode;

  Serial.print(F("Switched to mode: "));
  if (currentMode == MODE_MANUAL) Serial.println(F("MANUAL"));
  else if (currentMode == MODE_SELF_DRIVE) Serial.println(F("SELF_DRIVE"));
  else if (currentMode == MODE_LINE_TRACK) Serial.println(F("LINE_TRACK"));
  else Serial.println(F("HAND_FOLLOW"));

  updateLCD();
}


float readUltrasonicCM() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  long duration = pulseIn(echoPin, HIGH, ULTRASONIC_TIMEOUT_US);
  if (duration == 0) return -1; 
  return duration / 58.00;
}

int decideTurnDirection(bool leftBlocked, bool rightBlocked) {
  if (leftBlocked && !rightBlocked) return 1;  
  if (rightBlocked && !leftBlocked) return -1; 
  return sdTurnDir;
}

void pivotTurn(int dir) {
  if (dir > 0) pivotRight(TURN_SPEED);
  else pivotLeft(TURN_SPEED);
}


void runSelfDrive() {
  unsigned long now = millis();
  bool leftBlocked  = !digitalRead(leftIR);   
  bool rightBlocked = !digitalRead(rightIR);
  float distance = readUltrasonicCM();
  bool frontBlocked = (distance >= 0 && distance < SAFE_DISTANCE_CM);
  bool blocked = frontBlocked || leftBlocked || rightBlocked;

  switch (sdState) {

    case SD_FORWARD:
      if (!blocked) {
        moveForward(sdSpeed);
        if (now - sdStateStart > SD_PROGRESS_MS) sdConsecutiveTurns = 0;
        return;
      }
      sdTurnDir = decideTurnDirection(leftBlocked, rightBlocked);
      sdConsecutiveTurns++;
      if (sdConsecutiveTurns >= SD_MAX_CONSECUTIVE) {
        sdTurnLen = ESCAPE_TURN_MS;  
        sdConsecutiveTurns = 0;
      } else {
        sdTurnLen = TURN_MS;
      }
      sdState = SD_BACKUP;
      sdStateStart = now;
      moveBackward(REVERSE_SPEED);
      break;

    case SD_BACKUP:
      moveBackward(REVERSE_SPEED);
      if (now - sdStateStart >= BACKUP_MS) {
        sdState = SD_TURN;
        sdStateStart = now;
        pivotTurn(sdTurnDir);
      }
      break;

    case SD_TURN:
      pivotTurn(sdTurnDir);
      if (now - sdStateStart >= sdTurnLen) {
        sdState = SD_FORWARD;
        sdStateStart = now;
      }
      break;
  }
}

bool readLineSensorRaw(int pin) {
  bool high = digitalRead(pin);
  return LINE_SENSOR_BLACK_IS_HIGH ? high : !high;
}

bool readFilteredDigital(int pin, int* history, int &historyIndex) {
  history[historyIndex] = readLineSensorRaw(pin) ? 1 : 0; 
  historyIndex = (historyIndex + 1) % LINE_FILTER_SAMPLES;
  int sum = 0;
  for (int i = 0; i < LINE_FILTER_SAMPLES; i++) sum += history[i];
  return sum > LINE_FILTER_SAMPLES / 2; // majority vote
}

bool readLeftLine()  { return readFilteredDigital(leftLineTrackPin, leftLineHistory, leftLineHistoryIndex); }
bool readRightLine() { return readFilteredDigital(rightLineTrackPin, rightLineHistory, rightLineHistoryIndex); }

void lineArc(int dir) {
  if (INVERT_LINE_STEERING) dir = -dir;
  if (dir > 0) driveNow(0, turnSpeed);  
  else         driveNow(turnSpeed, 0);   
}

void linePivot(int dir) {
  if (INVERT_LINE_STEERING) dir = -dir;
  if (dir > 0) driveNow(-searchSpeed, searchSpeed);
  else         driveNow(searchSpeed, -searchSpeed);
}



void lineSweep(unsigned long now) {
  unsigned long lostFor = now - recoverStart;
  if (RECOVER_NUDGE_MS > 0 && lostFor > RECOVER_NUDGE_MS && (lostFor % 1200) < 300) {
    driveNow(-searchSpeed, -searchSpeed);
    return;
  }

  if (now - sweepStart > sweepLen) {
    sweepDir = -sweepDir;
    sweepStart = now;
    sweepLen += SWEEP_GROW_MS;
  }
  linePivot(sweepDir);
}

void runLineTrack() {
  bool leftOn  = readLeftLine();
  bool rightOn = readRightLine();
  unsigned long now = millis();
  if (recovering) {
    bool aligned = leftOn && rightOn;
    bool desperate = (now - recoverStart > RECOVER_MAX_MS) && (leftOn || rightOn);
    if (aligned || desperate) {
      recovering = false;
    } else {
      lineState = LINE_LOST;
      lineSweep(now);
      return;
    }
  }
  if (leftOn && rightOn) {
    lineState = LINE_CENTERED;
    driveNow(lineSpeed, lineSpeed);
  } else if (leftOn) {
    if (lineState != LINE_LEFT) oneSensorSince = now; 
    lineState = LINE_LEFT;
    lastSeenDir = -1;                            
    lineArc(lastSeenDir);
  } else if (rightOn) {
    if (lineState != LINE_RIGHT) oneSensorSince = now;
    lineState = LINE_RIGHT;
    lastSeenDir = +1;                             
    lineArc(lastSeenDir);
  } else {
    bool sustainedDrift = (lineState == LINE_LEFT || lineState == LINE_RIGHT)
                          && (now - oneSensorSince >= ONE_SENSOR_MIN_MS);
    sweepDir  = sustainedDrift ? lastSeenDir : cornerTurnDir;
    sweepStart = now;
    sweepLen   = SWEEP_FIRST_MS;
    recoverStart = now;
    recovering = true;
    lineState  = LINE_LOST;
    lineSweep(now);
  }
}

const float HAND_BACKUP_CM = 5;    
const float HAND_STOP_CM   = 10;   
const float HAND_FOLLOW_CM = 30;   
const int HAND_BACKUP_SPEED = 90;      
const int HAND_MIN_CREEP_SPEED = 90;   
const int HAND_STEER_STEP = 40;        
const unsigned long HAND_SIGNAL_TIMEOUT_MS = 250; 

float readFilteredHandDistance() {
  float raw = readUltrasonicCM();
  if (raw >= 0) {
    handDistanceFiltered = (handDistanceFiltered < 0) ? raw : (handDistanceFiltered * 0.7 + raw * 0.3);
    lastValidHandReading = millis();
  }
  if (millis() - lastValidHandReading > HAND_SIGNAL_TIMEOUT_MS) return -1;
  return handDistanceFiltered;
}

void runHandFollow() {
  float distance = readFilteredHandDistance();
  bool leftBlocked = !digitalRead(leftIR);
  bool rightBlocked = !digitalRead(rightIR);

  if (distance < 0 || distance >= HAND_FOLLOW_CM) {
    stopMove(); 
    return;
  }

  if (distance < HAND_BACKUP_CM) {
    moveBackward(HAND_BACKUP_SPEED); 
    return;
  }

  if (distance < HAND_STOP_CM) {
    stopMove(); 
    return;
  }


  float t = (distance - HAND_STOP_CM) / (HAND_FOLLOW_CM - HAND_STOP_CM);
  int creepFloor = min(HAND_MIN_CREEP_SPEED, driveSpeed);
  int speed = constrain((int)(driveSpeed * t), creepFloor, driveSpeed);

  if (leftBlocked && !rightBlocked) {
    steerToward(-1, speed, max(speed - HAND_STEER_STEP, 0));
  } else if (rightBlocked && !leftBlocked) {
    steerToward(1, speed, max(speed - HAND_STEER_STEP, 0));
  } else {
    moveForward(speed);
  }
}


void showTuningValue(const __FlashStringHelper* label, float value, byte decimals) {
  lcd.setCursor(0, 0);
  lcd.print(label);
  lcd.print(value, decimals);
  lcd.print(F("        "));
  lcdShownAt = millis();
  lcdShowingTuning = true;
}


void checkLcdRestore() {
  if (lcdShowingTuning && millis() - lcdShownAt >= LCD_TUNING_SHOW_MS) {
    lcdShowingTuning = false;
    updateLCD();
  }
}


void notePhoneActivity() {
  if (!phoneActive) {
    phoneActive = true;
    updateLCD();
  }
}


void phoneJoystick(int steering, int throttle) {
  if (currentMode != MODE_MANUAL) return; 

  if (abs(throttle) < JOY_DEADZONE) throttle = 0;
  if (abs(steering) < JOY_DEADZONE) steering = 0;

  if (throttle == 0 && steering == 0) {
    if (activeMoveKey == KEY_NONE) stopMove(); 
    return;
  }


  int base = ((long)throttle * driveSpeed) / JOY_CENTER;
  int diff = ((long)steering * driveSpeed) / JOY_CENTER;
  drive(base - diff, base + diff);
}


void phoneMessage(char* id, char* val) {
  notePhoneActivity();


  if (id[0] == 'd') {
    char* comma = strchr(val, ',');
    if (comma) {
      *comma = 0;
      phoneJoystick(atoi(val) - JOY_CENTER, atoi(comma + 1) - JOY_CENTER);
    }
    return;
  }


  if (id[0] == 'b') {
    if (val[0] == '1') {
      int n = atoi(id + 1);
      if (n >= 0 && n <= 3) setMode((Mode)n);
    }
    return;
  }


  if (id[0] == 's' && id[1] == 'l') {
    int n = atoi(id + 2);
    int v = constrain(atoi(val), 0, SLIDER_MAX);
    switch (n) {
      case 0: 
        LEFT_TRIM = 1.00 + (0.40 * v) / (float)SLIDER_MAX;
        showTuningValue(F("TRIM "), LEFT_TRIM, 3);
        break;
      case 1:
        lineSpeed = map(v, 0, SLIDER_MAX, 80, 200);
        showTuningValue(F("LINE SPD "), lineSpeed, 0);
        break;
      case 2:
        sdSpeed = map(v, 0, SLIDER_MAX, 80, 200);
        showTuningValue(F("SELF SPD "), sdSpeed, 0);
        break;
      case 3:
        ESCAPE_TURN_MS = map(v, 0, SLIDER_MAX, 400, 2000);
        showTuningValue(F("TURN "), ESCAPE_TURN_MS, 0);
        break;
    }
  }
}

void handlePhone() {
  while (Serial.available()) {
    char c = Serial.read();
    if (c == 1) {                
      mbState = 1; mbIdLen = 0; mbValLen = 0;
    } else if (c == 2 && mbState == 1) {   
      mbState = 2;
    } else if (c == 3 && mbState == 2) {  
      mbId[mbIdLen] = 0;
      mbVal[mbValLen] = 0;
      phoneMessage(mbId, mbVal);
      mbState = 0;
    } else if (mbState == 1) {
      if (mbIdLen < sizeof(mbId) - 1) mbId[mbIdLen++] = c;
    } else if (mbState == 2) {
      if (mbValLen < sizeof(mbVal) - 1) mbVal[mbValLen++] = c;
    }
  }
}

RemoteKey decodeKeyValue(long result) {
  switch (result) {
    case 0x16: return KEY_0;
    case 0xC:  return KEY_1;
    case 0x18: return KEY_2;
    case 0x5E: return KEY_3;
    case 0x8:  return KEY_4;
    case 0x1C: return KEY_5;
    case 0x5A: return KEY_6;
    case 0x42: return KEY_7;
    case 0x52: return KEY_8;
    case 0x4A: return KEY_9;
    case 0x9:  return KEY_PLUS;
    case 0x15: return KEY_MINUS;
    case 0x7:  return KEY_EQ;
    case 0xD:  return KEY_USD;
    case 0x19: return KEY_CYCLE;
    case 0x44: return KEY_PLAY_PAUSE;
    case 0x43: return KEY_FORWARD;
    case 0x40: return KEY_BACKWARD;
    case 0x45: return KEY_POWER;
    case 0x47: return KEY_MUTE;
    case 0x46: return KEY_MODE;
    default:   return KEY_ERROR;
  }
}

void printKeyName(RemoteKey k) {
  switch (k) {
    case KEY_0: Serial.print(F("0")); break;
    case KEY_1: Serial.print(F("1")); break;
    case KEY_2: Serial.print(F("2")); break;
    case KEY_3: Serial.print(F("3")); break;
    case KEY_4: Serial.print(F("4")); break;
    case KEY_5: Serial.print(F("5")); break;
    case KEY_6: Serial.print(F("6")); break;
    case KEY_7: Serial.print(F("7")); break;
    case KEY_8: Serial.print(F("8")); break;
    case KEY_9: Serial.print(F("9")); break;
    case KEY_PLUS: Serial.print(F("+")); break;
    case KEY_MINUS: Serial.print(F("-")); break;
    case KEY_EQ: Serial.print(F("EQ")); break;
    case KEY_USD: Serial.print(F("U/SD")); break;
    case KEY_CYCLE: Serial.print(F("CYCLE")); break;
    case KEY_PLAY_PAUSE: Serial.print(F("PLAY/PAUSE")); break;
    case KEY_FORWARD: Serial.print(F("FORWARD")); break;
    case KEY_BACKWARD: Serial.print(F("BACKWARD")); break;
    case KEY_POWER: Serial.print(F("POWER")); break;
    case KEY_MUTE: Serial.print(F("MUTE")); break;
    case KEY_MODE: Serial.print(F("MODE")); break;
    case KEY_NONE: Serial.print(F("NONE")); break;
    default: Serial.print(F("ERROR")); break;
  }
}

 </code></pre>
</div>

# Bill of Materials
| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| SunFounder R3 Board | Used for prototyping electronics and robotics. | $15 | <a href="https://eu.robotshop.com/products/sunfounder-uno-r3-control-board"> Link </a> |
|L9110 Motor Driver Module|Used to control the speed and direction of up to two DC motors. | $7.79 for 5 | <a href="https://www.amazon.com/HiLetgo-H-bridge-Stepper-Controller-Arduino/dp/B00M0F243E"> Link </a> |
| TT Motor|Used to power the wheels of robot cars. | $9.99 for 6| <a href="https://www.amazon.com/AEDIKO-Motor-Gearbox-200RPM-Ratio/dp/B09N6NXP4H/ref=sr_1_1?adgrpid=190073077647&dib=eyJ2IjoiMSJ9.VFvmjZ6X2lLerHG6wM2_Z7nBQu_C6jwuUD0a5dykvPqqIOYn1t4CYgMQikHFRydWNrOgcg7Du12M0TNoCAvWLVd1a2jxXPN89_YAP8k3av3yWcEe0zJEz8EegRZZENvovEA_Nxpos9FSG22ubw1-FQ0RTkh9oQvk0QtiFDFym883joE2MHcwbHHQBOIFutuVw5QiKR0Wkr8jlKaGVxiPnozER0ukB109xIuyg7vBR245Ll2KqQCOdxF4hvL4uji3LAy5XU2FbyjgbnGcrPrAIMQMcL3brDmLYshb1MSu28I.geDsvCohdlEJhfN60OlsG9D3NIqJRr1RVnkw7HFSc94&dib_tag=se&hvadid=779556177337&hvdev=c&hvexpln=0&hvlocphy=9004329&hvnetw=g&hvocijid=7982671839558946546--&hvqmt=e&hvrand=7982671839558946546&hvtargid=kwd-1930315191&hydadcr=3535_13857088_11052&keywords=tt+motor&mcid=116ed5dee20b3426a82053fa2363321d&qid=1784582678&sr=8-1"> Link </a> |
| Ultrasonic Module | Uses sound waves to measure distance without physical contact. | $8.99 for 5 | <a href="https://www.amazon.com/ELEGOO-HC-SR04-Ultrasonic-Distance-MEGA2560/dp/B01COSN7O6"> Link </a> |
| Obstacle Avoidance Module |Uses an infrared transmitter and receiver to detect nearby objects.| $9.96 for 10 | <a href="https://www.amazon.com/OSOYOO-Infrared-Obstacle-Avoidance-Arduino/dp/B01I57HIJ0"> Link </a> |
|9V Batteries| Used to Power the Car | $8.99 for 10 | <a href="amazon.com/PKCELL-9V-Batteries-Battery-Detector/dp/B00ZTS55Y4/ref=sr_1_1_sspa?crid=136RUNZNEKEEP&dib=eyJ2IjoiMSJ9.8xIC2eXJTnIdYA30fCJOnwpj50bL6qiESVMJBDb6SNyQ4dL_0_l4gmMTjMviAMDgoAjMG_wPsSVU03QwXiavauZRuNcPo87IYGO8h3w0JQmbUsuURQ6InWWDWLfvBN7Ahumt4syvHh6RUCMKjkvnrmaNkw1wcve4oVFVdX9gVuxtwBrB8jfP7xjJS8262pwbiuxBLUn3L9Kv487GPg3lPFjWWjr1eA59uwvrZGCmGDgqUo7qQqw7VfgJGsTVoSSBI2-zljurPNlQGg_MdCjz1q4OuiC0tTa5bUeFKfOV0_Y.7feILipDBbT7RmXkSqrm4jOC7CXmGrCHXt6qp9p2VCQ&dib_tag=se&keywords=9v+batteries&qid=1784583169&rdc=1&s=electronics&sprefix=9+v+%2Celectronics%2C110&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| Small BreadBoard | Used to organize wires | $5.99 for 6 | <a href="https://www.amazon.com/WWZMDiB-SYB-170-Breadboard-Plates-Multicolored/dp/B09YXQJMTG/ref=sr_1_2_sspa?crid=3CD4E2Z1KHHPK&dib=eyJ2IjoiMSJ9.izD01hGoehT6aqD9wbs5-QgpQ2udoLGHXOy-GNGcnLvnLVxQ0ySPHTRRot-0oo173NyWqBt50b91QsIj_C_1AKHwHETxALf9zhSnQ7oJUzhnxQ9w_OhUpSJZp7MI2z5pPimEBy_UZgwRwIjUmrKldaixzr7eB_f7fdgh1VBDgA7O-p0WMl8AD4sdnVQjd3p3thzJwB175aLMYGFjKLpTvpaU_6oyG5Cr193PIrSI-rvlZsesyWeXGh32-pFyDNbpFq0QbPOGeFkmwWEajCKwATrnxEyJB4NPhSkPNFmAFv8.xEEs_i3anQTuLZT4xIAVnz1SMMCJRKmFRIYuINnJwZQ&dib_tag=se&keywords=small+breadboard&qid=1784583233&s=electronics&sprefix=small+breadboard%2Celectronics%2C106&sr=1-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
|Jumper Wires| Used to connect components, breadboards, or circuit boards without soldering | $6.98 for 120 | <a href="https://www.amazon.com/Elegoo-EL-CP-004-Multicolored-Breadboard-arduino/dp/B01EV70C78/ref=sr_1_1_sspa?crid=PXOD9MMRX0H3&dib=eyJ2IjoiMSJ9.tjHxIQLJsk16_0YVtUGN6erd5BDBwPCXF98pEA_v-6tuB6AWi5A5z-nJ-MVkqPYvYbcPR7RCNLkAKwZk0KfdMa2A-_B7iFJNKz0BIdenBsHE-RAYTtoJfmT_suO-tl_h-1V6AdShARMODAO5HUppHSv-cCCKVz9vNtygBHAPPNQa5-JJQ3dNEysuztTv1kIXbBaRMKy4w9EzAVmcHRBCQPKZtKVQCl2phJrTlpadct9XBNDJptnNvofBS699oCOAfUhmsNDB08NIYs4QXc0Mf4QT4IRBEuRbm8QlAQz_DxQ.LbqvskcJVhHJNijUHeYpJ-jPrnRCMgUWOis9EC0gV9E&dib_tag=se&keywords=Jumper%2Bwires&qid=1784583322&s=electronics&sprefix=jumper%2Bwires%2Celectronics%2C106&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| Arduino USB Cable | Used to upload code from your computer to the board, provide operating power, and allow serial communication between the Arduino and the IDE. | $7.99 | <a href="https://www.amazon.com/Arduino-Data-Sync-Cable-Microcontroller/dp/B08RCJXY1Z/ref=sr_1_2_sspa?crid=NCRMCC96SMKT&dib=eyJ2IjoiMSJ9.4_ZhDX5GMYMW0_L6Pn9rz2Wp3T_M5sgp4fwIaCkGTYw8Ym9FiweWWPMchlMk6FyGCG320--8vb78bNBix_P_B5H0G2mftfh7IPNgklFbuF_3dT9ugyUa1l42DkHYwoY1uT1nU3Ux-fVqIp66zD_UJVtkKeA6_-U8hWR722kk5BV1y4ci9InER4hr3GqsXaZ0bSYprjqlZ35WdT-Ec9kSZje6zQ4KjlgG0FBT5q22W2E.0tCWhcZM8js85gwgQREo8Tll6XyaFqKYBhnNtfzUcqE&dib_tag=se&keywords=USB+Cable&qid=1784583439&sprefix=usb+cable%2Caps%2C124&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| M 2.5x6,3x10,3x6,3x30 Screw and M 2.5x11,3x24,3x12 Standoff and Screwdriver |Used to secure things | $9.999| <a href="https://www.sunfounder.com/products/nylon-screws-kit?srsltid=AfmBOooD29WSCB8SlcIg-gVC2-4dRu7FOmrLItz_r_IKrDvoGbrZM8Tb"> Link </a> |
| TT Wheels| Used for the Motion of the Car and synced to TT Motor  | $8.97 for 8 | <a href="https://www.amazon.com/ThtRht-Motor-Wheels-Replacement-Smart/dp/B0CG1C7T8J/ref=sr_1_1?dib=eyJ2IjoiMSJ9.ZrpNmrbicccc2COTV1s2mCEsEJTAUea5_2CFRsGAz92T-_D2Ae4N8ofcLTHda_0he0dEgPhI_ZzWvPuUP3MhDr7X47bI8PgOF389a0uR4R2TE9GxbJxTBiewW23a0W3JaENJOSdhDsrQuAErW6aBHsWUm6hFnOdz43yf5VFmULojd34zVyPTFEZVKCJf216u9keFRepgvY7GE6LsA2v3v4jb9YB288_eLSqTP73FVghuF_0dkFJiK8aY9BgWOskrxtNXcqVuu7-Xn3gRX_3l7cAhU4_ItyBLlhU4U076oBY.hSbZULzGySTuizhEs64PuyZvMh5b7aIAlB5Sxo_kOu8&dib_tag=se&keywords=tt+motor+wheel&qid=1784583968&sr=8-1"> Link </a> |
|Universal Wheel | Used for extra stablility  | $8.99 for 4 | <a href="amazon.com/Dalyndar-Replacement-Universal-Rollers-Furniture/dp/B0DRX77FLV/ref=sr_1_3?crid=1ELDU0ZHZRHMG&dib=eyJ2IjoiMSJ9.Jyhhe1k2AZk0VOGaWFzYNhMZ1oC4peq1Um18nUTrwaRNtIRbBU9y0q1TmgyH0HguQKftY8-75UrnsSXSmNEBhTE9D6TD62Ikkf7uhmW-p77jfDjSoQbRZzpN0Sur40DNBIk4OYtafmxnR1zpx53P5MQBNi5o_r9P4J12Bi7Shwcz5lcgrVISfXSAetuS0OEPoPMsAqlHuG8xsrtwfr0yW97MqLtclyII1_AfyuvTZUw.GCsH5CRSE7ZvrVeUnOjoCH5f1miV3gSl4cWaXLU_nRw&dib_tag=se&keywords=universal+wheel+small&qid=1784584030&sprefix=universal+wheel+samkk%2Caps%2C115&sr=8-3"> Link </a> |
| Velcro | Used to fasten thingss| $8.24 for 12 | <a href="https://www.amazon.com/Melsan-inch-Hook-Loop-Tape/dp/B07Y3SZCRY/ref=sr_1_2_sspa?crid=243TMF6EXLQPO&dib=eyJ2IjoiMSJ9.Wqxj0Djxnfg8ZyY40TUmW_SA3xs4VISa62X9I26GI3wUbEATrTasKP86pYI-7JqkVrWMDd26poJEG9pLEdcY1c6SJP4xoa95dWacivkjnE7eKQnMsXtVFDJogME_Flo6ejIF4--t-Caz2K7rgI205My2vMGwpTTfSFDLHVEbQuvOc-GluEuoes8DQ5xPKpJWUo0-qmTmqA-XOAxy1qBMonmeUDVrJEZ8RPi2N-8PIL4.sK1fV1X5HoJRpKG2vr7vWmNfHMwbbY0XMc-d0YE2w2Q&dib_tag=se&keywords=velcro%2Bstrips&qid=1784584091&sprefix=velcro%2Bstrips%2Caps%2C139&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| IR Reciver| Used for wireless remote control  | $9.99 | <a href="https://www.amazon.com/ALLECIN-Infrared-Emitter-Receiver-Receivers/dp/B0D72WY17V/ref=sr_1_1_sspa?dib=eyJ2IjoiMSJ9.vsgpGIwckTFj8X9ZgQhKYimGOqm7UNP3i1Zp5cdrwl4qNbSgjz-qwKnvDzRp8NECC9bp9OwiBUoY20clK2Ecg84xPG_DGJ9jvkKsXwGKIxzyoPRbPawW0guMEAga0Kr1nxQpv4eSajweWBwSAUlddALrwc9NTZRLYZBYvN8GQ1B7Z6HGWz9kwxa_mVRbaYKbkiFsRG4zLioAe89Upcpj5SOfCMgfO1-oGyZ8qUSVIhA.W2xpGFOC3YxsDN4Ti0PWfE07-rK5e169I_c9sJM7Yhs&dib_tag=se&keywords=ir+receiver&qid=1785159940&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| I2C LCD | Used to show text and numbers in microcontroller projects using only four pins | $12.99 for 3 | <a href="amazon.com/Hosyond-Display-Module-Arduino-Raspberry/dp/B0BWTFN9WF/ref=sr_1_1_sspa?crid=2HJ9V8D7ZO8X3&dib=eyJ2IjoiMSJ9.KbkFF5Phxvs3jUkctMNx9Dr8xY5EDei7_yjyLwsZEjbq4rNvsROve9sD-HuuYfFT6KLh2CHnToSyfz2lIQYkcOonKEVrQSSjXWHDYZCqlOuzL4frS-JAlnQP4NrULPFcRKOHzgb8740ONoXXwk_LMlW-F_yoGeNx4Fm9GNnVmlG4kU_1nFZ5gt1JDfTwuBxhUjiCvfBZVRQV8Ue-YnYTPwLjC6w1Tjve0Ii91VFVuFM.LiXFjwYKAFezPN09TvJcC9oTKDtq2XSodOUsqmMTXNQ&dib_tag=se&keywords=i2c+LCD&qid=1785160012&sprefix=i2c+lcd%2Caps%2C156&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| Remote Control |Used to operate systems from a distance by sending wireless signals via infrared light or radio wave. | $9.59 for 6| <a href="https://www.amazon.com/DWEII-Infrared-Wireless-Control-Raspberry/dp/B09ZTZQFP7/ref=sr_1_3?crid=2D12U6Q937OI0&dib=eyJ2IjoiMSJ9.umdx3yFamLqVj7OmxopGeyCMUPdn_CMltLj92RC4LOnuXzpGs8iJ3fJvci-K9PasbdqcpBnfZuSz9h_Ff6IAnqUqkLvTq-eymTmLkh852CEHoNB_C8O1LgVzUzZ-ekkXxAcGAKUz5dkrC0sCs4j-1NEYOU-Ah34tTlGu-iAWgbTATmM4uzRhEt10J5ZdAexk1RCvYwRhF3a4b3m8J7KVkHIuMhrLYivZI1RcUBWDWX0.q2IF9MWFIuCgumtKEmHjR7XE3c9-g9o_FaUgcmk68EU&dib_tag=se&keywords=remote+control+for+arduino&qid=1785160138&sprefix=remote+control+for+arduino%2Caps%2C118&sr=8-3"> Link </a> |
|DSD TECH HM-10 Bluetooth 4.0|Used to add wireless serial communication to Arduino. | $10.99 for 1 | <a href="https://www.amazon.com/DSD-TECH-Bluetooth-iBeacon-Arduino/dp/B06WGZB2N4/ref=sr_1_1_sspa?crid=3M7T6OI7DWYJH&dib=eyJ2IjoiMSJ9.EHGqURdubuZgU6hgPq-REgXKdLCLMK-5LsQTe1xdNzhLW-vFXL4nFxYw0DaA1Y_WT5mJ_I_itfYzgzs7DjEK9k4o7fX2XVaLSxjacR_Vo8OXC1osjpgmT2ZQRWI-ZwvkqxsCDmArKouHRvJ6U00gomWxxTXCuAVtpmnhkCP0tCuCZqwqb6yrocTqY-5S8gC-saLwkRWhbkk_QPNaXIYYs2pSM1yqtQSsNTlKpI4NWHIyJH5dMzy-DppiEnbuJj4MOqYItVYb3yy-OjCT-O17fhK6gGsCfH0-bMVKv-xrWgo.Ed0QKYtSLQpjqKOmqNQx8V2aBIJrsKU5Kx0WpMcwX_E&dib_tag=se&keywords=SD+TECH+HM-10+Bluetooth+5.0+BLE+Module+with&nsdOptOutParam=true&qid=1785510168&s=electronics&sprefix=sd+tech+hm-10+bluetooth+5.0+ble+module+with%2Celectronics%2C88&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.

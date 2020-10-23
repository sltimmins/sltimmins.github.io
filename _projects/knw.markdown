---
layout: page
title: Introduction to Engineering Design
permalink: /knw/
---

Introduction to Engineering Design is a design and team based course. The course requires the design, construction, and implementation of a fully autonomous robot that must complete several tasks, such as navigating a maze, measuring the salinity percentage of a water sample and making a decision based on that reading, and the collection and deposit of items. During my time in this course I was behind the design, construction, and wiring of the physical robot and the implementation of the navigational code. I took this course in the Spring of 2020 and was consequently sent home due to Covid-19. When this happened tasks were split amongst our team and I took the navigational portion of the robot. The task for navigation for the remainder of the semester was to build and implement a system that could navigate through a maze, correct its direction of movement after motor slippage during turns. Below you can find images of the original robot's design modeled in Solidworks and the implementation of the navigational code written both completed by myself.

I now serve as a teaching assistant for this class providing current students with technical knowledge and a teaming mentor. In this course TAs are assigned to individual teams in order to provide the students with someone they can feel comfortable discussing teaming issues. As a TA I also act as a mentor to the teams by providing technical know how and assistance when necessary.

:-------------------------:|:-----------------------------:|:---------------------------:
![topView](/images/Top.jpg)|![frontView](/images/Front.jpg)|![sideView](/images/Side.jpg)

{% highlight C++ %}
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>
#include <NewPing.h>
#include <Servo.h>
#include <Keypad.h>
#include <navSlippage.h>

#define TRIGGER_PIN  12  // Arduino pin tied to trigger pin on the ultrasonic sensor.
#define ECHO_PIN     11  // Arduino pin tied to echo pin on the ultrasonic sensor.
#define MAX_DISTANCE 200 // Maximum distance we want to ping for (in centimeters). Maximum sensor distance is rated at 400-500cm.
const byte ROWS = 4; //four rows in keypad
const byte COLS = 4; //four columns in keypad
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {39,41,43,45}; //connect to the row pinouts of the keypad
byte colPins[COLS] = {47,49,51,53}; //connect to the column pinouts of the keypad

Keypad keypad = Keypad(makeKeymap(keys),rowPins,colPins,ROWS,COLS);
LiquidCrystal_I2C lcd(0x27,2,1,0,4,5,6,7,3,POSITIVE);  // Set the LCD I2C address and parameters
Servo myservo;  // Create servo object to control a servo
int pos = 0;    // Variable to store the servo position angle

void setup() {
  lcd.begin(16,2);  // Initialize the lcd 
  lcd.clear();       // Clear screen and go to the top line
  lcd.blink();      // Enable blinking cursor
  myservo.attach(9);  // Attach the servo on pin 9 to the servo object
  delay(1000);
}

void loop() {
  // put your main code here, to run repeatedly:
  NewPing sonar(TRIGGER_PIN, ECHO_PIN, MAX_DISTANCE); // NewPing setup of pins and maximum distance.
  
  char key, distanceChar;
  unsigned int cm, distance;
  double inches, actualAngle, angleToTurn = 90, uS = 1;
  String printScreen;

  key = keypad.getKey(); //gets key input

  if(key) //when there is a key input
  {
    lcd.clear(); //clears LCD display
    printScreen = "Command: " + String(key); //prints the key to the first line of the display
    lcd.print(printScreen); 
    lcd.setCursor(0,1); //moves cursor to next line

    if(key == '1') //stop 3" from a wall
    {
      lcd.clear();
      pos = 90;
      myservo.write(pos);
      lcd.print("Stop at? ");
      while(uS > 0) //while ping sensor is getting a distance reading it will loop
      {
        uS = sonar.ping(); //takes time reading
        lcd.clear(); //clears LCD to display distance
        cm = uS / US_ROUNDTRIP_CM; //converts time to cm
        inches = double(cm) / 2.54; //converts cm to inches
        printScreen = String(inches) + " inches"; //prints distance in inches to LCD
        lcd.print(printScreen);
        if(inches <= 3.0) //when the distance becomes less than 3" lcd will display stop
        {
          lcd.clear();
          lcd.print("3.00 inches");
          lcd.setCursor(0,1);
          lcd.print("STOP");
          break;
        }
        delay(500);
      }
    }

    else if(key == '2') //find which direction to turn
    {
      pos = 180;
      myservo.write(pos); //move servo/ping sensor to face right
      delay(1000);
      uS = sonar.ping();
      lcd.clear();
      cm = uS / US_ROUNDTRIP_CM; //convert ping reading to cm

      if(cm > 35) //if hallway detetcted
      {
        delay(1000);
        pos = 90;
        myservo.write(pos); //return servo to be straight
        delay(500);
        actualAngle = navSlippage(angleToTurn);
        printScreen = "Turn right " + String(actualAngle) + "°"; //prints angle to turn to the right
        lcd.print(printScreen); 
      }
      else if(cm <= 35) // if wall detected
      {
        pos = 0;
        myservo.write(pos); //turns servo/ping sensor to the left
        delay(1000);
        uS = sonar.ping();
        lcd.clear();
        cm = uS / US_ROUNDTRIP_CM; ;//converts new reading to cm

        if(cm > 35) //if hallway detected
        {
          delay(1000);
          pos = 90; 
          myservo.write(pos); //resets servo to face forward
          delay(500);
          actualAngle = navSlippage(angleToTurn);
          printScreen = "Turn left " + String(actualAngle) + "°"; //prints the angle to turn left
          lcd.print(printScreen);
        }
        else //if in dead end
        {
          pos = 90; 
          myservo.write(pos); //resets servo to face forward
          lcd.print("Maze completed"); //prints to go back on display
        }
      }
    }
    
    else if(key == '3') //find a gap instruction
    {
      NewPing sonar(TRIGGER_PIN, ECHO_PIN, 50);
      
      lcd.clear(); //clear lcd to display gap data
      pos = 0;
      myservo.write(pos); //turn servo to face the left
      delay(1000);
      
      for(int i = 0; i < 16; i++)
      {
        uS = sonar.ping();
        if(uS > 0)
        {
          lcd.print('X');
        }
        else
        {
          lcd.print('0');
        }
        delay(1000);
      }
      
    }

    else if(key == '4') //stop 4 inches from wall
    {
      lcd.clear();
      pos = 90;
      myservo.write(pos);
      delay(1000);
      while(uS > 0) //while ping sensor is getting a distance reading it will loop
      {
        uS = sonar.ping(); //takes time reading
        lcd.clear(); //clears LCD to display distance
        cm = uS / US_ROUNDTRIP_CM; //converts time to cm
        inches = double(cm) / 2.54; //converts cm to inches
        printScreen = String(inches) + " inches"; //prints distance in inches to LCD
        lcd.print(printScreen);
        if(inches <= 4.0) //when the distance becomes less than 3" lcd will display stop
        {
          lcd.clear();
          lcd.print("4.00 inches");
          lcd.setCursor(0,1);
          lcd.print("STOP");
          break;
        }
        delay(500);
      }
    }

    else if(key == '5') //stop 5 inches from wall
    {
      lcd.clear();
      pos = 90;
      myservo.write(pos);
      delay(1000);
      while(uS > 0) //while ping sensor is getting a distance reading it will loop
      {
        uS = sonar.ping(); //takes time reading
        lcd.clear(); //clears LCD to display distance
        cm = uS / US_ROUNDTRIP_CM; //converts time to cm
        inches = double(cm) / 2.54; //converts cm to inches
        printScreen = String(inches) + " inches"; //prints distance in inches to LCD
        lcd.print(printScreen);
        if(inches <= 5.0) //when the distance becomes less than 3" lcd will display stop
        {
          lcd.clear();
          lcd.print("5.00 inches");
          lcd.setCursor(0,1);
          lcd.print("STOP");
          break;
        }
        delay(500);
      }
    }

    else if(key == '6') //stop 6 inches from wall
    {
      lcd.clear();
      pos = 90;
      myservo.write(pos);
      delay(1000);
      while(uS > 0) //while ping sensor is getting a distance reading it will loop
      {
        uS = sonar.ping(); //takes time reading
        lcd.clear(); //clears LCD to display distance
        cm = uS / US_ROUNDTRIP_CM; //converts time to cm
        inches = double(cm) / 2.54; //converts cm to inches
        printScreen = String(inches) + " inches"; //prints distance in inches to LCD
        lcd.print(printScreen);
        if(inches <= 6.0) //when the distance becomes less than 3" lcd will display stop
        {
          lcd.clear();
          lcd.print("6.00 inches");
          lcd.setCursor(0,1);
          lcd.print("STOP");
          break;
        }
        delay(500);
      }
    }

    else if(key == 'A') //course correction
    {
      lcd.clear();
      pos = 0;
      myservo.write(pos);
      delay(1500);
      uS = sonar.ping();
      double current = uS / US_ROUNDTRIP_CM;
      double last = 10000;
      lcd.print("Begin turning");
      delay(100);
      while(true)
      {
        last = current;
        delay(100);
        uS = sonar.ping();
        current = uS / US_ROUNDTRIP_CM;
        delay(100);
        if(current > last)
        {
          break;
        }
        delay(200);
      }
      lcd.clear();
      lcd.print("STOP TURNING");
      lcd.setCursor(0,1);
      lcd.print("CONTINUE FORWARD");
    }

    else if(key == 'B') //navigate moveable barrier
    {
      pos = 90;
      myservo.write(pos);
      uS = sonar.ping();
      cm = uS / US_ROUNDTRIP_CM;
      while(cm < 10)
      {
        lcd.clear();
        lcd.print("WAIT");
        delay(100);
        uS = sonar.ping();
        cm = uS / US_ROUNDTRIP_CM;
      }
      lcd.clear();
      lcd.print("GO");
    }

    else if(key == 'C') //follow a wall
    {
      double distanceFromWall = 0;
      double nextDistance = 0;
      lcd.clear();
      pos = 180;
      myservo.write(pos);
      delay(1000);
      uS = sonar.ping();
      distanceFromWall = uS / US_ROUNDTRIP_CM;
      lcd.print("Forward");
      for(int i = 0; i < 5; i++)
      {
        delay(2000);
        lcd.clear();
        uS = sonar.ping();
        nextDistance = uS / US_ROUNDTRIP_CM;
        if(nextDistance > distanceFromWall)
        {
          lcd.print("Angle to the");
          lcd.setCursor(0,1);
          lcd.print("right");
          distanceFromWall = nextDistance;
        }
        else if(nextDistance < distanceFromWall)
        {
          lcd.print("Angle to the");
          lcd.setCursor(0,1);
          lcd.print("left");
          distanceFromWall = nextDistance;
        }
        else
        {
          lcd.print("Continue");
          distanceFromWall = nextDistance;
        }
      }
      lcd.clear();
      lcd.print("DONE");
    }
  }
}

{% endhighlight %}
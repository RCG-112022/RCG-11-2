# RCG-11-2

Readme for WRO

For the exoskeleton of the vehicle, one of a 4x4 vehicle was taken, it was disassembled and its wheels and suspension were taken. These pieces were cleaned to remove dust and any liquid they might have so they could get the most out of them.

For the Arduino board, a base made of Lego with different colors and sizes was used to be able to adjust the Arduino board to size and not move when the robot moved, in turn having done it with Lego allowed us more ease when mounting the wheels to the base. When implementing Lego pieces, many pieces had to be found because if we did not put enough and they were not reinforced, the robot had little stability and was quite prone to being damaged or broken. A sheet was used to give the vehicle more stability and to match its suspension for a more aesthetic look. Due to the fact that the electromechanical components were somewhat large, the base of the vehicle was modified to make it have more space and to be able to fit the components successfully and safely. Once the board was made, they tried to put the cables and these gave problems, so modifications were made for the second time, some kinds of windows were added to make the cables pass through them and be able to be connected impeccably and without problems.

For the electromechanical components, holes were implemented in the base to be able to join them to the body without much difficulty and thus provide a more organized and futuristic look. It should be noted that external purchases were made to better accommodate the components and not affect the mobility of the vehicle.

Regarding the wheels, the wheels of the 4x4 vehicle were used to be able to provide rotation and thus not waste much time making the robot stop and then turn, however, thanks to this rotation we can make the robot make a zigzag path and save us time and space on the track.

The robot is constantly being modified by recommendations of the trainer and some electromechanical components that require more space, several trials and errors are still made and the decision was made to return to the original design to re-arrange the components and find a better way of dock the ones that take up more space.

Most of the robot was built outside the educational institution, at a teammate's house, brainstorming was done at the institution and some of the ideas were discussed with the trainer in order to make adjustments or review other aspects of the vehicle. with the coach to receive feedback on how we were doing in different aspects.

On the subject of programming, we worked both at the educational institution and at home, the trainer provided us with a guide so that we could make our code in Arduino and then be able to store it on the board and make the robot execute the actions required for the competition. It is worth mentioning that it was tested several times to check the behavior of the components and the Lego base to see how stable the suspension of the vehicle was, together with this, it was tested to verify that the sheet did its job effectively and kept the vehicle equal.

Each electromechanical component was programmed individually to work more orderly and make the code easy to understand. The programming was done in Tinkercad to make it easier to code and test, then it was passed to Arduino.

It was programmed separately to maintain order and avoid confusion when storing it on the plate to give the instructions to the robot. Everything was programmed from scratch, there were some errors in the code, especially with the programming of the servomotors and the ultrasonic sensor, but they were gradually resolved. To make the work more dynamic and check learning equally, the programming of each electromechanical component was divided.

The track that will be used in the competition was measured to know exactly at what distance the car should be programmed so that it turns and does not crash into any wall or obstacle that may be found on the way.

After having tested, we realized that the robot did not move, so it was decided to make the base with a wooden board and the idea of having the pieces so separated was discarded, since having them so separated meant that there was not much traction. The wooden top helped us to have more traction and we were able to place more batteries and correct the problem of the power source. It was tested in this way and the robot started to work.

At the end, the code began to be tested to see if there was any error in execution or language and to be able to correct it so that the robot would do an impeccable job and everything would be perfect for the day of the competition.

Without a doubt an experience with many obstacles and challenges throughout the journey, several restarts and changes, many discarded ideas and many ideas that worked, many design changes and additional modifications, but with hard work and effort a good job could be done with their pros and cons, and above all, synergy and camaraderie were reinforced since all the work was done together and with respect for teammates, it should be noted that other teammates from different teams also contributed giving us ideas that we were able to implement in the final vehicle, also our team coach helped us a lot with the design, and programming code of the vehicle, This was an unforgettable and very rewarding experience, a pleasure to have been part of it, and totally would like to be part of it another time soon.

Code:
//COLOR SENSOR SETUP
  //Declarations:
  pinMode(S0, OUTPUT);
  pinMode(S1, OUTPUT);
  pinMode(S2, OUTPUT);
  pinMode(S3, OUTPUT);
  pinMode(13, OUTPUT);
  pinMode(sensorOut, INPUT);
  // Set frequency scaling to 20%:
  digitalWrite(S0, HIGH);
  digitalWrite(S1, LOW);
  Serial.begin(9600);//begin serial communication
  calibrate();//calibrate sensor (look at serial monitor)
  //--------------------
  myservo.attach(A2);
  M_Control_IO_config();     //motor controlling the initialization of IO
  Set_Speed(Lpwm_val, Rpwm_val); //setting initialized speed
  myservo.write(DuoJiao);       //setting initialized motor angle
  pinMode(inputPin, INPUT);      //starting receiving IR remote control signal
  pinMode(outputPin, OUTPUT);    //IO of ultrasonic module
  Serial.begin(9600);            //initialized serial port , using Bluetooth as serial port, setting baud
  stopp();                       //stop
  delay(1000);
}
void loop()
{
  Self_Control();
}

//COLOR SENSOR FUNTIONS
void decideColor() {//format color values
  //Limit possible values:
  redColor = constrain(redColor, 0, 255);
  greenColor = constrain(greenColor, 0, 255);
  blueColor = constrain(blueColor, 0, 255);
  //find brightest color:
  int maxVal = max(redColor, blueColor);
  maxVal = max(maxVal, greenColor);
  //map new values
  redColor = map(redColor, 0, maxVal, 0, 255);
  greenColor = map(greenColor, 0, maxVal, 0, 255);
  blueColor = map(blueColor, 0, maxVal, 0, 255);
  redColor = constrain(redColor, 0, 255);
  greenColor = constrain(greenColor, 0, 255);
  blueColor = constrain(blueColor, 0, 255);

  //decide which color is present (you may need to change some values here):
  if (redColor > 250 && greenColor > 250 && blueColor > 250) {
    color = 0;//white
  }
  else if (redColor < 25 && greenColor < 25 && blueColor < 25) {
    color = 0;//black
  }
  else if (redColor > 200 &&  greenColor > 200 && blueColor < 100) {
    color = 1;//yellow
  }
  else if (redColor > 200 &&  greenColor > 25 && blueColor < 100) {
    color = 1;//orange
  }
  else if (redColor > 200 &&  greenColor < 100 && blueColor > 200) {
    color = 1;//purple
  }
  else if (redColor > 250 && greenColor < 200 && blueColor < 200) {
    color = 1;//red
  }
  else if (redColor < 200 && greenColor > 250 && blueColor < 200) {
    color = 2;//green
  }
  else if (redColor < 200 /*&& greenColor < 200*/ && blueColor > 250) {
    color = 2;//blue
  }
  else {
    color = 0;//unknown
  }
}
void calibrate() {
  Serial.println("Calibrating...");
  Serial.println("White");//aim sensor at something white
  //set calibration values:
  digitalWrite(13, HIGH);
  delay(2000);
  digitalWrite(S2, LOW);
  digitalWrite(S3, LOW);
  redMin = pulseIn(sensorOut, LOW);
  delay(100);
  digitalWrite(S2, HIGH);
  digitalWrite(S3, HIGH);
  greenMin = pulseIn(sensorOut, LOW);
  delay(100);
  digitalWrite(S2, LOW);
  digitalWrite(S3, HIGH);
  blueMin = pulseIn(sensorOut, LOW);
  delay(100);
  Serial.println("next...");//aim sensor at something black
  digitalWrite(13, LOW);
  delay(2000);
  Serial.println("Black");
  //set calibration values:
  digitalWrite(13, HIGH);
  delay(2000);
  digitalWrite(S2, LOW);
  digitalWrite(S3, LOW);
  redMax = pulseIn(sensorOut, LOW);
  delay(100);
  digitalWrite(S2, HIGH);
  digitalWrite(S3, HIGH);
  greenMax = pulseIn(sensorOut, LOW);
  delay(100);
  digitalWrite(S2, LOW);
  digitalWrite(S3, HIGH);
  blueMax = pulseIn(sensorOut, LOW);
  delay(100);
  Serial.println("Done calibrating.");
  digitalWrite(13, LOW);
}
//void printColor() {//print data
//  Serial.print("R = ");
//  Serial.print(redColor);
//  Serial.print(" G = ");
//  Serial.print(greenColor);
//  Serial.print(" B = ");
//  Serial.print(blueColor);
//  Serial.print(" Color: ");
//  switch (color) {
//    case 1: Serial.println("WHITE");
//      break; I
//    case 2: Serial.println("BLACK"); break;
//    case 3: Serial.println("ORANGE"); break;
//    case 4: Serial.println("YELLOW"); break;
//    case 5: Serial.println("PURPLE"); break;
//    case 6: Serial.println("RED"); break;
//    case 7: Serial.println("GREEN"); break;
//    case 8: Serial.println("BLUE"); break;
//    default: Serial.println("unknown"); break;
//  }
//}
void readColor() {//get data from sensor
  //red:
  digitalWrite(S2, LOW);
  digitalWrite(S3, LOW);
  redFrequency = pulseIn(sensorOut, LOW);
  redColor = map(redFrequency, redMin, redMax, 255, 0);
  delay(100);
  //green:
  digitalWrite(S2, HIGH);
  digitalWrite(S3, HIGH);
  greenFrequency = pulseIn(sensorOut, LOW);
  greenColor = map(greenFrequency, greenMin, greenMax, 255, 0);
  delay(100);
  //blue:
  digitalWrite(S2, LOW);
  digitalWrite(S3, HIGH);
  blueFrequency = pulseIn(sensorOut, LOW);
  blueColor = map(blueFrequency, blueMin, blueMax, 255, 0);
  delay(100);

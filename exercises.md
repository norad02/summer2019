# Exercises
## Ex1: Create an Arduino Account and log into the Arduino Web IDE: 
The IDE can be found [here](https://www.arduino.cc/en/Main/Software)

## Ex2:
- Run the following script for an Arduino Nano on the Arduino Web IDE: 
```
/*
# ***************************************************************************
#   Summer 2019: Ex2
#   ---------------------------------
#   Written by: [ADD YOUR NAME]
#   - for -
#   Mints: Multi-scale Integrated Sensing and Simulation
#   ---------------------------------
#   Date: June 25th, 2019
#   ---------------------------------
#   [SAY SOMETHING ABOUT THE SKETCH]
#   --------------------------------------------------------------------------
#   https://github.com/mi3nts
#   http://utdmints.info/
#  ***************************************************************************
*/

// the setup routine runs once when you press reset:
void setup() {
  // initialize serial communication at 9600 bits per second:
  Serial.begin(9600);
}

// the loop routine runs over and over again forever:
void loop() {
   // print out a String of your choice 
  Serial.println("Hello MINTS: Multi-scale Integrated Sensing and Simulation");
  // Delay for 1000 miliseconds 
  delay(1000);        // delay in between reads for stability
}
```
- Check Whats printed out on the serial monitor: 


## Ex3: Run the following script for an Arduino Nano on the Arduino Web IDE: 
```
/*
# ***************************************************************************
#   Summer 2019: Ex3
#   ---------------------------------
#   Written by: [ADD YOUR NAME]
#   - for -
#   Mints: Multi-scale Integrated Sensing and Simulation
#   ---------------------------------
#   Date: June 25th, 2019
#   ---------------------------------
#   [SAY SOMETHING ABOUT THE SKETCH]
#   --------------------------------------------------------------------------
#   https://github.com/mi3nts
#   http://utdmints.info/
#  ***************************************************************************
*/

void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(LED_BUILTIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(LED_BUILTIN, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);                       // wait for a second
  digitalWrite(LED_BUILTIN, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);                       // wait for a second
}

```

- Change the Delay Times and see how the Arduino responds: 



## Ex4: Download and install Arduino Desktop IDE: 
The IDE can be found [here](https://www.arduino.cc/en/Main/Software)

## Ex5: Do Exercise 2 and 3 on the Desktop IDE

## EX6: Download And Install Atom Desktop IDE
- The Atom Desktop IDE can be found [here](https://atom.io/)

## Ex7: Install PlatformIO within Atom
- This link gives you installation details [here](https://platformio.org/install/ide?install=atom)



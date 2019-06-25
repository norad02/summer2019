# Daily Notes
## June 24, 2019
### Task 1: Disassembling Sensors
#### 1.1 Disassmebling the interior
 - Disconnect USBs and other wires
 - Unscrew the circuit boards from the sensor
 - Take out SD cards
 - Sort out parts into separate piles
#### 1.2 Disassembling exterior
 - Unscrew the lid of the sensor
 - Take out wires
 - Take apart the solar shields
 - Take out the circuit board and the wires

## June 25th, 2019
### Task 2: Try out the Arduino
#### 2.1 Set up Arduino Development Envioronment
 - Go to the web development editor [here](https://www.arduino.cc/en/Main/Software) and create an account
 - Download the Arduino Create plugin [here](https://create.arduino.cc/getting-started/plugin?page=2)
 - Write the following code in the editor:
````/*
# ***************************************************************************
#   Summer 2019: Ex2
#   ---------------------------------
#   Written by: Inbar
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
````

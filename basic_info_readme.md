Basic info - circuit digest

Skip to main content
About | Contact| Advertise

   
Home
User account menu
LOG IN
Search
Search
News 
Articles 
DigiKey Store 
Projects 
Forum
Contact 
More 
Home  Microcontroller Based Projects  Let's Build a Low Cost Drone using ESP32
 Let's Build a Low Cost Drone using ESP32
Published  March 22, 2024  0
Jobit Joseph
Author
DIY WiFi Controlled Drone
DIY WiFi Controlled Drone
Drones have rapidly evolved from niche hobbies to versatile tools with a wide range of applications, from photography to agriculture or even for defense and military purposes. Depending on the application, features, and payload capacity their price will vary from pocket changes to a few million. Even though the basic principle behind these drones may appear simple, there is a lot of technology and computation that goes behind it for proper operations and the desired result from these drones—for example, maintaining stability while in the air requires precision sensors such as a Gyroscope and proper processing of reading taken with it. In this project, we are going to make a drone that can be easily controlled using our phones. If you are an ESP32 fan, you can also check out our other ESP32 projects which we have built earlier. 



This DIY drone is small in size and can be built using easily available components such as ESP32 modules, MPU6050 IMU, coreless motors, and plastic propellers.

Features of our DIY WiFi-Controlled Drone
WiFi controlled: This can be controlled using a smartphone.
MPU6050 IMU for stability control.
All-in-one PCB:  Doesn’t need any 3D printed parts or such.
Easily upgradable: Additional features such as position hold or height hold can be added using external modules.
Small size and lightweight.
Built-in battery charger.
Built-in USB interface for programming and debugging.
Android and IOS apps.
Open source 
Components Required to Build the DIY WiFi-Controlled Drone
The components required to build the DIY WiFi Controlled Drone are listed below. The exact value of each component can be found in the schematics or the BOM.

ESP32 Wroom Module – x1
CP2102N USB - UART controller – x1
MPU6050 IMU – x1
TP4056 Li-ion charger IC – x1
MIC5219-3.3YM5 3.3v LDO – x1
AO3401 P - MOSFET – x1
2N7002DW dual N - MOSFET – x1
SI2302 N - MOSFET – x4
SS34 Diode – x1
1N4148 Diode – x4
SD Card Reader – x1
Type C USB Connector 16Pin – x1
LiPo Battery 1300mAh 30C – x1
720 Coreless Motor – x4
55mm propeller type A(CW) – x2
55mm propeller type B(CCW) – x2
7mm rubber grommet – x4
SMD resistors and capacitors
SMD LEDs
SDM Slide Switch
Connectors
Custom PCB
3D printed Enclosure and mounting screws
Other tools and consumables.
Approximate Build Cost
Here is the approximate build cost for the drone. Keep in mind that the costs provided here are not exact and may vary depending on the vendor. The prices are calculated based on the cheapest options available from reputed vendors, with only the bare minimum number of components as the minimum MOQ.

Capacitor, Resistors, diodes & MISC	 ₹ 100.00
MOSFETS       	 ₹ 40.00
ESP32     	 ₹ 240.00
TP4056	 ₹ 17.50
MPU6050	 ₹ 140.00
CP2102	 ₹ 200.00
720 Motors + 55mm propellers	 ₹ 266.00
PCB    	 ₹ 100.00
Total  	 ₹ 1093.50
DIY WiFi-Controlled Drone Complete Circuit Diagram
The complete circuit diagram for the DIY WiFi Controlled Drone is shown below. It can also be downloaded in PDF format from the link given at the end.

DIY WiFi Controlled Drone Complete Circuit Diagram

Let’s discuss the Schematics section by section for better understanding. A type C USB port is used for both charging as well as programming purposes. The power from the USB port is connected to a power path controller circuit built around a P-Channel MOSFET U2 and a diode D1. When the USB power is available the device will operate from the USB power and also will charge the internal battery, when the USB power is cut, the device will automatically change to battery power. For voltage regulation, we have used a MIC5219 3.3V LDO from Microchip, which is capable of providing up to 500mA of current with a very low dropout voltage of 500mV at full load. A slide switch with a pullup resistor is connected to the enable pin of MIC5219. This switch is used for turning on and off the thermal camera. When this pin is pulled to the ground the LDO will be shut down and hence the other parts of the device too, except the battery charging section. For charging the internal battery we are using a TP4056 charge controller which is capable of a maximum charge current of 1A. For battery voltage sensing we have used a classic voltage divider, which will reduce the battery voltage to a safe level for measurement. 

Voltage Divider Circuit

In the next section, we have the ESP32 SoC itself along with the programming circuit and the MPU6050 PMU chip. The programming circuit consists of a CP2102 USB to UART controller along with a 2N7002DW dual N – N-Channel MOSFET from ON Semi. We have used the CP2102 with the QFN28 package. The tiny dual MOSFET will act as an auto-reset circuit for the ESP32 thus removing any need for manual rest or boot selection circuitry reducing board size and BOM cost. The circuit around the ESP32 is standard, just bypass capacitors and pullup resistors. The MPU6050 PMU is used for flight stabilization and motion control. It is interfaced with the ESP32 SoC using the standard I2C pins GPIO21 and GPIO22.

Programming Circuit

In the last section, we have the motor driver circuit. Each motor driver circuit consists of SI2302 N-Channel MOSFET along with a flyback diode and pulldown resistor. There are 4 such circuits, for each motor. When a high signal is applied to the gate of the driver MOSFET, it will turn on and drive the motor. We will be using PWM signals to control the motor speed. The flyback diode and the capacitors are in place to protect the circuit from any back EMF or voltage spikes.

We have also added three LEDs for debugging purposes apart from the power and charging indicators. The blue LED will blink slowly during sensor calibration and will blink at a fast pace indicating the system is ready for take-off. The green LED will start blinking when a UDP connection is detected from the controller app. The RED LED is used to indicate low battery status, it will lit full-time if the battery voltage is low.

LEDs for Debugging Purposes

PCB for DIY WiFi-Controlled Drone
For this project, we have decided to make a custom PCB. This will ensure that the final product is as compact as possible as well as easy to assemble and use. We have also designed the PCB in a way that the feet for the drone are also included in the PCB and Can be easily broken away from the main PCB. Here are the top and bottom layers of the PCB.

Top & Bottom Layer of PCB

Here is the PCB.

PCB    

PCB 3D view

Here is the 3D render of the fully assembled drone.

Fully Assembled Drone 3D View

And here is the fully assembled drone.

Fully Assembled Drone

The below images show all the important parts marked on the drone. 



Propeller Direction
Install A and B propellers according to the figure below. During the power-on self-test, check if the propellers spin properly and are spinning in the correct direction.

Propeller Direction

Firmware for the DIY WiFi-Controlled Drone

The firmware from our DIY drone is based on the ESP-drone firmware from Espressif. The code is written with ESP-IDF, and the version that is used to compile this code is ESP-IDF 4.4.5. Follow the following link to install and configure ESP32 IDF version 4.4.5. You can either build the firmware from scratch using the source provided at the Github repo below or you can just flash the binary file provided within the same repo if you don’t want the hassle. Make sure to use the source code provided within the below GitHub, because there are many modifications on the Espressif’s code to suit our PCB design.

Flashing the Firmware
To flash the code on our ESP32 Drone, you can follow any one of the below three methods. 

Method 1: Building from Source using ESPIDF
To start with install and configure ESP-IDF. Please follow the detailed instructions from Espressif. Make sure to install version 4.4.x of ESP-IDF.
Once the ESP-IDF is installed clone the ESP-Drone firmware repo using git and navigate to the firmware folder.
git clone https://github.com/Circuit-Digest/ESP-Drone.git

cd ESP-Drone/Firmware/esp-drone
You can change all the firmware configurations using menuconfig. For our use case, the current configuration is enough, and you don’t need to change anything.
idf.py menuconfig
Now to build and flash the firmware you can use the flash command. It will automatically build and flash the project. Replace PORT with your ESP32-S2 board’s serial port name.
idf.py -p PORT flash
Method 2: Using ESPTOOL
To use the ESPtool, make sure you have installed the ESP IDF. You can find the instructions for that in the previous section. Once installed and configured open the terminal in the same folder as the firmware image and use the following command to flash the firmware.

esptool.py write_flash --flash_size detect 0x0 ESPDrone.bin
Method 3: Using ESP32 Flash Download Tool
First download the ESP32 Flash Download Tool
Extract it to a folder and double-click on the exe file to run it. When prompted select ESP32 in the chip type field and click on OK.
Select the firmware file with the ESPDrone.bin file and add the address as 0x00. Select the proper com port and click on erase. Once the flash erase is complete click on START to flash the firmware
Thats it. You are ready to use your DIY drone.
Using The Drone
Place the drone on a flat surface and turn it on. As soon as it’s turned on the flight controller will create a WiFi Hotspot. Connect to it using the password 12345678 and open the app. For the IOS the app can be downloaded from the App Store, just search for ESP-Drone APP. For Android you can download the app from the following link. Keep in mind that the app is created and hosted by a third party. So, install them at your own will. The app interface will look like this. 

App Interface

Click on the connect button to start communicating with the drone. When the connection is established successfully between your drone and the APP, the LED on the drone blinks GREEN. The turn lock button can be used to lock the left controller for just up and down or up, down, left turn and right turn. Use the left stick to tack off or land the drone. Use the right stick to control the movements. If the drone disconnects from the app or the drone itself is rebooting when trying to take off, that means the battery can’t provide enough power. We have used a 1300mAh 30C battery. So please ensure to use a battery with a higher discharge rating.

Preflight Check
Place the drone with its head on the front, and its tail (i.e., the antenna part) at the back.
Place the drone on a level surface and power it up when the drone stays still.
After the communication is established, check if the LED at the drone tail blinks GREEN fast.
A blinking RED LED indicates battery LOW, Charge the battery if it happens.
Slide forward the Trust controller slightly to check if the drone can respond to the command.
Use the right controller to check if the direction control works well.


Supporting Files
You can download all the necessary files from the Circuit Digest GitHub repo, from the following link.



Video


Tags
       
Have any question realated to this Article?
Ask Our Community Members
Log in or register to post comments


Opta Expansions	Opta Expansions
16 programmable inputs for connecting digital sensors and eight more relays to operate machines

3M™ TwinAx High Speed Cable Solutions	3M™ TwinAx High Speed Cable Solutions
3M™ TwinAx High Speed Cable Solutions: Thin, low profile cable with extremely tight bend radii

Black Plated RF Connectors and Adapters	Black Plated RF Connectors and Adapters
Industrial plating option with enhanced durability for a variety of harsh environments

G9KB Series High Power PCB DC Relay	G9KB Series High Power PCB DC Relay
Omron's G9KB series supports high current applications with high capacity load ratings

TechForum	TechForum
Join the conversation. TechForum is a community of experts on all things technical.

ECX-16 32.768 KHz Quartz Crystal	ECX-16 32.768 KHz Quartz Crystal
The ECX-31B tuning fork crystal brings efficiency to mobile, industrial and wireless applications.

RAC10E-K/277 Series AC/DC Converters	RAC10E-K/277 Series AC/DC Converters
RECOM Power’s 10 W AC/DC converters in a 1.8” x 1.0” case are ideal for EV charging applications

RM Series Aluminum Rack Cases	RM Series Aluminum Rack Cases
Hammond’s RM series aluminum cases are suitable for 19” rackmount or desktop/benchtop applications.


Join 100K+ Subscribers
Your email is safe with us, we don’t spam.

Type your email address

Be a part of our ever growing community.



Semicon Media is a unique collection of online media, focused purely on the Electronics Community across the globe. With a perfectly blended team of Engineers and Journalists, we demystify electronics and its related technologies by providing high value content to our readers.

   
COMPANY
Privacy Policy Cookie Policy Terms of Use Contact Us Advertise
PROJECT
555 Timer Circuits Op-amp Circuits Audio Circuits Power Supply Circuits Arduino Projects Raspberry Pi Projects MSP430 Projects STM32 Projects ESP8266 Projects PIC Projects AVR Projects 8051 Projects ESP32 Projects IoT Projects PCB Projects Arduino ESP8266 Projects All Microcontroller Projects
OUR NETWORK

Copyright © 2023 Circuit Digest. All rights reserved.

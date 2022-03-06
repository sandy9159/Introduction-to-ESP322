# Introduction-to-ESP322

![image](https://user-images.githubusercontent.com/19898602/156910057-f5fc1eaf-3b85-4beb-a349-73cc53463579.png)


In this article we are going to talk about ESP32, which I consider to be an older brother of ESP8266. I really like this microcontroller because it has WiFi. 

Just so you have an idea, before ESP exists, if you needed an Arduino to have WiFi, you would have to spend between $ 200 and $ 300 to buy a Wifi adapter. 

The adapter for network cable is not that expensive, but for WiFi it has always been and still is expensive. But fortunately, Espressif Systems has launched ESP and is resolving our lives.

I like ESP32 with this format that has a USB port. This NodeMCU scheme is easy to manipulate because it does not need any electronics. Just plug in the cable, power the device and program it. It works just like an Arduino.

Anyway, today we will talk about the general aspects of ESP32 and how to configure the Arduino IDE to program more devices of the type. Also we will make a program that searches the networks and shows which one is more powerful.

# Key Features

![image](https://user-images.githubusercontent.com/19898602/156910065-6825cd35-a168-41d1-8a26-46dab5b5f01e.png)


Chip with built-in WiFi: standard 802.11 B / G / N, operating in the range of 2.4 to 2.5GHz

Modes of operation: Client, Access Point, Station + Access Point

Dual core microprocessor Tensilica Xtensa 32-bit LX6

Adjustable clock from 80MHz up to 240MHz

Operating voltage: 3.3 VDC

It has SRAM of 512KB

Features 448KB ROM

It has external flash memory of 32Mb (4 megabytes)

Maximum current per pin is 12mA (it is recommended to use 6mA)

It has 36 GPIOs

GPIOs with PWM / I2C and SPI functions

It has Bluetooth v4.2 BR / EDR and BLE (Bluetooth Low Energy)



Before moving fuurther I would like to tell you something about PCB

Yes PCB are the heart of the electronics based project usually we hesitate to try custom PCB and opt to homemade solutions

like breadboard or Zero PCB earlier I also was in the same boat, I hesitate to try custom PCB my belief was they are much expensive.

but then I came to know about [JLCPCB.com](https://jlcpcb.com/IAT) and I was totally surprised how low price PCB's are they offering 
not only PCB [JLCPCB.com](https://jlcpcb.com/IAT) one-stop service from  PCB design and PCB prototype to PCB assembly to PCB enclosures,
SMT Assebly service of [JLCPCB.com](https://jlcpcb.com/IAT) is cherry on top now get your PCB fully assembled and save our time and money
Select components for your PCB from there Parts Library of 200k+ in-stock components (689 Basic components and 200k+ Extended components)
they are offering $27 valued New User coupon  & $24 SMT coupons every month
$8.00 setup fee, and $0.0017  per joint
Now no need to order components for you PCB and get free from stress of soldering them on PCB just experice PCB SMT assembly service and get you PCB ready fro the project


![image](https://user-images.githubusercontent.com/19898602/134224512-bea8d1c8-9ebe-448d-bbba-0cbecb42d528.png)


![image](https://user-images.githubusercontent.com/19898602/130722577-c30b7b43-ea89-4847-9c6b-058f9fabeda3.png)![image](https://user-images.githubusercontent.com/19898602/130722585-b5268db1-5f17-428f-ba60-b823140f2a70.png)


# Comparison Between ESP32, ESP8266 and Arduino R3

![image](https://user-images.githubusercontent.com/19898602/156910070-053c5837-c20f-4e0f-b5da-3825070999e5.png)


# Types of ESP32

![image](https://user-images.githubusercontent.com/19898602/156910080-cf1af390-acce-478c-b255-2bafcfbc27bb.png)


ESP32 was born with a lot of siblings. Today I'm using the first from the left, Espressif, but there are several brands and types, including Oled display built-in. However, the differences are all the same chip: the Tensilica LX6, 2 Core.


# WiFi NodeMCU-32S ESP-WROOM-32

![image](https://user-images.githubusercontent.com/19898602/156910109-8941b3af-299e-4539-b956-fe0b07cbdd4f.png)


This is the diagram of ESP that we are using in our assembly. It is a chip that has lots of appeal and power. They are several pins you choose whether they want to work as digital analog, analog digital or even if that work the door as digital.


# Configuring Arduino IDE (Windows)

![image](https://user-images.githubusercontent.com/19898602/156910118-523f3ec1-5c93-47db-bc7f-f1bc3b8eed16.png)


Here's how to configure the Arduino IDE so we can compile for ESP32:

1. Download the files through the link: https://github.com/espressif/arduino-esp32

2. Unzip the file and copy the contents to the following path:

C: / Users / [YOUR_USER_NAME] / Documents / Arduino / hardware / espressif / esp32

Note: If there is no directory "espressif" and "esp32", just create them normally.

3. Open the directory

C: / Users / [YOUR_USER_NAME] / Documents / Arduino / hardware / espressif / esp32 / tools

Run the file "get.exe".

4. After the "get.exe" finishes, plug the ESP32, wait for the drivers to be installed (or install manually).

Ready, now just pick the ESP32 board in "tools >> board" and compile your code.


# WiFi Scan

Here's an example of how to look for available WiFi networks near the ESP-32, as well as the signal strength of each of them. With each scan, we will also find out which network has the best signal strength.

# Code

First let's include the library "WiFi.h", it will be necessary to allow us to work with the network card of our device.

#include "WiFi.h"
Here are two variables that will be used to store the network's SSID (name) and signal strength.

String networkSSID = "";
int strengthSignal = -9999;

# Setup

In the setup () function, we will define the WiFi behavior mode of our device. In this case, since the goal is to search for available networks, we will configure our device to work as a "station".

~~~
void setup()
{
   // Initialize Serial to log in Serial Monitor
   Serial.begin(115200);

      // configuring the mode of operation of WiFi as station
      WiFi.mode(WIFI_STA);//WIFI_STA is a constant indicating the station mode

      // disconnect from the access point if it is already connected
      WiFi.disconnect(); 
      delay(100);

//    Serial.println("Setup done");
}

~~~

# Loop

In the loop () function, we will search for the available networks and then print the log in the found networks. For each of these networks we will make the comparison to find the one with the highest signal strength.

~~~
void loop()
{
//    Serial.println("scan start");

  // performs the scanning of available networks
    int n = WiFi.scanNetworks();
    Serial.println("Scan performed");

    //check if you have found any network 
    if (n == 0) {
        Serial.println("No network found");
    } else {
        networkSSID = "";
        strengthSignal= -9999;
        Serial.print(n);
        Serial.println(" networks found\n");
        for (int i = 0; i < n; ++i) {
    	  //print on serial monitor each of the networks found
          Serial.print("SSID: ");
          Serial.println(WiFi.SSID(i)); //network name (ssid)
          Serial.print("SIGNAL: ");
          Serial.print(WiFi.RSSI(i)); //signal strength
          Serial.print("\t\tCHANNEL: ");
          Serial.print((int)WiFi.channel(i));
          Serial.print("\t\tMAC: ");
          Serial.print(WiFi.BSSIDstr(i));
          Serial.println("\n\n");
          
 
          if(abs(WiFi.RSSI(i)) < abs(strengthSignal))
          {
             strengthSignal = WiFi.RSSI(i);
             networkSSID = WiFi.SSID(i);
             Serial.print("NETWORK WITH THE BEST SIGNAL FOUND: ( ");
             Serial.print(networkSSID);
             Serial.print(" ) - SIGNAL : ( ");
             Serial.print(strengthSignal );
             Serial.println(" )");
          }                        
         
          delay(10);
        }
    }
    Serial.println("\n------------------------------------------------------------------------------------\n");

// interval of 5 seconds to perform a new scan
    delay(5000);
}

~~~

  "If (abs (WiFi.RSSI (i))"

Note that in the above statement we use abs (), this function takes the absolute value (ie not negative) of the number. In our case we did this to find the smallest of the values in the comparison, because the signal intensity is given as a negative number and the closer to zero the better the signal.









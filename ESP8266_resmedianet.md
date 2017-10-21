# Setup for ESP8266, and connection to Resmedianet

1. Follow the [Sparkfun ESP8266 Thing Development Board Hookup Guide](https://learn.sparkfun.com/tutorials/esp8266-thing-development-board-hookup-guide)
    - [ ] Introduction
    - [ ] Hardware Overview
    - [ ] Hardware Setup
    - [ ] Setting Up Arduino
2. Get your ESP8266's MAC address - [Source](https://techtutorialsx.com/2017/04/09/esp8266-get-mac-address/)
    ```
    #include <ESP8266WiFi.h>

    void setup(){

       Serial.begin(115200);
       delay(500);

       Serial.println();
       Serial.print("MAC: ");
       Serial.println(WiFi.macAddress());

    }

    void loop(){}
    ```   

    - This will return the MAC Address of your ESP8266
        - The MAC should start with 5C:CF:7F
3. Enter the MAC Address on http://netreg.clemson.edu
4. Make sure you set the wifi consts in your code and you should be good to go!
    ```
    const char *ssid = "resmedianet";
    const char *password = "tigerpaw";
    ```

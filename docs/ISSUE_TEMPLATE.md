I am attempting to send data to Adafruit IO useing all of the example sketches in the adafruit IO.

Windows 10 Version 20H2.
I also reproduced this bug in another windows 10 box, and a linux distro.

The esp32 conects tot he wifi, and can be pinged but. It will not connect to the Adafruit IO.

The simplest is the example sketch for "ADAFRUITIO_00_PUBLISH" in the examples in arduino app.

**Connecting to Adafruit IO.Network disconnected.
.Disconnected from Adafruit IO.
.Disconnected from Adafruit IO.
.Disconnected from Adafruit IO.
**

It loops continusly, although normaly it is just a series of pirods.


I have been able to identify the issue to some dagree. If i use the board library version 1.0.4 it conects and works fine with AIO.
Without changeing anything in the code, if I switch to version 1.0.5, or 1.0.6, it no longer connects to AIO, and procedes to loop  for ever in the loop I noted above.

I have located others that is experienceing the same issue also. "https://forums.adafruit.com/viewtopic.php?f=56&t=175113"


----------------------------- Remove above -----------------------------


### Hardware:
|||||||
|:---|---|---|---|---|---|
|<B>Board</B>|ESP32 Dev Module|
|<B>Version/Date</B>|1.0.4|2.0.0|0badbeef|11/jul/2017|today's master|
|<B>IDE name</B>|Arduino IDE|
|<B>Flash Frequency</B>|40Mhz|80Mhz|
|<B>PSRAM enabled</B>|no|
|<B>Upload Speed</B>|115200|
|<B>Computer OS all three:</B>|Windows 10|Mac OSX|Ubuntu|

### Description:

I have been able to identify the issue to some dagree. If i use the board library version 1.0.4 it conects and works fine with AIO.
Without changeing anything in the code, if I switch to version 1.0.5, or 1.0.6, it no longer connects to AIO, and procedes to loop  for ever in the loop I noted above.

### Sketch:  (leave the backquotes for [code formatting](https://help.github.com/articles/creating-and-highlighting-code-blocks/))
```cpp

//Change the code below by your sketch
// Adafruit IO Publish Example
//
// Adafruit invests time and resources providing this open source code.
// Please support Adafruit and open source hardware by purchasing
// products from Adafruit!
//
// Written by Todd Treece for Adafruit Industries
// Copyright (c) 2016 Adafruit Industries
// Licensed under the MIT license.
//
// All text above must be included in any redistribution.

/************************** Configuration ***********************************/

// edit the config.h tab and enter your Adafruit IO credentials
// and any additional configuration needed for WiFi, cellular,
// or ethernet clients.
#include "config.h"

/************************ Example Starts Here *******************************/

// this int will hold the current count for our sketch
int count = 0;

// set up the 'counter' feed
AdafruitIO_Feed *counter = io.feed("counter");

void setup() {

  // start the serial connection
  Serial.begin(115200);

  // wait for serial monitor to open
  while(! Serial);

  Serial.print("Connecting to Adafruit IO");

  // connect to io.adafruit.com
  io.connect();

  // wait for a connection
  while(io.status() < AIO_CONNECTED) {
    Serial.print(".");
    Serial.println(io.statusText());
    delay(3000);
  }

  // we are connected
  Serial.println();
  Serial.println(io.statusText());

}

void loop() {

  // io.run(); is required for all sketches.
  // it should always be present at the top of your loop
  // function. it keeps the client connected to
  // io.adafruit.com, and processes any incoming data.
  io.run();

  // save count to the 'counter' feed on Adafruit IO
  Serial.print("sending -> ");
  Serial.println(count);
  counter->save(count);

  // increment the count by 1
  count++;

  // Adafruit IO is rate limited for publishing, so a delay is required in
  // between feed->save events. In this example, we will wait three seconds
  // (1000 milliseconds == 1 second) during each loop.
  delay(3000);

}
```

### Debug Messages:
```
....................................................................
```

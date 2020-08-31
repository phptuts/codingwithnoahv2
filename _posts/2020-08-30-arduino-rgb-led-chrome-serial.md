---
layout: post
title: How to control  RGB LED with an Arduino directly from the chrome browser
date: 2020-08-30 20:10 -0700
---



This project uses chrome serial, navigator serial to control an rgb led directly from the chrome browser.  In order for this to work you will have to go to chrome://flags and turn on the "Experimental Web Platform features".  This will only work with the chrome web browser.

## Demo Link

[Live Demo](https://phptuts.github.io/rgb-led-chrome-serial/)

## Kit

![Kit](/assets/2020-08-30-arduino-rgb-led-chrome-serial/kit.jpg)

## Build of Materials

- 3 x Resistor between 150 to 1000 Ohms
- 4 x jumper wires
- 1 x breadboard
- 1 x rgb led
- 1 x arduino with usb cable

## Tinkercad

[Tinkercad](https://www.tinkercad.com/things/lpwTE2KHRP7)

<iframe width="725" height="453" src="https://www.tinkercad.com/embed/lpwTE2KHRP7?editbtn=1" frameborder="0" marginwidth="0" marginheight="0" scrolling="no"></iframe>

## RGB LED

The RGB led uses the red, green, and blue color model to create colors.  These colors are mixed together to create various colors. The RGB LED has 4 wires, red, ground, green, and blue.  The ground wire is the longest of the wires.

![rgb led](/assets/2020-08-30-arduino-rgb-led-chrome-serial/rgb_led.jpg)

## Helpful Links

- [Specs for Chrome Navigator Serial](https://wicg.github.io/serial/)
- [C Function Code](https://stackoverflow.com/a/14824108)
- [hex to rgb function](https://stackoverflow.com/a/5624139)

## Build Instructions

1) Insert the rgb led into the breadboard.  

- Red Wire (17, E)
- Ground (-) (15, E)
- Green Wire (13, E)
- Blue Wire (11, E)

![step 1](/assets/2020-08-30-arduino-rgb-led-chrome-serial/rgb_in_breadboard.jpg)

2) Insert the resister into holes (17, D) to (17, B).  This is for the wire controlling the red color.

![step 2](/assets/2020-08-30-arduino-rgb-led-chrome-serial/red_resistor.jpg)

3) Insert a jumper wire into holes (17, B) and pin 3 of the Arduino.

![step 3](/assets/2020-08-30-arduino-rgb-led-chrome-serial/red_wire.jpg)

4) Insert the resister into holes (13, D) to (13, B).  This is for the wire controlling the green color.

![step 4](/assets/2020-08-30-arduino-rgb-led-chrome-serial/green_resistor.jpg)

5) Insert a jumper wire into holes (13, B) and pin 9 of the Arduino.

![step 5](/assets/2020-08-30-arduino-rgb-led-chrome-serial/green_wire.jpg)

6) Insert the resister into holes (11, D) to (11, B).  This is for the wire controlling the blue color.

![step 6](/assets/2020-08-30-arduino-rgb-led-chrome-serial/blue_resistor.jpg)

7) Insert a jumper wire into holes (11, B) and pin 10 of the Arduino.

![step 7](/assets/2020-08-30-arduino-rgb-led-chrome-serial/blue_wire.jpg)

8) Insert a jumper wire into holes (15, A) and ground of the arduino.

![step 8](/assets/2020-08-30-arduino-rgb-led-chrome-serial/gnd_wire.jpg)

It should look like this when you are done.

![final wiring](/assets/2020-08-30-arduino-rgb-led-chrome-serial/final_wires.jpg)

## Side Notes

- It would be nice to display whether we are connected or not.

## Code

[github repo](https://github.com/phptuts/rgb-led-chrome-serial)


### Arduino Code
```c
#define RED_PIN 3
#define GREEN_PIN 9
#define BLUE_PIN 10

void setup() {
  // put your setup code here, to run once:
  pinMode(RED_PIN, OUTPUT);
  pinMode(GREEN_PIN, OUTPUT);
  pinMode(BLUE_PIN, OUTPUT);
  Serial.begin(115200); // Number of bits the arduino can receive per second 115200 bits 
  Serial.setTimeout(10); // This is how long the arduino will wait for a message 10 milliseconds
}

void changeColor(int red, int green, int blue) {
  analogWrite(RED_PIN, red);
  analogWrite(GREEN_PIN, green);
  analogWrite(BLUE_PIN, blue);
}

String getValue(String data, char separator, int index)
{
  int found = 0;
  int strIndex[] = {0, -1};
  int maxIndex = data.length()-1;

  for(int i=0; i<=maxIndex && found<=index; i++){
    if(data.charAt(i)==separator || i==maxIndex){
        found++;
        strIndex[0] = strIndex[1]+1;
        strIndex[1] = (i == maxIndex) ? i+1 : i;
    }
  }

  return found>index ? data.substring(strIndex[0], strIndex[1]) : "";
}

void loop() {
  
  String computerText = Serial.readStringUntil('@');
  computerText.trim();
  if (computerText.length() == 0) {
    return;
  }
  // 92-0-130
  int redColor = getValue(computerText, '-',0).toInt();
  int greenColor = getValue(computerText, '-',1).toInt();
  int blueColor = getValue(computerText, '-', 2).toInt();
  changeColor(redColor, greenColor, blueColor);
  Serial.println("WORKING");
  delay(1000);
//  changeColor(179, 56, 181);
//  delay(2000);
//  changeColor(0,255,0);
//  delay(2000);
//  changeColor(0, 0,0);
//  delay(2000);
}


```

### Website Code


```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link
      href="https://fonts.googleapis.com/css2?family=Bebas+Neue&display=swap"
      rel="stylesheet"
    />
    <link
      rel="stylesheet"
      href="https://stackpath.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css"
      integrity="sha384-wvfXpqpZZVQGK6TAh5PVlGOfQNHSoD2xbE+QkPxCAFlNEevoEH3Sl0sibVcOQVnN"
      crossorigin="anonymous"
    />

    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>RGB LED Website</title>
    <style>
      body {
        font-family: "Bebas Neue", cursive;
      }
      h1 {
        font-size: 40px;
        text-align: center;
      }
      input {
        display: block;
        margin: 20px auto;
      }
      section {
        text-align: center;
      }
    </style>
  </head>
  <body>
    <h1>RGB LED Website</h1>
    <input type="color" id="color-picker" />
    <section id="controls">
      <button onclick="onConnectUsb()" id="connect-usb">
        <i class="fa fa-usb" aria-hidden="true"></i>
      </button>
      <button onclick="onChangeColor()" id="change-color">Change Color</button>
    </section>
    <script>
      let isConnectted = false;
      let port;
      let writer;
      const enc = new TextEncoder();

      async function onChangeColor() {
        if (!isConnectted) {
          alert("you must connect to the usb in order to use this.");
          return;
        }
        try {
          const colorHex = document.getElementById("color-picker").value;
          const colorRgb = hexToRgb(colorHex);
          const computerText = `${colorRgb.r}-${colorRgb.g}-${colorRgb.b}@`;
          await writer.write(enc.encode(computerText));
        } catch (e) {
          console.log(e);
          alert("could not write color");
        }
      }

      async function onConnectUsb() {
        try {
          const requestOptions = {
            // Filter on devices with the Arduino USB vendor ID.
            filters: [{ usbVendorId: 0x2341 }],
          };

          // Request an Arduino from the user.
          port = await navigator.serial.requestPort(requestOptions);
          await port.open({ baudrate: 115200 });
          writer = port.writable.getWriter();
          isConnectted = true;
        } catch (e) {
          console.log("err", e);
        }
      }

      function hexToRgb(hex) {
        var result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
        return result
          ? {
              r: parseInt(result[1], 16),
              g: parseInt(result[2], 16),
              b: parseInt(result[3], 16),
            }
          : null;
      }
    </script>
  </body>
</html>
```
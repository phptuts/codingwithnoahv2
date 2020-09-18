---
layout: post
title: Arduino Chrome Serial with Led Matrix
date: 2020-09-16 21:31 -0700
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/XsAFvrEJv7U" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This project teaches you how to wire and code an led matrix that can talk directly to the chrome browser.  It uses an experimental api called navigator serial.

[Demo Link](https://phptuts.github.io/led-matrix-led-yt/)

[Code](https://github.com/phptuts/led-matrix-led-yt)

## Kit

![Kit](/assets/2020-09-16-arduino-led-matrix-chrome-serial/kit.jpg)

## Build of Materials

- Arduino Uno
- Led Matrix
- 5 jumper wires
- breadboard

## Wires

![Wiring](/assets/2020-09-16-arduino-led-matrix-chrome-serial/wiring.png)

Before the wiring.

![step 0](/assets/2020-09-16-arduino-led-matrix-chrome-serial/step0.jpg)

1. Connect DIN Pin to Pin 12 of the Arduino.

![step 1](/assets/2020-09-16-arduino-led-matrix-chrome-serial/step1.jpg)

2. Connect CS Pin to Pin 10.

![step 2](/assets/2020-09-16-arduino-led-matrix-chrome-serial/step2.jpg)

3. Connect CLK Pin to Pin 11.

![step 3](/assets/2020-09-16-arduino-led-matrix-chrome-serial/step3.jpg)

4. Connect GND to Arduino GND.

![step 4](/assets/2020-09-16-arduino-led-matrix-chrome-serial/step4.jpg)

5. Connect VCC to Arduino V5.

![step 5](/assets/2020-09-16-arduino-led-matrix-chrome-serial/step5.jpg)


## Svelte Javascript Code

```javascript

<script>
  import _ from "lodash";
  let port;
  let writer;
  let status = "disconnected";
  let leds = _.range(0, 8).map(() => _.range(0, 8).map(() => false));

  const textEncoder = new TextEncoder();

  function onToggleLed(row, column) {
    leds[row][column] = !leds[row][column];
    leds = [...leds];
  }
  $: arduinoMessage =
    [...leds]
      .reverse()
      .reduce((acc, value) => {
        return acc + value.map((l) => (l ? "1" : "0")).join(":") + ":";
      }, "")
      .slice(0, -1) + "@";

  async function connectUsb() {
    if (!navigator.serial) {
      alert("Turn on experimential web features to connect to the Arduino.");
      return;
    }

    // if it's connected we want to return
    if (status === "connected") {
      return;
    }

    try {
      port = await navigator.serial.requestPort({
        filters: [{ usbVendorId: 0x2341 }],
      });
      await port.open({ baudrate: 115200 });
      writer = port.writable.getWriter();
      status = "connected";
    } catch (e) {
      console.log(e);
    }
  }

  $: if (status == "connected" && arduinoMessage != "") {
    writer.write(textEncoder.encode(arduinoMessage));
  }
</script>

<style>
  main {
    width: 500px;
    height: 500px;
    margin: 10px auto;
    border: solid gray 1px;
    background-color: #535353;
  }
  section.row {
    display: flex;
  }
  div.circle {
    width: 50px;
    height: 50px;
    border-radius: 50%;
    margin: 5px;
    border: solid gray 1px;
    background-color: #fff;
    cursor: pointer;
  }
  div.on {
    background-color: #aa0000;
  }
  h1,
  h2 {
    text-align: center;
  }
  section#message {
    width: 500px;
    word-wrap: break-word;
    border: solid gray 1px;
    padding: 5px;
    margin: 10px auto;
  }
  #usb-connection-info {
    width: 500px;
    margin: 5px auto;
    text-align: center;
  }
  #usb-connection-info h3 {
    letter-spacing: 2px;
  }
  #usb-connection-info button {
    border: none;
    padding: 10px;
    outline: none;
    font-size: 16px;
    cursor: pointer;
  }
  #usb-connection-info button:disabled {
    cursor: no-drop;
  }
  button:active,
  button:focus {
    outline: none;
  }
</style>

<h1>Led Matrix Arduino</h1>

<section id="usb-connection-info">
  <button
    disabled={status === 'connected'}
    on:click={connectUsb}>Connect</button>
  <h3>Status: {status}</h3>
</section>

<main>
  {#each leds as row, rowIndex}
    <section class="row">
      {#each row as led, columnIndex}
        <div
          class:on={led}
          on:click={() => onToggleLed(rowIndex, columnIndex)}
          class="circle" />
      {/each}
    </section>
  {/each}
</main>

<h2>Arduino Message</h2>

<section id="message">{arduinoMessage}</section>


```


## Arduino Code

```c++
#include "LedControl.h"

// 12 is for the data pin
// 11 is for the CLK pin
// 10 is for the CS pin
LedControl matrix = LedControl(12, 11, 10, 1);

String getValue(String data, char separator, int index)
{
    int found = 0;
    int strIndex[] = { 0, -1 };
    int maxIndex = data.length() - 1;

    for (int i = 0; i <= maxIndex && found <= index; i++) {
        if (data.charAt(i) == separator || i == maxIndex) {
            found++;
            strIndex[0] = strIndex[1] + 1;
            strIndex[1] = (i == maxIndex) ? i+1 : i;
        }
    }
    return found > index ? data.substring(strIndex[0], strIndex[1]) : "";
}

void setup() {
  // put your setup code here, to run once:
  matrix.shutdown(0, false); // wake up the led matrix
  matrix.setIntensity(0, 8); // sets the brigtness of the leds
  matrix.clearDisplay(0); // clear everything
  Serial.begin(115200); // baud rate which is the number of zeros and one through the usb per second
  Serial.setTimeout(10); // how long in milliseconds the arduino will wait for a message
}

void loop() {
  String message = Serial.readStringUntil('@');
  message.trim(); 

  if (message.length() == 0) {
    return;
  }

  for (int row = 0; row < 8; row += 1) {
    for (int col = 0; col < 8; col += 1) {
       bool isOn = getValue(message, ':',row * 8 + col) == "1";
       matrix.setLed(0, col, row, isOn);
    }
  }
  
}

```

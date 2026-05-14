---
title: "Basketball match control console for live streaming"
author: "llex"
description: "This project is a physical hardware control surface designed to manage live basketball match broadcasting. It allows an operator to control match scoring, player actions (fouls, free throws, points), and OBS scene switching from a single dedicated console."
created_at: "2026-05-11"
---

# May 11
Reflection about technologie and objectives:
3 big part:
- left: management of players and stats
- middle: fast action of match
- right: cameras / obs management

In my imagination:
3 big part = 3 ESP for manage 3 parts
1 raspberry or PC for manage all ESP flux by LAN (Maybe WiFi) and connected to a obs PC
lot of button for managing 
2 potentiometer for settings / players selection
And a switch for link all cameras and raspberry in ethernet to the PC obs for stream
(And lot of little component like diode ...)

For the button one of my idea is make a grid x, y axis ?

![image.png](https://cdn.hackclub.com/019e188a-8019-706e-8271-b708721e3d73/image.png)

**Total time spent: 2 hours**

# May 12
This day, I've make reflection about lot of technical thinks:
For connexion:
3 sides:
-> for each sides:
  ESP-WROOM-32 --> Wifi --> Raspberry --> Switch --> OBS PC

for 3 ethernet cameras (example):
-> ETHERNET CAMS
  CAMERAS --> ethernet port on table --> switch --> OBS PC

At starting I thought connnect all think with cable

The switch make a LAN with all cameras / the plate.

At the base of project I think all cameras pass by the raspberry
but with reflexion: 
Cameras does not pass by raspberry for high quality !

I've also start to make reflexion on the feature I would like.
![img journal](https://user-cdn.hackclub-assets.com/019e1da1-a788-7cef-81ce-c3945b9f5259/2-6.png)
**Total time spent: 2 hours**

# May 13
I've start to design my plate on Fusion 360, one on my first time with this software 😅,
I've search many time for cut a block:
![alt text](https://cdn.hackclub.com/019e2249-eb82-77e3-ad25-72129cd5a435/2-2.png)


Here is the partial block:
![images.png](https://cdn.hackclub.com/019e224a-0a14-7609-9f6e-e43724dbb5d6/2-1.png)

Next day i will make the electronic scheme
For the moment, I wait for the electronic sheme for make round

**Total time spent: 2 hours**

# May 14
I've start designing electronic on TinkerCad with button gestion and simulate Arduino card.

Is the first time with this software and I try some other system for the buttons, the choiced placement is Grid i've follow this tutorial: [Yt instruction](https://www.youtube.com/watch?v=dop0SoD2XL8&t=123s)

![tinkercad](https://cdn.hackclub.com/019e2783-9cb5-7c2e-8249-07d5b66bad78/2-3.png)

This scheme is sheme of one button grid with ESP (one of three)

I've start the code:
![2-4](https://cdn.hackclub.com/019e2783-aceb-7973-8018-3fd905d7773c/2-4.png)
With this code it's globally work but the left side of button dosent active ?
See the screenshot: ![2-5](https://cdn.hackclub.com/019e2783-b714-7ad4-ae0a-b462d6b81fe1/2-5.png)
I search why this dont work

The code:
```
const int rowPins[6] = {13, 12, 11, 10, 9, 8};
const int colPins[6] = {7, 6, 5, 4, 3, 2};

const int ROWS = 6;
const int COLS = 5;

void setup() {
  Serial.begin(9600);

  for (int r = 0; r < ROWS; r++) {
    pinMode(rowPins[r], OUTPUT);
    digitalWrite(rowPins[r], HIGH);
  }

  for (int c = 0; c < COLS; c++) {
    pinMode(colPins[c], INPUT_PULLUP);
  }
}

void loop() {

  for (int r = 0; r < ROWS; r++) {

    digitalWrite(rowPins[r], LOW);

    for (int c = 0; c < COLS; c++) {

      if (digitalRead(colPins[c]) == LOW) {
        Serial.println(r);
        Serial.println(c);

        delay(200);
      }
    }

    digitalWrite(rowPins[r], HIGH);
  }
}
```

**Total time spent: 3 hours**
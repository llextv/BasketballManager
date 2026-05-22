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

# May 15
Today my works are to list electric component and make a scheme of electrical device and specs.
This scheme: ![scheme](https://cdn.hackclub.com/019e2b69-08ad-7e8b-99f9-04b1df589936/2-6.png)

I have calculate max voltage of all my components:
ESP-WROOM-32 -> 5V 0.25A max
SWITCH TP-Link LS1005G -> 5V 0.5A max
RASPBERRY -> 5V 3A max

Cams & PC OBS are not managed by my system

For calculate:
Moy using: 1.42A
Max using: 4.25A (take 5A for security)

For all -> 5V

Power: 
P = U * I
with U = 5V and I= 5A
P = 25W

For the power system:

https://www.amazon.fr/Tesfish-Alimentation-adaptateur-Transformateurs-lumineuse/dp/B083QN65BB

https://fr.aliexpress.com/item/1005010339166210.html

https://fr.aliexpress.com/item/1005005545792891.html

Normally all connexion are secured from joult effect.

About this subject:
I will install fan for refresh interior of box (I need to prevent it from overheating and melting my case.)

**Total time spent: 2 hours**

# May 16

For today i've make reflection about button alignement, choiced button place ...
This is scheme: 
![design btn](https://cdn.hackclub.com/019e30d2-e360-7274-957a-6bd32f1393c8/2-7.png)

I've remake this about 3 times for decide the best place for each button.

After reflexion I think button with LED is better for On / Off not for all but for 80% min.

This is a plan for 3 block of button for 1 ESP
![design btn place](https://cdn.hackclub.com/019e30d2-eb4c-7981-a84e-55e5aaabe9b3/2-8.png)

Button need LED Btn are place with marker:
![marker led btn](https://cdn.hackclub.com/019e30d2-f484-72de-8261-d1dfbefc23c2/2-9.png)

Next step is to make row and column and make the electronic sheme totaly.
But before i need to investigate about button LED.

**Total time spent: 2 hours**

# May 17
Objective of the day:
Check for the LED button and edit electrical scheme:
![img elec](https://cdn.hackclub.com/019e3612-5bf6-79d5-af5e-ebb1eed4b3a9/2-10.png)
this is first version of this electric scheme
I work a lot on this elec scheme for security, I actually make reflection about put a security fusible for secure my circuit ? This is overkill for my system ?
I send message on forge-help channel on slack for judge my circuit, I wait response
This scheme is for the moment only for connection with cable but a PCB may be created after

**Total time spent: 2 hours**

# May 18
Okay today big work !
Make the entire scheme electronic on TinkerCad for each button and LED
Partialy maked for the moment:
![tk_elect](https://cdn.hackclub.com/019e3c89-326a-7dd9-bc5c-9473dd1a0123/2-11.png)
I actually work on but its terrible long to make, I try to place all button perfectly for easy develop after.
And cable management for more beautiful scheme to recongnising easely

I have actually place button like this scheme: 
![design btn](https://cdn.hackclub.com/019e30d2-e360-7274-957a-6bd32f1393c8/2-7.png)

All button are followed by diode for escaping Ghosting risk on the grid

My fear is to nope have enough PIN on ESP but it's Ok, ESP have more PIN than Arduino (i cannot import ESP in tinkercad so I make this with Arduino for the moment)
This scheme in the future its PCB

Images from my conception:
![2-tk](https://cdn.hackclub.com/019e3c89-7132-71f6-9ee1-06f5dd8fd707/2-12.png)

During wirding cable I also think how communicate ESP <--> RASPBERRY, I have alrealy make reflection about connexion system but never to how communication ? 
So with reflection here is plan:

RASPBERRY make a local web server
Local server of raspberry have backend road: POST /btn/push/:cardId/:id
cardId: ID of ESP Card
id: ID of btn pressed

![complete scheme](https://cdn.hackclub.com/019e3c89-96f3-78f2-91b2-42946a30c3a5/2-13.png)
This is complete scheme of the project:
![alt text](https://cdn.hackclub.com/019e3c89-ba7e-7eeb-b92c-4b80745f8d3b/2-14.png)
![alt text](https://cdn.hackclub.com/019e3c89-ea3c-7f06-bcf0-b1a77e58de6c/2-15.png)
![alt text](https://cdn.hackclub.com/019e3c8a-1b75-7fef-a7b7-2b9edd8eda9e/2-16.png)
![alt text](https://cdn.hackclub.com/019e3c8a-4e65-7deb-b366-12b3695027d5/2-17.png)
![alt text](https://cdn.hackclub.com/019e3c8a-57aa-7017-8f72-4ffa3e2c5813/2-18.png)
![alt text](https://cdn.hackclub.com/019e3c8a-603a-7723-9766-9d6437c1a1d5/2-19.png)

**Total time spent: 5 hours**

# May 19
Reflection about button and KeyCaps
Button I have choiced before are not good for my usage
So I start to check other button not so expensive, I find this:
https://fr.aliexpress.com/item/1005007871108640.html
need this:
https://fr.aliexpress.com/item/1005012163356976.html

and:
https://fr.aliexpress.com/item/1005006977105041.html

x80 for each
for:
- 18,80€
- 1,10€
- 3,31€
= 23,21€ with no transport !

For encoder:
- https://fr.aliexpress.com/item/1005007644083514.html (x3, Plum Handle 20mm)
= 1,82€

For the keycaps I will try to make impression with my 3d printer and see ...

I append to make rectangular network on fusion, it's difficult at starting but after little working with it's pretty simple
![network](https://cdn.hackclub.com/019e4133-01c1-7acd-8d55-53007fb23e3a/2-20.png)
Its could be big plate for little button but if it's so big i will reduce size of plate but keep white place for future plugins for my plate

**Total time spent: 2 hours**

# May 20
I start to place button with scheme.
First part like this
![hole](https://cdn.hackclub.com/019e4652-015f-7a17-aba0-73e1c1cee66f/2-21.png)
Hole for encoder:
![tk_hle](https://cdn.hackclub.com/019e4652-094d-7080-a076-d057e02cc91a/2-22.png)

Finish it but realy big, I will reduce little box
![allbtn](https://cdn.hackclub.com/019e4652-1286-73a7-a4b8-096cf2c93593/2-23.png)

This is final version reduced
![finalVersion](https://cdn.hackclub.com/019e4652-1b3b-713c-8e66-f34abc805701/2-24.png)

Place white for place of plugins and cable under the plate
I'have edit a little bit the scheme
**Total time spent: 4 hours**

# May 21
List all items for the plate:

- Button (x80) - 18,80€
- Diode (x80) - 1,10€
- LED (x80) - 3,31€
- Encoder (x3) - 1,82€
- ESP (x3) - 24€
- Adaptateur 40W (x1) - 22€
- DC 12V Male Female Connectors (x2) - 1.59€
- Positions Terminal Block Connector (x10) - 1.22€
- Resistor 330R - 0,58€
- Condensator - 2,00€
- Type C/Micro Female To 2 Pin Terminal Block - 2,20€ (https://www.aliexpress.com/ssr/300001493/welcomegiftspmpc?spm=a2g0o.productlist.main.1.328aodEAodEATE&businessCode=ug&productIds=1005011840570287&pha_manifest=ssr&source_position=app_home_moretolove_wg&_immersiveMode=true&disableNav=YES&sourceName=SEARCH&skuId=12000056741367887&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005011840570287%7C_p_origin_prod%3A&pvid=e60bdde1-0d69-4edb-aa87-0376538a3268)
- Solderless Plug USB C to 2Pin Terminal Adapter Screw Type-C to 2 Pin Crimp Terminal  - 1.46€ (https://fr.aliexpress.com/item/1005011551227977.html?spm=a2g0o.productlist.main.2.6f9fEwZwEwZwhs&algo_pvid=ced022b4-4863-45ec-95d5-b4bee4b0d98c&algo_exp_id=ced022b4-4863-45ec-95d5-b4bee4b0d98c-1&pdp_ext_f=%7B%22order%22%3A%2267%22%2C%22eval%22%3A%221%22%2C%22fromPage%22%3A%22search%22%7D&pdp_npi=6%40dis%21EUR%211.45%210.99%21%21%2111.20%217.63%21%40211b653717789425176881157e2473%2112000055894642923%21sea%21FR%216194915421%21ABX%211%210%21n_tag%3A-29910%3Bd%3Adcc5aa5c%3Bm03_new_user%3A-29895%3BpisId%3A5000000204883101&curPageLogUid=fJAmveAfPLnE&utparam-url=scene%3Asearch%7Cquery_from%3A%7Cx_object_id%3A1005011551227977%7C_p_origin_prod%3A)
-  Raspberry Pi 4 Modèle B (1 Go) - 50€

Other fee: 
- PCB - Unkown €
- Casing - Unkown €
- KeyCaps - Unkown €

= 130,08 € = 151,15 $

Next step is to design PCB / get $ of PCB creation and design keycaps + casing for estimation of cost

![img shop](https://cdn.hackclub.com/019e4ba1-0019-7dec-a449-11d44e8f2907/2-25.png)
**Total time spent: 2 hours**

# May 22
Today, I'have principly lost my time
Try to design PCB on KiCad but the size of keys ... are not good, after I try with Fusion PCB, but nothing too

No software are able to generate PCB stl for import after in Fusion for test ??

![alt text](https://cdn.hackclub.com/019e4ff0-ddef-77be-a62c-0e21b758dc02/2-26.png)

**Total time spent: 1 hours**
<h1>Functionality </h1>
This fork lets you control your logicdata standing desk via ESPHome/with an ESP32. It offers function for up, down, memory position 1 and memory position 2. 
Notice that saving of memory positions is only possible if you have a handset that ofers a save button. 

Additionally you find my lovelace card configuration to control your desk with a small tile if needed:
![lovelace-tile-card-standingdesk](https://github.com/RoMaTiX99/esphome-standing-desk/blob/master/lovelace-tile-card-standingdesk.png)

<h1>Motivation and history </h1>
This repository is a fork of [esphome-standing-desk](https://github.com/tjhorner/esphome-standing-desk) by @tjhorner.
At the same time it merges the adoptions [DanielHabenicht / logicdata-standing-desk](https://github.com/DanielHabenicht/logicdata-standing-desk/tree/main) to have functionality for Logicdata desk - but ended up not using it at the moment (read below). 

This fork/merger was created to make it compatible with latest ESPHome Version. 
Within further deep dive into the topic I had to experience, that the configuration by @DanielHabenicht wouldn't function properly on my desk: 
- current height was not measured (sensor had no value)
- up button moved desk up, but not to the highest level
- save button wouldn't save any position, but instead moved the desk up to it's maximum level
- memory position 4 was without any function

So I thought I messed up with the wiring and re-measured the voltage for the 7 Pins on the connector and so on. But everything was correct. Then I dived deeper into to provided records from SeaLogicAnalyzer but I ended up realizing that if somethings messed up I wouldn't be able to find the correct way with the given information. 

So I started to give 5 Volts to the pins manually and noticed, that you can address the basic functions of the logicdata control unit quite simple. For me this is: 
Up, Down, Memory1 to Memory5 and even save. Personally I don't need more than this. 

In the future I will probably work to get the "current height" sensor working in Order to send a specific height. 

<h1>Setup </h1>
First of all the Layout of the 7-Pin-Din connector:

![7-Pin-Din connector](https://github.com/RoMaTiX99/esphome-standing-desk/blob/master/LOGICDATA_7-PIN_Connector_Handset.png)


Here's what worked for me: 
| Line  | Description                                  | Colours          | ESP32 Pin |
| ----- | -------------------------------------------- | ---------------- |-----------|
| SHELL | Ground                                       | Grey             | GND       |
| 1     | up                                           | white            | GPIO16    |
| 5     | Memory Position 1                            | blue             | GPIO17    |
| 2     | Memory Position 2 (Position 3 on handset)    | green            | GPIO18    |
| 4     | down                                         | yellow           | GPIO19    |
| 6     | (not sure if needed)                         | red              | GPIO21    |


So the following Pin / cable doesn't need to be connected for basic functionality (up/down/mem1/mem2/mem3/mem4/save):
| Line  | Description                                  | Colours          | ESP32 Pin |
| ----- | -------------------------------------------- | ---------------- |-----------|
| 3     |                                              | brown            | -         |
| 7     | 5V Powersupply                               | black            | VIN / +5V |

According to my investigations adressing the functions works as follows: 
| Function  | Bit1 | Bit2 | Bit3 | Bit4 |
| --------- | -----|------|------|------|
| Memory1   | 0    | 1    | 0    | 0    |
| Memory2   | 0    | 0    | 1    | 0    |
| Memory3   | 0    | 0    | 1    | 1    |
| Memory4   | 0    | 1    | 0    | 1    |
| Up        | 1    | 0    | 0    | 0    |
| Down      | 0    | 0    | 0    | 1    |
| Save      | 1    | 0    | 0    | 1    |

I used 7-Pin-DIN Y-Splitter-Cable from here: [Aliexpress](https://de.aliexpress.com/item/1005003269764721.html?spm=a2g0o.order_list.order_list_main.5.61e95c5fDRpzA4&gatewayAdapt=glo2deu).
I ordered two of them and cut one on the female end and connected the wires to and ESP32. 


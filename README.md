# TigerGameComScreen
A project to reverse engineer the screen design of a Tiger GameCom in order to replace the older broken screen with a newer one using a microcontroller

There are 11 pins from the main board on the Tiger Game.com that connect to the LCD Dot Matrix Display. Using a multimeter when the console is powered on, I found that the first pin on the left was ground, and the rest of the pins were non-zero amounts of volts that were fluctuating in the mV ranges. The last pin had a voltage of 4.7V, which I decided was the input voltage for the display. This leaves 9 unknown pins. Using a DSLogics analyzer I captured the following data. 
![image](https://github.com/cscrosati98/TigerGameComScreen/assets/93940260/6f4fa2d5-d2a1-4414-8648-04f98568d446)

Using this data in conjunction with the following pinout from the SM8521 manual, I started to pick where I thought pins would be located. Data Line 0 is actually Pin 1 on the main board.

LCD Display Pins on SM8521
![image](https://github.com/cscrosati98/TigerGameComScreen/assets/93940260/d20d62a9-562d-451f-8ffc-4d3bf80ea554)

https://jp.sharp/ic/datasheet/micon/pdf/sm8521.pdf
page 5<br/>

| PIN | NAME | Protocol | Freq | Usage |
|---|---|---|---|---|
|0|GND|DC||
|1|LP||75.52Hz|
|2|FR|||
|3|XC|Shift Reg|Clock for Parallel Shift register?|
|4|XD0|Shift Reg|Data for Parallel Shift register?|
|5|XD1|Shift Reg|Data for Parallel Shift register?|
|6|XD2|Shift Reg|Data for Parallel Shift register?|
|7|XD3|Shift Reg|Data for Parallel Shift register?|
|8|DOFFB?||Held High|
|9|LP?||4x15.62Hz/629.39Hz|
|10|YD?||37.7Hz/25Hz|Possibly Refresh Rate?
|11|5V+|DC||
<br/>
Noting that the most inconsistent data was coming through Lines 2-5, I assumed those were XD0-XD3. Line 7 showed 7 pulses every 1.6ms, then 6 pulses, alternating. This leads me to believe this is the display latch pulse. Line 6 was held high the entire time, which I assume is the DOFFB, which may be active low. 

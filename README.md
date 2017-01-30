# WakeUpDarling-
Arduino + Coffee Machine Alarm

First prototype:

![simwud-2](https://cloud.githubusercontent.com/assets/22894897/20111199/a6c945e0-a5a4-11e6-84fa-bf9d606cb45e.gif)

The light bulb represents the Coffee Machine

PARTS I USED:

Arduino Uno

LCD 16x2

Wires

Resistors (220Ω, 1kΩ)

Potentiometer

Coffee Machine (light bulb in the simulator)

Diode (1N4001)

Transistor NPN (2222A)

Relay SPDT 5VDC 

Soldering iron

Real Time Module for arduino

Grounding Converters (2) like the one in the picture

![31caoriq80l _sy300_](https://cloud.githubusercontent.com/assets/22894897/20027208/738834a8-a2d3-11e6-9fa4-d4ee56378a8d.jpg)

Once I had everything, I wired the coffee machine as the light bulb in the simulator.  

Schematics View:

<img width="863" alt="screen shot 2016-11-08 at 8 56 51 am" src="https://cloud.githubusercontent.com/assets/22894897/20106175/78fda4d4-a591-11e6-80ca-00de4065ab49.png">

The Real Time module was wired as following:

![fritzing-rtc](https://cloud.githubusercontent.com/assets/22894897/22409110/1bd07cc2-e642-11e6-8a67-61d256fa60cf.png)

Code:

<pre>
<font color="#434f54">// WakeUpDarling!</font>
<font color="#434f54">// Dedicated to my wife.</font>
<font color="#434f54">// Made by Tania Molina</font>
<font color="#434f54">// License: MIT License</font>
<font color="#434f54">// Downloaded from: </font><u><font color="#434f54">https://circuits.io/circuits/3126865-wakeupdarling</font></u><font color="#434f54"></font>

<font color="#5e6d03">#include</font> <font color="#434f54">&lt;</font><b><font color="#d35400">DS3231</font></b><font color="#434f54">.</font><font color="#000000">h</font><font color="#434f54">&gt;</font> <font color="#434f54">//download if you don't have it yet.</font>
<font color="#5e6d03">#include</font> <font color="#434f54">&lt;</font><b><font color="#d35400">LiquidCrystal</font></b><font color="#434f54">.</font><font color="#000000">h</font><font color="#434f54">&gt;</font>

<b><font color="#d35400">DS3231</font></b> &nbsp;<font color="#000000">rtc</font><font color="#000000">(</font><font color="#00979c">SDA</font><font color="#434f54">,</font> <font color="#00979c">SCL</font><font color="#000000">)</font><font color="#000000">;</font>
<b><font color="#d35400">Time</font></b> <font color="#000000">t</font><font color="#000000">;</font>
<b><font color="#d35400">LiquidCrystal</font></b> <font color="#000000">lcd</font><font color="#000000">(</font><font color="#000000">7</font><font color="#434f54">,</font> <font color="#000000">6</font><font color="#434f54">,</font> <font color="#000000">5</font><font color="#434f54">,</font> <font color="#000000">4</font><font color="#434f54">,</font> <font color="#000000">3</font><font color="#434f54">,</font> <font color="#000000">2</font><font color="#000000">)</font><font color="#000000">;</font>


<font color="#00979c">void</font> <font color="#5e6d03">setup</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<font color="#434f54">// Setup Serial connection</font>
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#000000">115200</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">9</font><font color="#434f54">,</font> <font color="#00979c">OUTPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">9</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">rtc</font><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>

 &nbsp;<font color="#000000">rtc</font><font color="#434f54">.</font><font color="#d35400">setTime</font><font color="#000000">(</font><font color="#000000">6</font><font color="#434f54">,</font> <font color="#000000">59</font><font color="#434f54">,</font> <font color="#000000">50</font><font color="#000000">)</font><font color="#000000">;</font> &nbsp;&nbsp;&nbsp;&nbsp;<font color="#434f54">// Set the real time for the module </font>
 &nbsp;<font color="#434f54">//&lt;hour, minutes, seconds&gt; (24hr format)</font>
 &nbsp;<font color="#434f54">//rtc.setDate(1, 8, 2017); &nbsp;&nbsp;// Uncomment to set the date to January 1st, 2014 (optional)</font>
<font color="#000000">}</font>

<font color="#00979c">void</font> <font color="#5e6d03">loop</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<font color="#000000">t</font> <font color="#434f54">=</font> <font color="#000000">rtc</font><font color="#434f54">.</font><font color="#d35400">getTime</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#000000">16</font><font color="#434f54">,</font> <font color="#000000">2</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">clear</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#434f54">// Send time</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">1</font><font color="#434f54">,</font><font color="#000000">0</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">"Wake up Darling"</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">0</font><font color="#434f54">,</font><font color="#000000">1</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">t</font><font color="#434f54">.</font><font color="#d35400">hour</font><font color="#434f54">,</font> <font color="#00979c">DEC</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">"hrs "</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">t</font><font color="#434f54">.</font><font color="#d35400">min</font><font color="#434f54">,</font> <font color="#00979c">DEC</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">"min "</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">t</font><font color="#434f54">.</font><font color="#d35400">sec</font><font color="#434f54">,</font> <font color="#00979c">DEC</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">"sec"</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">delay</font><font color="#000000">(</font><font color="#000000">1000</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;
 &nbsp;<font color="#434f54">//set alarm:</font>
 
 &nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">t</font><font color="#434f54">.</font><font color="#d35400">hour</font> <font color="#434f54">==</font> <font color="#000000">7</font> <font color="#434f54">&&</font> <font color="#000000">t</font><font color="#434f54">.</font><font color="#d35400">min</font> <font color="#434f54">==</font> <font color="#000000">0</font><font color="#000000">)</font> <font color="#434f54">//time to wake up</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#d35400">digitalWrite</font> <font color="#000000">(</font><font color="#000000">9</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#434f54">//turns on coffee machine</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">t</font><font color="#434f54">.</font><font color="#d35400">hour</font> <font color="#434f54">==</font> <font color="#000000">9</font> <font color="#434f54">&&</font> <font color="#000000">t</font><font color="#434f54">.</font><font color="#d35400">min</font> <font color="#434f54">==</font> <font color="#000000">0</font><font color="#000000">)</font> <font color="#434f54">//time to leave</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#d35400">digitalWrite</font> <font color="#000000">(</font><font color="#000000">9</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font> <font color="#434f54">//turn off coffee machine</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;
<font color="#000000">}</font>

</pre>

To run the simulator, click here https://circuits.io/circuits/3145505-wakeupdarling

![simwudlong](https://cloud.githubusercontent.com/assets/22894897/20111111/4b3acc94-a5a4-11e6-9b0f-a2b7fd9c4a49.gif)

This is how it looks finished:

![ard](https://cloud.githubusercontent.com/assets/22894897/22409013/4d5550a8-e640-11e6-88ca-2ea34c908431.jpeg)

![ardu](https://cloud.githubusercontent.com/assets/22894897/22409431/ef554e34-e646-11e6-88f4-6e593f6a52df.jpeg)

I used an old box to put it inside so water/dust can't damage it quickly.

![arduino](https://cloud.githubusercontent.com/assets/22894897/22409432/f21dc042-e646-11e6-93e8-440aac4209b4.jpeg)


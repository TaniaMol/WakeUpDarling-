# WakeUpDarling-
Arduino based Coffee Machine Alarm

Video:

![simwud-2](https://cloud.githubusercontent.com/assets/22894897/20111199/a6c945e0-a5a4-11e6-84fa-bf9d606cb45e.gif)

Lab View:

<img width="792" alt="screen shot 2016-11-06 at 2 09 11 pm" src="https://cloud.githubusercontent.com/assets/22894897/20041727/a91ebbfc-a42a-11e6-9671-c52053b632ea.png">

Parts I used:

Arduino Uno

Pushbuttons (2)

LCD 16x2

Wires

Resistors (220Ω, 1kΩ)

Potentiometer

Coffee Machine (light bulb in the simulator)

Diode (1N4001)

Transistor NPN (2222A)

Relay SPDT 5V (coil) 120VAC

Soldering iron

Grounding Converters (2) like the one in the picture

![31caoriq80l _sy300_](https://cloud.githubusercontent.com/assets/22894897/20027208/738834a8-a2d3-11e6-9fa4-d4ee56378a8d.jpg)

Once I had everything, I wired the coffee machine as it was the light bulb.  

Schematics View:

<img width="863" alt="screen shot 2016-11-08 at 8 56 51 am" src="https://cloud.githubusercontent.com/assets/22894897/20106175/78fda4d4-a591-11e6-80ca-00de4065ab49.png">

Code:

<pre>
<font color="#434f54">// &nbsp;_ ___ _______ &nbsp;&nbsp;&nbsp;&nbsp;___ ___ ___ &nbsp;___ _ &nbsp;&nbsp;_ ___ _____ ___ </font>
<font color="#434f54">// / |_ &nbsp;)__ / &nbsp;&nbsp;\ &nbsp;&nbsp;/ __|_ _| _ \/ __| | | |_ _|_ &nbsp;&nbsp;_/ __| </font>
<font color="#434f54">// | |/ / |_ \ |) | | (__ | || &nbsp;&nbsp;/ (__| |_| || | &nbsp;| | \__ \ </font>
<font color="#434f54">// |_/___|___/___/ &nbsp;&nbsp;\___|___|_|_\\___|\___/|___| |_| |___/ </font>
<font color="#434f54">// </font>
<font color="#434f54">// WakeUpDarling!</font>
<font color="#434f54">// </font>
<font color="#434f54">// Made by Tania Melino</font>
<font color="#434f54">// License: CERN Open Hardware License</font>
<font color="#434f54">// Downloaded from: </font><u><font color="#434f54">https://circuits.io/circuits/3126865-wakeupdarling</font></u><font color="#434f54"></font>

<font color="#5e6d03">#include</font> <font color="#434f54">&lt;</font><b><font color="#d35400">LiquidCrystal</font></b><font color="#434f54">.</font><font color="#000000">h</font><font color="#434f54">&gt;</font>

<b><font color="#d35400">LiquidCrystal</font></b> <font color="#000000">lcd</font><font color="#000000">(</font><font color="#000000">7</font><font color="#434f54">,</font> <font color="#000000">6</font><font color="#434f54">,</font> <font color="#000000">5</font><font color="#434f54">,</font> <font color="#000000">4</font><font color="#434f54">,</font> <font color="#000000">3</font><font color="#434f54">,</font> <font color="#000000">2</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#00979c">int</font> <font color="#000000">starttime</font><font color="#000000">;</font>
<font color="#00979c">int</font> <font color="#000000">activetime</font><font color="#000000">;</font>
<font color="#00979c">int</font> <font color="#000000">prevoustime</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font>

<font color="#00979c">int</font> <font color="#000000">hours</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font>
<font color="#00979c">int</font> <font color="#000000">mins</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font>

<font color="#00979c">void</font> <font color="#5e6d03">setup</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#000000">16</font><font color="#434f54">,</font> <font color="#000000">2</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">clear</font><font color="#000000">(</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;
 &nbsp;<b><font color="#d35400">Serial</font></b><font color="#434f54">.</font><font color="#d35400">begin</font><font color="#000000">(</font><font color="#000000">9600</font><font color="#000000">)</font><font color="#000000">;</font>

 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">11</font><font color="#434f54">,</font> <font color="#00979c">INPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">11</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">10</font><font color="#434f54">,</font> <font color="#00979c">INPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">10</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font> 
 &nbsp;<font color="#d35400">pinMode</font><font color="#000000">(</font><font color="#000000">9</font><font color="#434f54">,</font> <font color="#00979c">OUTPUT</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#d35400">digitalWrite</font><font color="#000000">(</font><font color="#000000">9</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;
 &nbsp;<font color="#000000">starttime</font> <font color="#434f54">=</font> <font color="#d35400">millis</font><font color="#000000">(</font><font color="#000000">)</font><font color="#434f54">/</font><font color="#000000">1000</font><font color="#000000">;</font>
<font color="#000000">}</font>

<font color="#00979c">void</font> <font color="#5e6d03">loop</font><font color="#000000">(</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;
 &nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#d35400">digitalRead</font><font color="#000000">(</font><font color="#000000">10</font><font color="#000000">)</font> <font color="#434f54">==</font> <font color="#00979c">LOW</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;
 &nbsp;&nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">0</font><font color="#434f54">,</font><font color="#000000">1</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">"What time is it?"</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">1</font><font color="#434f54">,</font><font color="#000000">0</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">hours</font><font color="#434f54">++</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;&nbsp;&nbsp;<font color="#5e6d03">else</font> <font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#d35400">digitalRead</font><font color="#000000">(</font><font color="#000000">11</font><font color="#000000">)</font> <font color="#434f54">==</font> <font color="#00979c">LOW</font><font color="#000000">)</font>
 &nbsp;&nbsp;&nbsp;
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">{</font> 
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">mins</font><font color="#434f54">++</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">}</font>
 &nbsp;&nbsp;&nbsp;
 &nbsp;
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">activetime</font> <font color="#434f54">=</font> <font color="#000000">(</font><font color="#d35400">millis</font><font color="#000000">(</font><font color="#000000">)</font> <font color="#434f54">/</font> <font color="#000000">1000</font><font color="#000000">)</font> <font color="#434f54">-</font> <font color="#000000">starttime</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">prevoustime</font> <font color="#434f54">&lt;</font> <font color="#000000">(</font><font color="#000000">activetime</font> <font color="#434f54">-</font> <font color="#000000">59</font><font color="#000000">)</font><font color="#000000">)</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">mins</font><font color="#434f54">++</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">prevoustime</font> <font color="#434f54">=</font> <font color="#000000">activetime</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">}</font> &nbsp;
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">mins</font> <font color="#434f54">&gt;</font> <font color="#000000">59</font><font color="#000000">)</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">hours</font><font color="#434f54">++</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">mins</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">}</font> 
 &nbsp;&nbsp;&nbsp;
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">hours</font> <font color="#434f54">&gt;</font> <font color="#000000">23</font><font color="#000000">)</font>
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">hours</font> <font color="#434f54">=</font> <font color="#000000">0</font><font color="#000000">;</font> 
 &nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">}</font>
 &nbsp;
 &nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">setCursor</font><font color="#000000">(</font><font color="#000000">6</font><font color="#434f54">,</font><font color="#000000">0</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;
 &nbsp;<font color="#5e6d03">if</font><font color="#000000">(</font><font color="#000000">hours</font> <font color="#434f54">&lt;</font> <font color="#000000">10</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">"0"</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">hours</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#5e6d03">else</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">hours</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;&nbsp;&nbsp;
 &nbsp;&nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">":"</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;
 &nbsp;<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">mins</font> <font color="#434f54">&lt;</font> <font color="#000000">10</font><font color="#000000">)</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#005c5f">"0"</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">mins</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>
 &nbsp;<font color="#5e6d03">else</font>
 &nbsp;<font color="#000000">{</font>
 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<font color="#000000">lcd</font><font color="#434f54">.</font><font color="#d35400">print</font><font color="#000000">(</font><font color="#000000">mins</font><font color="#000000">)</font><font color="#000000">;</font>
 &nbsp;<font color="#000000">}</font>

<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">hours</font> <font color="#434f54">==</font> <font color="#000000">6</font> <font color="#434f54">&&</font> <font color="#000000">mins</font> <font color="#434f54">==</font> <font color="#000000">0</font><font color="#000000">)</font>
<font color="#000000">{</font> 
 &nbsp;<font color="#d35400">digitalWrite</font> <font color="#000000">(</font><font color="#000000">9</font><font color="#434f54">,</font> <font color="#00979c">HIGH</font><font color="#000000">)</font><font color="#000000">;</font> &nbsp;
 &nbsp;&nbsp;&nbsp;<font color="#000000">}</font> 
<font color="#5e6d03">if</font> <font color="#000000">(</font><font color="#000000">hours</font> <font color="#434f54">==</font> <font color="#000000">10</font> <font color="#434f54">&&</font> <font color="#000000">mins</font> <font color="#434f54">==</font> <font color="#000000">0</font><font color="#000000">)</font>
<font color="#000000">{</font>
 &nbsp;<font color="#d35400">digitalWrite</font> <font color="#000000">(</font><font color="#000000">9</font><font color="#434f54">,</font> <font color="#00979c">LOW</font><font color="#000000">)</font><font color="#000000">;</font>
<font color="#000000">}</font>
 &nbsp;
<font color="#000000">}</font>

</pre>

The last part of the code is for you to select the alarm time.

To run the simulator, click here https://circuits.io/circuits/3145505-wakeupdarling

![simwudlong](https://cloud.githubusercontent.com/assets/22894897/20111111/4b3acc94-a5a4-11e6-9b0f-a2b7fd9c4a49.gif)

This is how it looks finished:

![whatsapp image 2016-11-08 at 15 05 07](https://cloud.githubusercontent.com/assets/22894897/20141226/a0661860-a64c-11e6-8023-d0a8f22379e6.jpeg)

![whatsapp image 2016-11-08 at 14 40 33](https://cloud.githubusercontent.com/assets/22894897/20141360/412e058c-a64d-11e6-8785-892cfd460142.jpeg)


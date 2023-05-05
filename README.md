Download Link: https://assignmentchef.com/product/solved-ece253-lab-1b-stopwatch
<br>



<h1>1           Overview</h1>

In Lab 1b we will design and implement a stopwatch using the Seven Segment Display and the Push Buttons on the development board. Most embedded systems by default have to interact with external events. Ideally, these events would be predictable and synchronous, but in many cases, they are not. Therefore, the embedded system must be able to deal with an event at an arbitrary time without affecting the code that is already executing. <strong>Interrupts </strong>are one method of dealing with this problem. Please be aware that this is time-dependent interrupt code development, please start the lab as soon as you can.

<h1>2           Hardware</h1>

The hardware to be used for this lab is the same as the one that you built in Lab 1A. There is 128 KB of BRAM available to you as well as 128 MB of the DDR2.

<h1>3           Project Settings</h1>

Create a new blank application project in SDK. Make sure to generate the linker script (Select the project and go to Xilinx −<em>&gt; </em>Generate Linker Script) with the Code, Data, Stack and Heap segments all mapped to the DDR2 (mig 7series 0 memaddr…) and <strong>not </strong>the BRAM. Also make sure that the the heap and stack sizes are 4096 (4 KB). You might need to increase the heap size further if you use malloc() in your code.

<h1>4           Writing Software</h1>

In this lab, you must present output on the 7-segment LED display. The 7 segment display is multiplexed. This means you can only light one digit position at a time, and must alternate between them quickly to show all the digits of a number. Specifically you must turn on the anode of exactly one digit position and set the segment cathodes to the correct value for that digit, then move onto the next. This alternation must be repeated quickly to take advantage of persistence of vision, and quickly enough to appear static. A display rate of 20 displays per second might be (barely) legible but will give people headaches. 50-60 times per second or even higher is much more comfortable, some people will still see noticeable flicker to beyond 120Hz.

An updated LED driver will be provided to do the bit packing associated with drawing a digit (Lab1B handout/software), or you can write this yourself. Add the two files to your project, and read the header file for details on the functions. Either

Figure 1: The sketch shows 2.1 seconds

way you have to write the code to alternate between drawing different digits rapidly. Grand loop is a common way to achieve this. Also remember that the display must be functional regardless of what state the counter is in.

The stop watch is controlled by 4 push buttons which are read in using a GPIO, just like the LEDs are output, however, you need to know when to look for an update. The hardware is configured to assert an interrupt when any change happens to the buttons (under software configuration control). Beware that buttons have contacts that bounce – so that you might find multiple assertions of the interrupts. (More on de-bouncing later).

The program will work as follows:

<ul>

 <li>The seven segments will count up from 0, and all eight segments must initially display 0. Use the upper four digits to display seconds (9999 – 0) and the lower four digits for fractions of second. Note: The default custom sevenSeg v 1 0 Verilog module provided in the lab 1 handout doesn’t support the decimal point.</li>

 <li>Time measurement counting is controlled by a hardware timer with a fixed frequency. Start out with a frequency of 2 Hz, so that it displays a new value every half a second. Use the upper 4 seven segments of each watch to display the seconds count. Use the lowest 4 seven segments to display fractions of seconds. It is up to you what resolution you will support (i.e. 1mS, 100uS…), but you must have a working version that correctly operates to 0.1s.</li>

</ul>

After your stopwatches are working, experiment with increasing the frequency of your timer. How small of time increments would it be possible to display for your stopwatch? Consider the timing requirements for displaying the

Seven Segment Display.

You will need to use 4 of your buttons as described below:

<ul>

 <li>When a <strong>reset </strong>push button is pressed, the stopwatch will reset to all zeros.</li>

 <li>When a <strong>start </strong>button is pushed, the stopwatch will begin timing.</li>

 <li>When a <strong>stop </strong>button is pushed, the stopwatch will freeze the time and display the result.</li>

 <li>When a <strong>count up </strong>button is pushed, the stopwatch will count up.</li>

 <li>When a <strong>count down </strong>button is pushed, the stopwatch will count down. If the stopwatch is at 0, this will do nothing as it cannot count lower.</li>

</ul>

Using your xSDK project from Lab1A as a starting point, write a new software project with the following functions:

<ol>

 <li>Implement the stopwatches counting using timer interrupts.</li>

 <li>Implement drawing to the Seven Segment Display (using grand loop).</li>

 <li>Implement 5 push buttons using interrupts.</li>

</ol>

To learn more about how to implement the interrupts read the code included in extra method.c and extra method.h. The auto-generated test code in the “peripheral test project” is also helpful. The documentation included on Gauchospace for

<strong>AXI Interrupt Controller Documentation </strong>is a good resource. This project uses interrupts from two different sources, the AXI Timer module and the AXI GPIO module. Both types of interrupts use slightly different programming implementations.

<strong>Tip: Create a ‘test peripheral’ project in xSDK to see how Xilinx implements interrupts according to your hardware configuration.</strong>

NOTE: For debugging an interrupt handler, use a variable to turn an LED on and off. Don’t use print() – it may well cause errors and since it is buffered, you can’t tell if it passed or not during execution.

<h2>4.1         Button Bouncing and Glitchs</h2>

Remove the count down button and make the count up button into a toggle that changes the counting direction with each press. Play around with it by trying lighter and harder presses. Notice any odd behavior? Slight changes in how the button is pressed and released can make the switch bounce. Since the button is linked to a toggle, this causes unwanted state changes. Does the glitch happen on depression or release (or both)?

Buttons are physical switches with internal springs at relatively high tension. When they are switched, nearly all buttons will make a contact, but then release it (possibly several times). The time between bounces depends on the tension and the button contact mass, but is usually between 30-300uS. The effect is that the interrupt you capture, may well be repeated multiple times. If you saw glitches in the modified program, please explain why you didn’t see them in the original

configuration.

<h1>5           Reporting</h1>

Within two pages, explain how design decisions for using a grand loop and interrupts changes the functionality of your stopwatch.

<ol>

 <li>How fast of a stop watch are you able to build using your hardware and software configuration? What was the limiting factor in the accuracy of your stopwatch?</li>

 <li>How did you control the display update timing (i.e. loop period) and did this effect accurate timing measurement? If so, how? What display update period did you choose?</li>

 <li>The push buttons in this stop-watch are implemented using interrupts. Would it be possible to instead check the values of your push buttons while executing the grand loop? Would this polling approach change the timing of your stop watch?</li>

 <li>What will happen if two push buttons are pressed at once?</li>

 <li>Describe the UI of the stopwatch. List the input and output devices.</li>

</ol>

Describe anything unique that you did to make your stopwatch work better
# Google Time Lamp Manual

In this manual, I will desribe how you can connect your Google Calender with your NodeMCU and how te change the colors of a LED strip based on events noted in your calender. 

I will guide you through the entire procces, but if you still get stuck somewhere, don't be afraid to take a look at the '*Common Errors*' chapter at the end of this manual. This manual is split into different chapters, each of these aim at completing a specific task (such as downloading the Google API, changing the colors depending on your calender, etc.).

Keep in mind that this manual is specifically made for the NodeMCU ESP8266, if you use something else, you may still find some parts of this manual usefull, but the specific code may be incorrect.



## What do you need?

- NodeMCU ESP8266
- LED strip
- A laptop or desktop
- Arduino IDE (Application)
- Adafruit account (Website)
- Zapier Account


## How to connect Google Calendar to a NodeMCU to influence different lights.

### Creating and setting up accounts

#### Step 1 creating an Adafruit account
Go to the Adafruit website and create an account if you don't already have one.
https://io.adafruit.com

#### Step 2 Creating a new Feed
In the navigation bar at the top of the screen is an option 'IO'. Click this option and afterwards click on 'Feeds' and an '+ New Feed'. We need this feed for later when we want multiple NodeMCU's to communicate with eachother. 

![Screenshot 2024-10-16 124917](https://github.com/user-attachments/assets/6028ef8a-03b6-4267-a507-4e276d24969d)

#### Step 3 Naming the new feed
After clicking on the '+ New Feed' action, a pop up will show up where we need to give our feed a name and description. We need to call upon this name multiple times during this manual and while coding so a simple name that is memorable is prefered. I will name my feed 'TimeLamp'. A desription is optional, so I will leave mine empty, but you can fill in whatever you want.

#### Step 4 Opening the feed
After naming and creating the feed it will show up on your screen under all your feeds. When opening it it will look something like this:

![image](https://github.com/user-attachments/assets/481726b1-6ae3-453b-a6ff-10412851c2ac)

Leave it open for now. We need this later down the line.

#### Step 5 Creating a Zapier account
Go to the Zapier website and create an account
http://zapier.com/

#### Step 6 Creating a Zap
To connect our Google Calender to Adafruit, we will need to make a Zap. Zap is an automated workflow which we can customize. To create one, we need to click on '+ Create' in the top left of our screen and click on 'Zaps'.

![Screenshot 2024-10-16 132317](https://github.com/user-attachments/assets/88ac4d09-fe1c-4636-b428-aba031b091a8)

After creating a Zap, you will be greeted with something like this:

![image](https://github.com/user-attachments/assets/f3068023-61e9-44f1-a015-25698a9d3e50)

We have now have a setup for everything we need. It's now time to start connecting!



### Connecting Google Calender to Adafruit using a Zap

#### Step 7 Setting the trigger
Go back to the Zap we just created. Here you will see a 'Trigger' block and an 'Action' block. We want to check for events in our Google Calender therefore we need to click on the 'Trigger' block and add google calender as our app. After selecting our app we will have a dropdown menu with the name 'Trigger Event'. Here we can specify when an action will be taken. In our case, we will check for 'Event start' (You can choose another trigger if you want, but I will be using this one). After selecting everything, it should look something like this:

![image](https://github.com/user-attachments/assets/df0277c9-40f3-499d-ae34-f24d38455d4c)

#### Step 8 Connecting your Google Account
When clicking on the 'Account' field, you will automatically be redirected to a new screen. Here you will be able to select the google account you would like to use. After linking your google account, it should look like this: (Note: Verwijzing naar nieuw google account aanmaken)

![Screenshot 2024-10-16 145729](https://github.com/user-attachments/assets/e86bf474-99fa-45ad-a7d1-0cc390fc68ad)


#### Step 9 Configuring details of our Calender
We can now press the 'Continue' button and choose a specific calender connected to our google account and add other details how much time before the event the trigger should activate. I will personally be setting it to 10 minutes in advance. We can also add an additional search term. When filling in a name here, the trigger will only activate if the event in our calender has that specific name in it. We will leave this empty for now.

![Screenshot 2024-10-16 151254](https://github.com/user-attachments/assets/b71fca1d-5e02-4dff-a8cb-2523fc095cd8)

#### Step 10 Sending data to our Adafruit IO feed
We also have an 'Action' block. Here we need to select select our feed we created on the Adafruit website. WE will need to do the following things:
1. Under 'Choose App', select Adafruit IO
2. Under Choose Action Event select 'Create Feed Data'
3. Under 'Choose account' log in using your Adafruit account. This is the account we made in step 1. It will ask for your username and a key. You can find this information on the adafruit website when pressing the yellow key button in the top right.

![image](https://github.com/user-attachments/assets/19ad386c-4634-4035-a26b-7741bbbe1edf)

#### Step 11 Customizing our feed data
We also have an 'Action' block. Here we need to select select our feed we created on the Adafruit website. WE will need to do the following things:
1. Under 'Feed Key', fill in the name of the feed we created in step 3
2. on the right in the 'Value' field, is a '+' button. Click on this and select the color ID option. If you can't find this option, have a look at the 'Common Errors' at the bottom of this manual.

#### Step 12 Testing
We are now almost ready to start automating. Click on the test button on the 'Action' blocks. The value will then show up in your Adafruit feed with a value of 1 to 12 depending on the color of the event. Now also run a test on the 'trigger' block, and we'll be ready to publish.


### Connecting our NodeMCU

#### Step 13 Setting up our Arduino IDE File
We are ready to start automating. We now have a program that reads our calender and sends the color data to a feed we can read with our NodeMCU.

1. Start with connecting your NodeMCU to your laptop or desktop.
2. Download the following code at GitHub. https://github.com/SummerDanoe/ReadGoogleCalFeed.
3. This code is a good setup for reading our feed data. After downloading it, open the 'readfeedtutorial.ino' and make sure all ports are correct.

#### Step 14 Filling in netwerk and account information
We now have a file where we need to fill in some information before it wil work. We will need to fill in the following information.

- On the top of the screen, you will see a second tab with the name 'config.h'. Click on this to see a different page. Here we need to fill in Adafruit account information just like we did in step 10.

![image](https://github.com/user-attachments/assets/5090a54e-bb16-42db-abf7-1c059d03b34c)

- We also need to connect our Ardiuno to a wifi network. Fill in the name of your wifi network and the password on line 34 and 35.

![image](https://github.com/user-attachments/assets/62ffa4e6-01ea-42ba-80ef-91282ccb829f)

#### Step 15 Connecting our NodeMCU to our feed
Now we need to send our feed information to our NodeMCU. Therefore we have to go back to our original page with the name 'readfeedtutorial.ino' instead of the 'config.h' page. Fill in the following:

- On line 8 we need to again fill in the name of our Adafruit account.

![image](https://github.com/user-attachments/assets/84b57ae8-2690-4238-98e7-8b4403589ea8)

- On line 11, it asks for the name of your feed, in my case it was "TimeLamp".

![image](https://github.com/user-attachments/assets/8b5c92ce-8bb9-4a51-be3a-128af4a64387)

- Lastly, at the bottem is a function called 'HandleMessage'. A lot of the information is here is not needed. We can simply replace all of the code in this funtion with a simple serialprint to test if it can read our feed. 

```Serial.println(data->toInt());```

### Changing the colors of the light depending on your calender.

Now comes the fun part. We will now start with changing the colors of our LEDstrip depending on the color of our event. 

#### Step 16 Downloading Libraries.
For our LEDstrip to work, we will need a new library. In the ArduinoIDE application, go to tools and 'manage libraries'. Here you can search for the Adafruit_NeoPixel library and download it wit all it's dependencies. Afterwards go to the top of your file and include the following code:

```
#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
 #include <avr/power.h> // Required for 16 MHz Adafruit Trinket
#endif
```

#### Step 17 Connecting our LED strip to NodeMCU

Our LEDstrip had 3 connectors with the following names "+5V, GND and DO". 
- Connect the +5V cable of the LEDstrip to a 3,3V pin on the NodeMCU
- Connect the GND cable of the LEDstrip to a GND pin on the NodeMCU
- Connect the DO cable of the LEDstrip to a D(Number) pin on the NodeMCU. I chose number 1.

Now that the LEDstrip is connected, we will also need to specify some things in our ArduinoIDE application to let the NodeMCU know a LED strip is connected.

Add the following code in the readfeedtutorial.ino file below the libraries:

```
// Which pin on the Arduino is connected to the NeoPixels?
#define PIN        D1 // On Trinket or Gemma, suggest changing this to 1

// How many NeoPixels are attached to the Arduino?
#define NUMPIXELS 15 // Popular NeoPixel ring size

// When setting up the NeoPixel library, we tell it how many pixels,
// and which pin to use to send signals. Note that for older NeoPixel
// strips you might need to change the third parameter -- see the
// strandtest example for more information on possible values.
Adafruit_NeoPixel pixels(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);
```

The PIN defines on what pin the LED strip is connected to. I connected it to D1, make sure to change the number if you connected it to another PIN. The NUMPIXELS defines how many LED's are on our strip. Change it to the amount of LED's you have on your LEDstrip.

Lastly, we need to initialize the LEDstrip. Add the following code in the void setup function:

```  pixels.begin(); // INITIALIZE NeoPixel strip object (REQUIRED)```

#### Step 18 (Final) Reading our feed data and changing colors

Now comes the tedious part. Each color has a ID from 1 to 11 in the google calender app that gets send to our feed. In the 'HandleMessage' function we will need to add a switch function from 1 to 11 and change the colors so they match with the colors of google. 

```
switch(data->toInt()){
case 1:
  //code goes here
break;
```

When adding multiple cases for each color ID it will look something like this. 

```
case 2:
  //code goes here
break;
case 3:
  //code goes here
break;
case 4:
  //code goes here
break;
```

The case number corresponds to the color id, so repeat this up until we reach case 11.

Now we add the code to change our LEDstrip color to each case. Add the following code in eacht case:

```
  for(int i=0; i<15; i++) {
    pixels.setPixelColor(i, pixels.Color(100, 100, 100));
    pixels.show();
  }
```

Lastly, we need to change the RGB to match the colors of the google calender events. Here are the color ID's with their corresponding RGB values:

- 1: 121, 134, 203
- 2: 51, 182, 121
- 3: 142, 36, 170
- 4: 230, 124, 115
- 5: 246, 191, 38
- 6: 244, 81, 30
- 7: 3, 155, 229
- 8: 97, 97, 97
- 9: 63, 81, 181
- 10: 11, 128, 67
- 11: 213, 0, 0

After filling in all the correct RGB values, everything should work! Now the color of the LED strip lights will change to match the color of the event in your google calender. 

Hopefully this manual was helpfull, and if something isn't working please take a look at the 'Common Errors' below.



### Common Errors

####I can't see my serial monitor?

If you can't find the serial monitor. It could be that you haven't opened it yet. To do so, go to 'Tools' and click on 'Serial Monitor'

![image](https://github.com/user-attachments/assets/06b81f0d-8feb-4078-a89c-63ac016254ec)

#### Nothing is showing in my serial monitor, or weird symbols are showing in my serial monitor?

Sometimes, the serial monitor and the actual code can be desynced. The file we used as setup already has code in place that should prevent it from happening. But when uploading the code, you still don't see the text confirming that it's attempting to connect, you can add an additional 
``` Delay(10000) ``` in the setup function. This will delay everything by an additional 10 seconds so the serial monitor has time to catch up. It should look something like this:

![image](https://github.com/user-attachments/assets/49ac124a-9def-48cc-9727-7fa5cbef217d)

Another reason for the serial monitor not working properly is that the Baud isn't matched with what we're calling in the code. You will then see something like this pop up in the serial monitor:

![image](https://github.com/user-attachments/assets/419e646c-e391-4fe7-a040-948a1b40e78a)

In the tutorial file we downloaded, the code is calling for a baud number equal to 115200, so we should change this in our serial monitor aswell. In the top right of our serial monitor we can change this number.

![image](https://github.com/user-attachments/assets/cf44f2fa-71ac-443f-b3fd-f93c23560c3d)

#### My LED strip isn't working

The first thing to check is if all the PINS are correctly installed. Again, the pins should be connected like this:

- Connect the +5V cable of the LEDstrip to a 3,3V pin on the NodeMCU
- Connect the GND cable of the LEDstrip to a GND pin on the NodeMCU
- Connect the DO cable of the LEDstrip to a D(Number) pin on the NodeMCU. I chose number 1.


Another reason could be that you haven't updated the max number of pixels on your LED strip when changing the colors. In each case of our 'switch' function we also need to state the number of lights we want to change, in my case it's 15, but it could be different with your LED strip. 

```
  for(int i=0; i<15; i++) {
    pixels.setPixelColor(i, pixels.Color(51, 182, 121));
    pixels.show();
  }
```

Here you can see the 'i<15' in the 'for' function. You will need to change this number in all your cases to the amount your LED strip has.

#### My Zapier isn't updating my feed with every event?

Sadly, Zapier is a bit inconsistant with when it detects an event in Google Maps. You could always make a second Zap that also checks for the event at a different time interval to reduce the amount of times it fails, but sadly it's still not reliable. For betters result it would be possible to, either make a server yourself that checks for google calender events, or use a deffirent website that sends feed data. But that is another tutorial for itsel!


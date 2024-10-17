# Google Time Lamp Manual

In this manual, I will desribe how you can connect your Google Calender with your NodeMCU and how te change the colors of a LED strip based on events noted in your calender. 

I will guide you through the entire procces, but if you still get stuck somewhere, don't be afraid to take a look at the '*Common Errors*' chapter at the end of this manual. This manual is split into different chapters, each of these aim at completing a specific task (such as downloading the Google API, changing the colors depending on your calender, etc.).

Keep in mind that this manual is specifically made for the NodeMCU ESP8266, if you use something else, you may still find some parts of this manual usefull, but the specific code may be incorrect.



## What do you need?

- NodeMCU (2x)
- Google Calender API
- LED strip
- A laptop or desktop
- Arduino IDE (Application)
- Adafruit account (Website)
- Voice detector (Optional)



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

#### (Step 9.5 Creating paths)
If you want specific colors for specific events, we will need to create extra paths and filters. Otherwise it will always change to the same color and doesn't care about what type of event is in your calender.

- Creating paths. Click on the '+' icon beneath the trigger event and select the 'paths' option. This will create 2 different paths were we can set conditions for which path should be followed.

![Screenshot 2024-10-17 114516](https://github.com/user-attachments/assets/eb11eeb4-7da3-4eb3-81d7-f78654200f51)

- If you want more then 2 paths, clicking on the '+' icon beneath the 'paths' block will create an additional path. I will personally use 3 paths.
- We will now set conditions for each path. Sadly, as far as I am aware, we cannot directly send the color data of an event to our feed. We will therefore check for specific words in description for events. Click on on of the the divirging paths with 'condition' in them. You will then see this:

![image](https://github.com/user-attachments/assets/fffc5830-f3fb-4aa9-900b-f884550569f3)

We will fill it in like this: "Only continue if, description, (text) contains, Work". This way this path will only trigger when the starting event has a desription which contains the word "Work". 

![image](https://github.com/user-attachments/assets/8efb8da2-cda1-42f9-93c4-1fa616d75194)

- In the second path the condition will be exactly the same, but with the word "Meeting". The third condition will be a little different. This path will be used when the description DOESN'T have the word "Work" and "Meeting". This way this path will continue whenever an event isn't labeled.

#### Step 10 Sending data to our Adafruit IO feed
We also have an 'Action' block. Here we need to select select our feed we created on the Adafruit website. WE will need to do the following things:
1. Under 'Choose App', select Adafruit IO
2. Under Choose Action Event select 'Create Feed Data'
3. Under 'Choose account' log in using your Adafruit account. This is the account we made in step 1. It will ask for your username and a key. You can find this information on the adafruit website when pressing the yellow key button in the top right.

![image](https://github.com/user-attachments/assets/19ad386c-4634-4035-a26b-7741bbbe1edf)

#### Step 11 Customizing our feed data
We also have an 'Action' block. Here we need to select select our feed we created on the Adafruit website. WE will need to do the following things:
1. Under 'Feed Key', fill in the name of the feed we created in step 3
2. Under 'Value' fill in value 1
3. (Optional) If you created multiple paths in step 9.5, you will need to give each path a different value. I will give the "Work" path a value of 1, "Meeting" a 2 and the last one a 3.

#### Step 12 Testing
We are now almost ready to start automating. Click on the test button on one of the 'Action' blocks. The value will then show up in your Adafruit feed with a value of 1 (, 2 or 3 depending on which path you tested). Before we can turn this zap on we need to test every block. So click on every block en click on the test button! Afterwards, don't forget to turn publis the zap to turn it on!


Connecting multiple Arduino’s

Step 1
Step 2
Etc.

Changing the colors of the light depending on your calender.

Step 1
Step 2
Etc.

(Optional) Making new events using your voice

Step 1
Step 2
Etc.




Common Errors


Explaining the different kinds of errors/

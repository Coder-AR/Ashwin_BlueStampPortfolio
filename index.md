# Chess Clock
Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!



| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Ashwin R | International School | Computer Hardware | Incoming Junior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/qCYACPenO60?si=U6mSXugfV7t4DbD8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- I was able to rewrite my chess clock's setup, game state, and end state code so that it works properly. 
- I have been surprised by how quickly I can learn new skills. Once I got the hang of something, I was able to work on it quickly.
- I was able to overcome the difficulty of getting my code to work by working on each component separately. This allowed me to identify the exact problems in my code, rather than receiving a multitude of errors at once.
Before my final milestone, I need to complete the CAD design for my chess clock's housing. I also need to finish wiring up the tactile buttons.

# Second Milestone Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
#include <TM1637Display.h>

// Rotary Encoder Inputs
#define CLKEN 13
#define DT 12
#define SW 11
#define CLK1 5
#define DIO1 6
#define CLK2 8
#define DIO2 9
TM1637Display display1 = TM1637Display(CLK1, DIO1);
TM1637Display display2 = TM1637Display(CLK2, DIO2);
int gameState = 0;
int counter = 0;
int displayTime = 0;
int displayTime1 = 0;
int counter1 = 0;
int formatTime = 0;
int formatTime1 = 0;
int gameTime = 0;
int gameTime1 = 0;
int currentStateCLK;
int lastStateCLK;



int buttonPin = 2;
int button2Pin = 4;
int buzzerPin = 10;
int buttonState = 0;
int button2State = 0;



String currentDir = "";
unsigned long lastButtonPress = 0;

unsigned long lastButtonPress1 = 0;

unsigned long lastButtonPress2 = 0;


bool timer1Active = false;
bool timer2Active = false;




void setup() {

  // Set encoder pins as inputs
  pinMode(CLKEN, INPUT);
  pinMode(DT, INPUT);
  pinMode(SW, INPUT_PULLUP);

  // Setup Serial Monitor
  Serial.begin(9600);

  // Read the initial state of CLK
  lastStateCLK = digitalRead(CLKEN);

  display1.setBrightness(7);
  display2.setBrightness(7);
  display1.clear();
  display2.clear();
  pinMode(buttonPin, INPUT);
  pinMode(button2Pin, INPUT);
  pinMode(buzzerPin, OUTPUT);

}

void loop() {

  counter = constrain(counter, 0, 5999);
  counter1 = constrain(counter1, 0, 5999);
  formatTime = constrain(formatTime, 0, 5999);
  formatTime1 = constrain(formatTime1, 0, 5999);
  buttonState = digitalRead(buttonPin);
  button2State = digitalRead(button2Pin);
  // Read the current state of CLK
  currentStateCLK = digitalRead(CLKEN);
  if (gameState == 0) { 
  // If last and current state of CLK are different, then pulse occurred
  // React to only 1 state change to avoid double count
    if (currentStateCLK != lastStateCLK && currentStateCLK == 1) {

    // If the DT state is different than the CLK state then
    // the encoder is rotating CCW so decrement
      if (digitalRead(DT) != currentStateCLK) {
        counter = counter+30;
        counter1 = counter1+30;
        currentDir = "CCW";
      } else {
      // Encoder is rotating CW so increment
        counter = counter-30;
        counter1 = counter1-30;
        currentDir = "CW";
    }

      Serial.print("Direction: ");
      Serial.print(currentDir);
      Serial.print(" | Counter: ");
      Serial.println(counter);
      gameTime = counter;
      gameTime1 = counter1;
      int minutes = gameTime / 60;
      int seconds = gameTime % 60;
      formatTime = (minutes * 100) + seconds;
      int minutes1 = gameTime1 / 60;
      int seconds1 = gameTime1 % 60;
      formatTime1 = (minutes1 * 100) + seconds1;
      display1.showNumberDecEx(formatTime, 0b11100000, false, 4, 0);
      display2.showNumberDecEx(formatTime1, 0b11100000, false, 4, 0);
      } }

  if (gameState == 1) {
    if (buttonState == HIGH && millis() - lastButtonPress1 > 200) {
    timer1Active = true;
    timer2Active = false;
    lastButtonPress1 = millis();
    Serial.println("Timer 1 started");
  }

  // Check button 2 (start Timer 2)
  if (button2State == HIGH && millis() - lastButtonPress2 > 200) {
    timer2Active = true;
    timer1Active = false;
    lastButtonPress2 = millis();
    Serial.println("Timer 2 started");
  }

  // Timer 1 countdown
  if (timer1Active && gameTime > 0) {
    gameTime--;
    int minutes = gameTime / 60;
    int seconds = gameTime % 60;
    displayTime = (minutes * 100) + seconds;
    display1.showNumberDecEx(displayTime, 0b11100000, false, 4, 0);
    delay(1000);  // Simple blocking delay for countdown
  }

  // Timer 2 countdown
  else if (timer2Active && gameTime1 > 0) {
    gameTime1--;
    int minutes = gameTime1 / 60;
    int seconds = gameTime1 % 60;
    displayTime1 = (minutes * 100) + seconds;
    display2.showNumberDecEx(displayTime1, 0b11100000, false, 4, 0);
    delay(1000);  // Simple blocking delay for countdown
  }

  if ((timer1Active && gameTime == 0) || (timer2Active && gameTime1 == 0)) {
  gameState = 2;
  }

  if (gameState == 2) { 
    tone(buzzerPin, 784);
    delay(5000);
    noTone(buzzerPin);

  }
}

  // Remember last CLK state
  lastStateCLK = currentStateCLK;

  // Read the button state
  int btnState = digitalRead(SW);

  //If we detect LOW signal, button is pressed
  if (btnState == LOW) {
    //if 50ms have passed since last LOW pulse, it means that the
    //button has been pressed, released and pressed again
    if (millis() - lastButtonPress > 50) {
      Serial.println("Button pressed!");
      gameState = gameState + 1;
    }

    // Remember last button press event
    lastButtonPress = millis();
  }

  // Put in a slight delay to help debounce the reading
  delay(1);
}
```


# First Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/-Dd418K57Yo?si=adzvxJMZy9eCWcms" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

- My project needs an Arduino Uno, 2 7-segment displays, a buzzer, and a rotary encoder. They will be able to integrate to
  set the time on the display using the rotary encoder and the Arduino. Then, the Arduino can count the time down. Finally,
  the buzzer will go off once the time reaches zero.
- So far, I have been able to set the time on the first display. I have also been able to get each component of my clock
  working independently.
- One challenge I've been facing is that the rotary encoder is not able to set values past 2.
- My plan to complete my project is to rewrite my time-setting code, as I know both parts are working. 
  From there, I plan to work on the code for the game state and the ending state. Once I have both of those done,
  I hope to be able to make a case for my chess clock using CAD.


# First Milestone Code

Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
#include <Arduino.h>
#include <TM1637Display.h>


#define CLK1 5
#define DIO1 6
#define CLK2 8
#define DIO2 9
#define outputA 13
#define outputB 12
TM1637Display display1 (CLK1, DIO1);
TM1637Display display2 (CLK2, DIO2);
int counter1 = 0;
int counter2 = 0;
int counter3 = 0;
int counter4 = 0;

int aState;
int aLastState;  

int buzzerPin = 10;
int buttonPin = 2;
int button2Pin = 4;
int robuttonPin=11;

int buttonState = 0;
int button2State = 0;

int buttonPresses = 0;
int robuttonState = 0;
int setTime = 1;




void setup() {
  // put your setup code here, to run once:
  display1.setBrightness(5);
  display2.setBrightness(5);
  display1.clear();
  display2.clear();
  
  pinMode(buzzerPin, OUTPUT);
  pinMode(buttonPin, INPUT);
  pinMode(button2Pin, INPUT);
  pinMode(robuttonPin, INPUT);
  pinMode (outputA,INPUT);
  pinMode (outputB,INPUT);
  aLastState = digitalRead(outputA); 
  Serial.begin (9600); 


}

void loop() {
  counter1 = constrain(counter1, 0, 9);
  counter2 = constrain(counter2, 0, 9);
  counter3 = constrain(counter3, 0, 5);
  counter4 = constrain(counter4, 0, 9);
  robuttonState = digitalRead(robuttonPin);
  if (robuttonState == LOW) {
    setTime++; 
    delay(300); 
  }

  aState = digitalRead(outputA);

  if (setTime == 1) {
    Serial.println("setTime1");
    if (aState != aLastState){     
      if (digitalRead(outputB) != aState) { 
        counter1 ++;
      } else {
        counter1 --;
      }
      display1.showNumberDec(counter1,false,1,0);
      delay(500);
    } 
    aLastState = aState;
  }

  if (setTime == 2) {
    Serial.println("setTime2");
    if (aState != aLastState){     
      if (digitalRead(outputB) != aState) { 
        counter2++;
      } else {
        counter2--;
      }
      display1.showNumberDec(counter2 % 10, false, 1, 1);  
      delay(50); 
    }
    aLastState = aState;
  }

  if (setTime == 3) {
    Serial.println("setTime3");
    if (aState != aLastState){     
      if (digitalRead(outputB) != aState) { 
        counter3++;
      } else {
        counter3--;
      }
      display1.showNumberDec(counter3 % 10, false, 1, 2); 
      delay(50); 
    }
    aLastState = aState;
  }

  if (setTime == 4) {
    Serial.println("setTime4");
    if (aState != aLastState){     
      if (digitalRead(outputB) != aState) { 
        counter4++;
      } else {
        counter4--;
      }
      display1.showNumberDec(counter4 % 10, false, 1, 3);  
      delay(50); 
    }
    aLastState = aState;

  }  
  
}

```

# Schematics

Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 



# Bill of Materials


| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Elegoo Starter Kit | Arduino Uno R3, Passive Buzzer, Breadboard, Jumperwires, etc. | $44.99 | <a href="https://www.amazon.com/ELEGOO-Project-Tutorial-Controller-Projects/dp/B01D8KOZF4"> Link </a> |
|:--:|:--:|:--:|:--:|
| Rotary Encoder | Changing the time on the Matrix Displays. | $7.99 | <a href="https://www.amazon.com/SongHe-KY-040-Encoder-Development-Arduino/dp/B087ZQLLWQ"> Link </a> |
|:--:|:--:|:--:|:--:|
| 4 Digit 7 Segment LED Displays | Used for displaying the time | $7.99 | <a href="https://www.amazon.com/WWZMDiB-Module%EF%BC%8CLED-Brightness-Adjustable-Accessories/dp/B0BFQNFX6D"> Link </a> |
|:--:|:--:|:--:|:--:|
| USB C to USB A Adapter | Used for connecting the USB A to USB B cable if you only have USB C | $2.99 | <a href="https://www.amazon.com/WWZMDiB-Module%EF%BC%8CLED-Brightness-Adjustable-Accessories/dp/B0BFQNFX6D](https://www.amazon.com/ENVEL-Transfer-Converter-Thunderbolt3-Compatible/dp/B0D3T2QDVJ?th=1"> Link </a> |


# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.

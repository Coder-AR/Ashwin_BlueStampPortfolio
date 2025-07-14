# Chess Clock
Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

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

<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone 


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

Self Driving Car
My project is an Arduino-based self-driving car that uses ultrasonic and infrared sensors to detect obstacles and navigate autonomously. In addition to self-driving capabilities, the car will include a remote-control mode, allowing it to switch between manual and autonomous operation. This project combines programming, electronics, and robotics to demonstrate the fundamentals of autonomous vehicle technology.

You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 
<!--- This is an HTML comment in Markdown -->
<!--- Anything between these symbols will not render on the published site -->
```

| Engineer|School Name| Interests | Grade|
|:--:|:--:|:--:|:--:|
| Charlie T| Paul D. Schreiber High School |AI and Software Engineering| Incoming Juior

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

<iframe width="560" height="315" src="https://www.youtube.com/embed/CaCazFBhYKs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

* **Project Overview:** I am building an Arduino-powered self-drivingcar that can detect obstacles, navigate around them, and switch between autonomous and remote-controlled driving modes.

* **Components & Integration:** The project uses an Arduino R3, ultrasonic sensor, infrared obstacle sensors, L9110 motor driver, TT motors, IR Transmitter, and a rechargeable battery. The Arduino processes sensor data and controls the motors to safely navigate the environment.

* **Progress So Far:** I have assembled the car, connected the motors and sensors, tested the hardware, and begun programming the obstacle detection and motor control systems as well as the manual mode.

* **Challenges:** My biggest challenge is creating smoother and smarter navigation instead of simply backing up and turning.

* **Next Steps:** I will improve the obstacle avoidance algorithm,test the car in different obstacle courses, and refine the software until it can reliably drive both autonomously and under manual control.

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  Serial.println("Hello World!");
}

void loop() {
  // put your main code here, to run repeatedly:

}
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| SunFounder R3 Board | Used for prototyping electronics and robotics. | $15 | <a href="https://eu.robotshop.com/products/sunfounder-uno-r3-control-board"> Link </a> |
|L9110 Motor Driver Module|Used to control the speed and direction of up to two DC motors. | $7.79 for 5 | <a href="https://www.amazon.com/HiLetgo-H-bridge-Stepper-Controller-Arduino/dp/B00M0F243E"> Link </a> |
| TT Motor|Used to power the wheels of robot cars. | $9.99 for 6| <a href="https://www.amazon.com/AEDIKO-Motor-Gearbox-200RPM-Ratio/dp/B09N6NXP4H/ref=sr_1_1?adgrpid=190073077647&dib=eyJ2IjoiMSJ9.VFvmjZ6X2lLerHG6wM2_Z7nBQu_C6jwuUD0a5dykvPqqIOYn1t4CYgMQikHFRydWNrOgcg7Du12M0TNoCAvWLVd1a2jxXPN89_YAP8k3av3yWcEe0zJEz8EegRZZENvovEA_Nxpos9FSG22ubw1-FQ0RTkh9oQvk0QtiFDFym883joE2MHcwbHHQBOIFutuVw5QiKR0Wkr8jlKaGVxiPnozER0ukB109xIuyg7vBR245Ll2KqQCOdxF4hvL4uji3LAy5XU2FbyjgbnGcrPrAIMQMcL3brDmLYshb1MSu28I.geDsvCohdlEJhfN60OlsG9D3NIqJRr1RVnkw7HFSc94&dib_tag=se&hvadid=779556177337&hvdev=c&hvexpln=0&hvlocphy=9004329&hvnetw=g&hvocijid=7982671839558946546--&hvqmt=e&hvrand=7982671839558946546&hvtargid=kwd-1930315191&hydadcr=3535_13857088_11052&keywords=tt+motor&mcid=116ed5dee20b3426a82053fa2363321d&qid=1784582678&sr=8-1"> Link </a> |
| Ultrasonic Module | Uses sound waves to measure distance without physical contact. | $8.99 for 5 | <a href="https://www.amazon.com/ELEGOO-HC-SR04-Ultrasonic-Distance-MEGA2560/dp/B01COSN7O6"> Link </a> |
| Obstacle Avoidance Module |Uses an infrared transmitter and receiver to detect nearby objects.| $9.96 for 10 | <a href="https://www.amazon.com/OSOYOO-Infrared-Obstacle-Avoidance-Arduino/dp/B01I57HIJ0"> Link </a> |
|9V Batteries| Used to Power the Car | $8.99 for 10 | <a href="amazon.com/PKCELL-9V-Batteries-Battery-Detector/dp/B00ZTS55Y4/ref=sr_1_1_sspa?crid=136RUNZNEKEEP&dib=eyJ2IjoiMSJ9.8xIC2eXJTnIdYA30fCJOnwpj50bL6qiESVMJBDb6SNyQ4dL_0_l4gmMTjMviAMDgoAjMG_wPsSVU03QwXiavauZRuNcPo87IYGO8h3w0JQmbUsuURQ6InWWDWLfvBN7Ahumt4syvHh6RUCMKjkvnrmaNkw1wcve4oVFVdX9gVuxtwBrB8jfP7xjJS8262pwbiuxBLUn3L9Kv487GPg3lPFjWWjr1eA59uwvrZGCmGDgqUo7qQqw7VfgJGsTVoSSBI2-zljurPNlQGg_MdCjz1q4OuiC0tTa5bUeFKfOV0_Y.7feILipDBbT7RmXkSqrm4jOC7CXmGrCHXt6qp9p2VCQ&dib_tag=se&keywords=9v+batteries&qid=1784583169&rdc=1&s=electronics&sprefix=9+v+%2Celectronics%2C110&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| Small BreadBoard | Used to organize wires | $5.99 for 6 | <a href="https://www.amazon.com/WWZMDiB-SYB-170-Breadboard-Plates-Multicolored/dp/B09YXQJMTG/ref=sr_1_2_sspa?crid=3CD4E2Z1KHHPK&dib=eyJ2IjoiMSJ9.izD01hGoehT6aqD9wbs5-QgpQ2udoLGHXOy-GNGcnLvnLVxQ0ySPHTRRot-0oo173NyWqBt50b91QsIj_C_1AKHwHETxALf9zhSnQ7oJUzhnxQ9w_OhUpSJZp7MI2z5pPimEBy_UZgwRwIjUmrKldaixzr7eB_f7fdgh1VBDgA7O-p0WMl8AD4sdnVQjd3p3thzJwB175aLMYGFjKLpTvpaU_6oyG5Cr193PIrSI-rvlZsesyWeXGh32-pFyDNbpFq0QbPOGeFkmwWEajCKwATrnxEyJB4NPhSkPNFmAFv8.xEEs_i3anQTuLZT4xIAVnz1SMMCJRKmFRIYuINnJwZQ&dib_tag=se&keywords=small+breadboard&qid=1784583233&s=electronics&sprefix=small+breadboard%2Celectronics%2C106&sr=1-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
|Jumper Wires| Used to connect components, breadboards, or circuit boards without soldering | $6.98 for 120 | <a href="https://www.amazon.com/Elegoo-EL-CP-004-Multicolored-Breadboard-arduino/dp/B01EV70C78/ref=sr_1_1_sspa?crid=PXOD9MMRX0H3&dib=eyJ2IjoiMSJ9.tjHxIQLJsk16_0YVtUGN6erd5BDBwPCXF98pEA_v-6tuB6AWi5A5z-nJ-MVkqPYvYbcPR7RCNLkAKwZk0KfdMa2A-_B7iFJNKz0BIdenBsHE-RAYTtoJfmT_suO-tl_h-1V6AdShARMODAO5HUppHSv-cCCKVz9vNtygBHAPPNQa5-JJQ3dNEysuztTv1kIXbBaRMKy4w9EzAVmcHRBCQPKZtKVQCl2phJrTlpadct9XBNDJptnNvofBS699oCOAfUhmsNDB08NIYs4QXc0Mf4QT4IRBEuRbm8QlAQz_DxQ.LbqvskcJVhHJNijUHeYpJ-jPrnRCMgUWOis9EC0gV9E&dib_tag=se&keywords=Jumper%2Bwires&qid=1784583322&s=electronics&sprefix=jumper%2Bwires%2Celectronics%2C106&sr=1-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| Arduino USB Cable | Used to upload code from your computer to the board, provide operating power, and allow serial communication between the Arduino and the IDE. | $7.99 | <a href="https://www.amazon.com/Arduino-Data-Sync-Cable-Microcontroller/dp/B08RCJXY1Z/ref=sr_1_2_sspa?crid=NCRMCC96SMKT&dib=eyJ2IjoiMSJ9.4_ZhDX5GMYMW0_L6Pn9rz2Wp3T_M5sgp4fwIaCkGTYw8Ym9FiweWWPMchlMk6FyGCG320--8vb78bNBix_P_B5H0G2mftfh7IPNgklFbuF_3dT9ugyUa1l42DkHYwoY1uT1nU3Ux-fVqIp66zD_UJVtkKeA6_-U8hWR722kk5BV1y4ci9InER4hr3GqsXaZ0bSYprjqlZ35WdT-Ec9kSZje6zQ4KjlgG0FBT5q22W2E.0tCWhcZM8js85gwgQREo8Tll6XyaFqKYBhnNtfzUcqE&dib_tag=se&keywords=USB+Cable&qid=1784583439&sprefix=usb+cable%2Caps%2C124&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| M 2.5x6,3x10,3x6,3x30 Screw and M 2.5x11,3x24,3x12 Standoff and Screwdriver |Used to secure things | $9.999| <a href="https://www.sunfounder.com/products/nylon-screws-kit?srsltid=AfmBOooD29WSCB8SlcIg-gVC2-4dRu7FOmrLItz_r_IKrDvoGbrZM8Tb"> Link </a> |
| TT Wheels| Used for the Motion of the Car and synced to TT Motor  | $8.97 for 8 | <a href="https://www.amazon.com/ThtRht-Motor-Wheels-Replacement-Smart/dp/B0CG1C7T8J/ref=sr_1_1?dib=eyJ2IjoiMSJ9.ZrpNmrbicccc2COTV1s2mCEsEJTAUea5_2CFRsGAz92T-_D2Ae4N8ofcLTHda_0he0dEgPhI_ZzWvPuUP3MhDr7X47bI8PgOF389a0uR4R2TE9GxbJxTBiewW23a0W3JaENJOSdhDsrQuAErW6aBHsWUm6hFnOdz43yf5VFmULojd34zVyPTFEZVKCJf216u9keFRepgvY7GE6LsA2v3v4jb9YB288_eLSqTP73FVghuF_0dkFJiK8aY9BgWOskrxtNXcqVuu7-Xn3gRX_3l7cAhU4_ItyBLlhU4U076oBY.hSbZULzGySTuizhEs64PuyZvMh5b7aIAlB5Sxo_kOu8&dib_tag=se&keywords=tt+motor+wheel&qid=1784583968&sr=8-1"> Link </a> |
|Universal Wheel | Used for extra stablility  | $8.99 for 4 | <a href="amazon.com/Dalyndar-Replacement-Universal-Rollers-Furniture/dp/B0DRX77FLV/ref=sr_1_3?crid=1ELDU0ZHZRHMG&dib=eyJ2IjoiMSJ9.Jyhhe1k2AZk0VOGaWFzYNhMZ1oC4peq1Um18nUTrwaRNtIRbBU9y0q1TmgyH0HguQKftY8-75UrnsSXSmNEBhTE9D6TD62Ikkf7uhmW-p77jfDjSoQbRZzpN0Sur40DNBIk4OYtafmxnR1zpx53P5MQBNi5o_r9P4J12Bi7Shwcz5lcgrVISfXSAetuS0OEPoPMsAqlHuG8xsrtwfr0yW97MqLtclyII1_AfyuvTZUw.GCsH5CRSE7ZvrVeUnOjoCH5f1miV3gSl4cWaXLU_nRw&dib_tag=se&keywords=universal+wheel+small&qid=1784584030&sprefix=universal+wheel+samkk%2Caps%2C115&sr=8-3"> Link </a> |
| Velcro | Used to fasten thingss| $8.24 for 12 | <a href="https://www.amazon.com/Melsan-inch-Hook-Loop-Tape/dp/B07Y3SZCRY/ref=sr_1_2_sspa?crid=243TMF6EXLQPO&dib=eyJ2IjoiMSJ9.Wqxj0Djxnfg8ZyY40TUmW_SA3xs4VISa62X9I26GI3wUbEATrTasKP86pYI-7JqkVrWMDd26poJEG9pLEdcY1c6SJP4xoa95dWacivkjnE7eKQnMsXtVFDJogME_Flo6ejIF4--t-Caz2K7rgI205My2vMGwpTTfSFDLHVEbQuvOc-GluEuoes8DQ5xPKpJWUo0-qmTmqA-XOAxy1qBMonmeUDVrJEZ8RPi2N-8PIL4.sK1fV1X5HoJRpKG2vr7vWmNfHMwbbY0XMc-d0YE2w2Q&dib_tag=se&keywords=velcro%2Bstrips&qid=1784584091&sprefix=velcro%2Bstrips%2Caps%2C139&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&th=1"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.

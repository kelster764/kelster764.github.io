---
title: "BoneVoyage"
showDate: false
showWordCount: false
showReadingTime: false
---
What is BoneVoyage?
- 

You are a train conductor checking for tickets in a train to the afterlife. If someone doesn't have their ticket, you can grab them by the neck and throw them off the train! Or you can just do that if you feel like it. 

Unfortuneatly, our code got lost, but I will do my best to explain what I did and my thought process. 

![alt text](<bonevoyage1.gif>)


![alt text](<bonevoyage2.gif>)

Dialogue System
-

![alt text](<bonelogue.gif>)
![alt text](<psuedocode.png>)

- Within our dialogue system, I wanted the words to print out like a type writer, and I only had it print 2 sentences at a time to have that satisfying feel that visual novels have. 

- We had an excel spreadsheet with all of the dialogue for the different characters and situations. 

- Once the dialogue is grabbed, we enter a while loop, thank is while the length of the dialogue doesn't equal 0, keep going.

- We then grab the first 2 sentences of the dialogue and put it in a seperate text, and then delete it from the original dialogue.

- In the seperate text, I do a similar process. While the seperate text is not empty, take a single character and add it to the output. This is what makes the typing effect.

- Once that inner while done, we go back to the outer while loop and see if there is more in the original dialogue. 

Interaction
-

![alt text](<interact.gif>)

For press E to interact, you hover over a skeleton you want to interact with. When the mouse is over the skeleton, they get a white highlight to indicate you are in fact hovering over him. Once you press E, you are lerped into a fixed position where you can either uninteract or strangle them. 

- The camera had to smoothly interpolate to a fixed position

- Upon the camera lerping into a fixed position, the location of the player also had to change to the location of the camera. 

- However, if the player is lerped to a bad spot, they could easily clip through a bench if we are not careful. 

I would consider BoneVoyage a strong predecessor to my camera work in Lantern of Souls. 





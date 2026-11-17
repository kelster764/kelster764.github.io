---
title: "BoneVoyage"
showDate: false
showWordCount: false
showReadingTime: false
---

What is BoneVoyage?

You are a train conductor checking tickets on a train to the afterlife. If someone doesn't have a ticket, you can grab them by the neck and throw them off the train! Or you can just do that if you feel like it.

Unfortunately, our code got lost, but I will do my best to explain what I did and my thought process.

![alt text](<bonevoyage1.gif>)

![alt text](<bonevoyage2.gif>)

Dialogue System

![alt text](<bonelogue.gif>)
![alt text](<psuedocode.png>)

- Within our dialogue system, I wanted the words to print out like a typewriter, and I had them print only two sentences at a time to create that satisfying feel that visual novels have.
- We had an Excel spreadsheet with all of the dialogue for the different characters and situations.
- Once the dialogue is retrieved, we enter a while loop. While the dialogue's length doesn't equal zero, we keep going.
- We then grab the first two sentences of the dialogue and put them in a separate text, then delete them from the original dialogue.
- In the separate text, I use a similar process. While the separate text is not empty, I take a single character and add it to the output. This is what creates the typing effect.
- Once that inner while loop is done, we go back to the outer while loop and check whether there is more in the original dialogue.

Interaction

![alt text](<interact.gif>)

To interact, you hover over a skeleton. When the mouse is over the skeleton, they get a white highlight to indicate that you are hovering over them. Once you press E, you are lerped into a fixed position where you can either stop interacting or strangle them.

Some problems I handled were:

- The camera had to smoothly interpolate to a fixed position.
- When the camera lerped into a fixed position, the player's location also had to change to match the camera's location.
- However, if the player was lerped to a bad spot, they could easily clip through a bench, so we had to be careful.

I would consider BoneVoyage a strong predecessor to my camera work in Lantern of Souls.


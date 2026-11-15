---
title: "Cupid Shooter"
showDate: false
showWordCount: false
showReadingTime: false
---

  ![alt text](<cupid_shooter.gif>)

  The Cupid Shooter Vision
  -

  You, cupid, are spawned into a mall, but everyone is in a relationship with the wrong person! You must use your break up gun to break couples up, then use your get together gun to... well... get them together!

In this game jam, I was in charge of making a system that keeps track of the healthy couples and toxic couples in the game. 

Initialization
-
  ![alt text](<couple manager initialize.png>)

 The couple manager is initialized by putting our preassigned couples into a map. It just adds every person and their partner, so everyone will show up once as a key and once as a value. That is why when Num of Toxic Couples is set (for the UI display), it takes the length of the couples array and divides by 2. 
 
 <!-- 
{{< accordion mode="open" separated=true >}}
  {{< accordionItem title="Initialization" icon="code" open=false >}} -->



  <!-- {{< /accordionItem >}}
{{< /accordion >}}

 -->




 Couple Breakup
-

  ![alt text](<breakup 1.png>)

  To get couples together, we must break them up.  
We first make sure a person is within a couple in the first place. 
If they are, we get their partner and assign them to the Ex variable. 
We then remove both couple instances from the array. Fly away then pushes them apart.


  ![alt text](<breakup 2.png>)

  We then see if they were in a healthy relationship or not so we can see if we should subtract from the healthy couples number or toxic couple number. We then set the variables to indicate that they are single now. 

  Couple Assign
  -

![alt text](<couplemanager_1.png>)
![alt text](<couplemanager_2.png> "logic for adding couple to array")
But now we get to DTR. We set the suitor, then we check if they’re suitable to be matched with someone. Then if the suitor is the first person we’re matching together, we assign him to Waiting Pawn. 
If it’s the second person we shoot, we then add them to the couples array.




        

![alt text](<couplemanager_3.png>)
We then change the sprites depending on if they’re in a healthy or toxic relationship. Then we update the couple numbers again.
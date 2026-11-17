---
title: "Lantern of Souls"
showDate: false
showWordCount: false
showReadingTime: false
---


What does our camera do?
-


[Jump to Player Blueprints](#contributions-to-the-player-blueprint)


Lantern of Souls is our current senior capstone, and I was tasked with designing the custom camera manager that fit our needs.
Our camera is a dynamic 3rd person camera with our player on the third, and his soul to his side.
The camera needed to be able to handle:


- Smoothly springarming against walls but not smaller objects
- switching between our dynamic camera to fixed cameras
- switching to first person within cracks, and allowing the player to look around smoothly
- switching to cinematics such as dramatic quick time events


 Design of Springarm
 -


  ![alt text](<new springarm.gif>)


The purpose of our spring arm is to prevent the camera from clipping through walls, pushing the camera closer or towards the center.
It currently uses three line traces: one from the player's head to the camera, one from the camera to straight down, and one from the player to the right vector.


The ray shooting from the head will push the camera to the ray's collision point, but the ray shooting right from the player will push the camera to the center of the player if it collides.


{{< accordion mode="open" separated=true >}}
  {{< accordionItem title="Code Breakdown" icon="code" open=false >}}
  <img src="/images/raytrace1.png" alt="raytrace">

  <img src="/images/raytrace2.png" alt="raytrace">

  <img src="/images/raytrace3.png" alt="raytrace">


    The linetrace collision for the springarm is divided into 3 parts.
- The first linetrace is to see the location of the wall hit. The trace starts above the player's head. this is done by getting the actor location and adding (15, 0, 150) to move it above the players head
The trace ends 300 centimeters behind the player by subtracting (300, -90, -35) to the actor location and plugging that into the end.


    The outhit location is then used to make a new camera offset. I get the actor location, subtracted that by the outhit location to get the length, I then subtract it by 12,12,12 to make the location land before the wall. (if you get the exact location the camera clips through). I unrotate the vector and that is our new desired camera offset.


- The second linetrace shoots down from where the camera normally would be without collision, and that sees if there is a floor. If there isn’t a floor, it spring arms. This method is planned to change in the future when we implement tags.
- The third linetrace shoots off to the right to prevent the camera from clipping to the right side of a wall. When the line collides, it brings the camera to the center of the player.






<!--


  This item demonstrates Markdown rendering:
  - **Bold text**
  - Lists
  - `inline code` -->
  {{< /accordionItem >}}
{{< /accordion >}}




Camera States
-
My camera switches on an enum between various states such as Looking, Fixed Camera, Peek left, Peek Right, Crack Look, and a death cutscene similar to fixed camera. This system has been easy for my team to follow along and adjust elements if needs be.

<img src="/images/camera states.png" alt="camera state">
<img src="/images/camera states 2.png" alt="camera state">


{{< accordion mode="open" separated=true >}}
  {{< accordionItem title="Looking" open=false >}}
  ![alt text](<walking camera3.png>)
    Normal Looking is how the looking camera typically works. The desired camera offset changes continuously depending on collisions with walls. If the desired camera offset is different from the actual camera offset, the actual offset will lerp to the desired camera offset.This accounts for changes with wall collisions


Lerp into normal looking is how the looking camera works if we need to interpolate into the looking camera. This happens if we need to transition from another camera mode like Fixed Camera.


  {{< /accordionItem >}}
{{< /accordion >}}






{{< accordion mode="open" separated=true >}}
  {{< accordionItem title="Fixed" open=false >}}
  <img src="/images/fixed camera.png" alt="fixed camera">
  
  ![alt text](<fixed camera.gif>)


This is a camera that stays in a set position. You can choose to lerp to it or not. Typically used for interactables or cutscenes.


It takes in an arrow within interactables and sets that to the desired camera location and rotation.


![alt text](<fixed arrow.png>)


![alt text](<enter fixed camera.png>)
Above is a function that my team is able to easily call and plug in an arrow any time they make a new interactable. They are also able to decide if they want to lerp to the camera or cut to the camera.
  {{< /accordionItem >}}
{{< /accordion >}}






{{< accordion mode="open" separated=true >}}
  {{< accordionItem title="Peek Left/ Peek Right" open=false >}}
  ![alt text](<peek.gif>)
 
  Peek left and peek right shift the camera to either side and allows the player to look around corners. This adds to the stealth a player can utilize. Q allows the player to peek to the left, and E allows the player to peek to the right. Those inputs are handled in the game controller.
  <img src="/images/peek code.png" alt="peek code">
Peek left and peek right simply lerp the actual camera offset to a position that is off to the side. It takes the current desired camera location and adds to the Y for peek left and subtracts for peek right. If there is a wall collision that is detected in the Camera orient however, it will interpolate off to the side of the collision location.


  {{< /accordionItem >}}
{{< /accordion >}}






{{< accordion mode="open" separated=true >}}
  {{< accordionItem title="Crack Look" open=false >}}
  ![alt text](<cracklook.gif>)
  Crack look started off as a fixed camera that would just switch to peek left and right, which were more fixed cameras. However we wanted the experience in the crack to be more interactive. This is first done by letting the player look around the crack with their mouse.


  ![alt text](<qte.gif>)
Then with the quick time event, we wanted something dynamic, which the camera movements had to match.
The player is able to look around the corners with the mouse, when the enemy is around the area, and decides to look inside the crack, a quick time event starts.


  <img src="/images/crack look.png" alt="crack look">
  This is the flow of logic within the blueprint update camera. The structure here is similar to the other walking cameras, however instead of camera orient, we have Enter Crack Look, which handles the core logic differently.






  <img src="/images/enter crack look.png" alt="crack look">
  Enter Crack look can actually be called within the crack logic. The original rotation is the base rotation for the rest of the logic, and the crack forward vector is plugged into it. We set it to the crack Rotation so that this separate rotation can be plugged into the function in the Blueprint update camera.


Normal Crack look around
-

  <img src="/images/qte not active.png" alt="crack look">
  
  When the QTE is not active, we have a view where you can look around with your mouse. I grab the crack rotation and use the delta rotator to get the difference between the original crack rotation and the control rotation. If the distance is greater than 10, it sets the desired camera offset to face the left, and if it's less than -10 it goes to the right. If it’s between those values, it pulls back in the center.
The input mouse is there, because if the mouse is fast enough, it won’t pull back to the center.



  
  <img src="/images/change view.png" alt="crack look">
Afterwards, I set the view max and mins for the pitch and yaw. This locks how far left/right, and up/down you can go with your mouse.


When QTE active
-

<img src="/images/qte active.png" alt="crack look">

  When there's a QTE, there’s one more boolean for QTE dodging, which happens when the player dodges after successfully doing the QT task.
When QTE dodging is active, the QTE direction is chosen by random in the GI_latern, which determines what side of the crack the camera will shift to.




  {{< /accordionItem >}}
{{< /accordion >}}




Contributions to the player blueprint
-
Our player has a soul companion that aids him throughout the level. With that being said, one of the jobs of the soul is to alert the player that a guard is nearby. When a guard is nearby, the soul will hide by the player.




 ![alt text](<soul locations.gif>)
The soul will hide in different sides of the player, specifically  on the opposite side of where the guard is. The sudden movement of the soul alerts the player that something is happening, then the side he's on helps the player think about their next move.




 ![alt text](<soul wall.gif>)


We were having an issue where the soul would clip into the wall on the right side, so I also made it so where if a linetrace collides on a wall, the soul will move closer to the player.


However, since both of these handle the soul location, I had to incorporate both of them into my logic.


{{< accordion mode="open" separated=true >}}
  {{< accordionItem title="Code Breakdown" icon="code" open=false >}}



 <img src="/images/soul_placement.png" alt="soul placement">
I first have a custom event that changes the desired soul position, which is used throughout the code.


  <img src="/images/soul_placement1.png" alt="soul placement">
We then check if the soul is in a cautious state, and if they are not crouching. (crouching has one set location)




 <img src="/images/soul_placement6.png" alt="soul placement">

  <img src="/images/soul_placement2.png" alt="soul placement">


 If those are true, we then check which side the guard is relative to the player(Thiago). The soul has a system that points to the closest guard, so we take the rotation of the soul and subtract that from the front vector of the guard to see what angle the soul is relative to the player. We then take the z float and see which float range it is within, using the soul placement custom event when one of them is true.  
 
  <img src="/images/soul_placement3.png" alt="soul placement">
 
 If the soul is in a cautious state, it will then quiver in fear using some sine and cosine waves that I am adding to the desired location.

 
  <img src="/images/soul_placement4.png" alt="soul placement">
 If the soul is not in a cautious state, it will send a linetrace to the right to see if there is a collision. and then lerps next to the player.




  {{< /accordionItem >}}
{{< /accordion >}}





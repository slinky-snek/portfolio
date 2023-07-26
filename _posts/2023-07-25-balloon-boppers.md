---
title: "Balloon Boppers"
last_modified_at: 2023-07-11T16:20:02-05:00
categories:
  - Games
tags:
  - Balloon Boppers
---

Balloon Boppers is a 2 to 4-person split-screen arena shooter in which the players are continuously falling. The players can only move around the arena using a grappling hook.
To win, the player must have the most kills by the end of the timed match. Along with fighting others, players must stay in the arena to stay alive. 

Developed in Unreal Engine 5 by myself and one artist.

<iframe width="560" height="315" src="https://www.youtube.com/embed/j3D1zjrDhuQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
<br>
<iframe frameborder="0" src="https://itch.io/embed/2166883" width="560" height="167"><a href="https://twixel.itch.io/balloon-boppers">Balloon Boppers by twixel</a></iframe>

Highlights:
<h2>
  Projectile Gameplay Ability
</h2>
<iframe src="https://blueprintue.com/render/c9gumui_/" scrolling="no" allowfullscreen width="800" height="400"></iframe>
<br>
<br>
  *  I implemented the shooting mechanic using Unreal's Gameplay Ability System. This allowed for quick iteration of possible pickups and more complex interactions with other mechanics.
<br>
<br>
<br>
<h2>
  Automatic Grapple Target Selection
</h2>
<iframe src="https://blueprintue.com/render/dh04g-nc/" scrolling="no" allowfullscreen width="800" height="400"></iframe>
<h2>
  Grapple Update
</h2>
<iframe src="https://blueprintue.com/render/5xjbls4h/" scrolling="no" allowfullscreen width="800" height="400"></iframe>
<br>
<br>
  * I implemented an assist system for the grapple mechanic. The system finds the best location for the players grapple point nearest to where the player is aiming. This allowed grappling on controller to be much more accessible and fun. The player was able to focus more on where they wanted to move using the grapple and not on constantly trying to hit grapple targets precisely.
  <br>
  <br>
  <br>
<h2>
  Aim Assist Component (Main Graph)
</h2>
<iframe src="https://blueprintue.com/render/b2cdxey4/" scrolling="no" allowfullscreen width="800" height="400"></iframe>
<h2>
  Target Gravity Subgraph
</h2>
<iframe src="https://blueprintue.com/render/p8gc2wi0/" scrolling="no" allowfullscreen width="800" height="400"></iframe>
<h2>
  Sticky Targets Subgraph
</h2>
<iframe src="https://blueprintue.com/render/7hsxg3cr/" scrolling="no" allowfullscreen width="800" height="400"></iframe>
<br>
<br>
  *  I implemented aim assist systems including bullet magnetism, area cursor, sticky targets, and target gravity to test which would work best for the game. I ended up settling on bullet magnetism and target gravity. With constantly moving targets, the sticky target approach would actually cause players to not be able to reach their target while trying to track them. This is because the player's input is slowed when aiming near the target. For targets that don't move much or stop intermittenly, this might be a good approach. Area cursor gave the player a feeling that they were hitting targets by cheating. The bullet magnetism approach worked quite well without players even knowing it was being used. Bullet magnetism gave players an increased chance that their shot would hit even with their target moving at high speeds. Players are moving quickly around the arena and tracking on controller was too difficult even for experienced controller players. With the aim assist strategies above, all players were able to accurately score hits on others without feeling they were being aided too heavily.
<br>
<br>
<br>    
  *  I implemented all sound effects and music using MetaSounds. This allowed for greater control over when and how sounds were played. Included randomized music selection and pitch scaling of sound effects. Fewer sound effects were needed and each time one was played, it sounded different. This was especially important for the shooting, grappling, and impact sounds. Being able to make a metasound as a placeholder was also very helpful. You can play the empty metasound where you need it while building the mechanic. When you have a sound that you've picked later on, instead of having to find the call-site where you played the sound, you can edit the metasound asset.

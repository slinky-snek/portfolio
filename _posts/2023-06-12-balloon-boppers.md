---
title: "Balloon Boppers"
last_modified_at: 2023-06-30T16:20:02-05:00
categories:
  - Games
tags:
  - Balloon Boppers
---

Balloon Boppers is a 2 to 4 person split screen arena shooter in which the players are continuously falling. The players can only move around the arena using a grappling hook.
To win, the player must have the most kills by the end of the timed match. Along with fighting others, players must stay in the arena to stay alive. 

Developed in Unreal Engine 5 by myself and one artist.

Available on <a href="https://twixel.itch.io/balloon-boppers">Itch.io</a>

<iframe width="560" height="315" src="https://www.youtube.com/embed/j3D1zjrDhuQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>



Highlights:

  * Implemented the shooting mechanic using Unreal's Gameplay Ability System. This allowed for quick iteration of possible pickups and more complex interactions with other mechanics.
  * Implemented an assist system for the grapple mechanic. The system finds the best location for the players grapple point nearest to where the player is aiming. This allowed grappling on controller to be much more accessible and fun. The player was able to focus more on where they wanted to move using the grapple and not on constantly trying to hit grapple targets precisely.
  * Implemented aim assist systems including bullet magnetism, area cursor, and target gravity. Players are moving quickly around the arena and tracking on controller was too difficult even for experienced controller players. With the aim assist strategies above, all players were able to accurately score hits on others without feeling they were being aided too heavily.
  * Implemented all sound effects and music using MetaSounds. This allowed for greater control over when and how sounds were played. Included randomized music selection and pitch scaling of sound effects. Fewer sound effects were needed and each time one was played it sounded different. This was especially important for the shooting, grappling, and impact sounds.

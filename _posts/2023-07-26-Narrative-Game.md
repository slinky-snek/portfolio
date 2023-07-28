---
title: "Narrative Game - Dialogue Prototype"
last_modified_at: 2023-07-26T16:10:02-05:00
categories:
  - Games
  - Prototypes
tags:
  - Narrative
  - Dialogue
---

Here are some snippets from work I was doing in late 2022 on a prototype for a narrative driven game. The dialogue is driven by a behavior tree. Using markup
that was written into the dialogue, I was able to parse the text as it was being printed and trigger animations and sounds at specific times in the conversation. The markup 
I devised used a delimiter that would signify the next characters were to be used as animation or sound cues. The characters would be used to select specific animations
and sounds that were mapped to those characters. The system is messy and could use some cleanup. For instance, the markup should be changed from mapping individual characters to mapping
whole strings. This would make is easier to remember what the markup means instead of having a character "cheat sheet" of sorts. Also, the simple parser is still quite messy inside the behavior tree display line
task and could be moved to its own task or a separate blueprint function library.
<br>
<iframe width="560" height="315" src="https://www.youtube.com/embed/Bg-4q3sLqZk" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>
<br>
<h2>
Snippet from Player Character
</h2>
<iframe src="https://blueprintue.com/render/7vtn8w34/" scrolling="no" allowfullscreen width="700" height="400"></iframe>
<h2>
Snippet from NPC
</h2>
<iframe src="https://blueprintue.com/render/ekv3zpdk/" scrolling="no" allowfullscreen width="700" height="400"></iframe>
<h2>
Snippet from Display Line Task
</h2>
<iframe src="https://blueprintue.com/render/exfnfvmv/" scrolling="no" allowfullscreen width="700" height="400"></iframe>
<h2>
Snippet from Dialogue Emotion Handler
</h2>
<iframe src="https://blueprintue.com/render/ihc00z_y/" scrolling="no" allowfullscreen width="700" height="400"></iframe>

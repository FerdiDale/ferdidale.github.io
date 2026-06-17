---
layout: project
title: Get Lost
platforms: Android
duration: 2 months
tools: Java, LiquidFun
role: Game Designer and Programmer
video: /videos/GetLostTrailer.mp4
thumbnail: /images/GetLostCover.jpg
link: https://ferdidale.itch.io/get-lost
status: Released
technologies:
  - Procedural generation
  - Layout consistency
  - Object pooling
  - Accelerometer input handling
order: 2
---

<div>
    <h1>Relevant contributions</h1>
    <h2>Procedural generation</h2>
    <p>The core of this game resides in the <b>procedurally generated maze</b>. Specifically, three aspects of the maze are generated:
     <ul>
      <li>The <b>base layout</b></li>
      <li>The <b>static obstacles</b></li>
      <li>The <b>dynamic obstacles</b></li>
    </ul>
    </p>
    <p>To enable the use of complex obstacles in a sensible positioning in relation to the surrounding environment, I decided to build the maze as a composition of simple square rooms I will refer to as <i>ChunkPieces</i>.
    The possible layouts for each <i>ChunkPiece</i> have been chosen so that it is impossible to generate a closed shape. This guarantees the maze generated will be fully explorable. The specific shapes I chose can be seen in this note I took early on in the design phase:
    <img src="{{ site.baseurl }}/images/notes/wfc_shapes.jpg">

      
    
    </p>
</div>
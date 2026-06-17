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
    The possible layouts for each <i>ChunkPiece</i> have been chosen so that it is impossible to generate a closed shape. This guarantees the maze generated will be fully explorable. The specific shapes I chose can be seen in this note I took early on during the design phase:
    <img src="{{ site.baseurl }}/images/notes/wfc_shapes.jpg">    
    </p>
    <p>The main algorithm generating <i>ChunkPieces</i> is based on <b>Wave Function Collapse</b> and works on a grid of <i>ChunkPieces</i> I will refer to as <i>Chunk</i>. WFC works associating a subset of possible states to each tile and choosing them a single state in this subset randomly (<i>collapsing the tile</i> in it). Each state is characterized by local constraints that are used to update the possible states for the adjacing tiles, treating thus the procedural generation like a Constraint Satisfaction Problem.
    </p>
    <p>
     Tipically the constraints are propagated forward: whenever a tile is collapsed into one state, its costraint are propagated to the adjacing tiles.
     In this game the layout of a <i>Chunk</i> is conditioned by the layout of the adjacing <i>Chunks</i>, thus using a forward propagation approach during the creation of a <i>Chunk</i> would mean creating the adjacing <i>Chunks</i> beforehand. This could lead to accumulation of a lot of useless data, if the player never visits those "phantom" <i>Chunks</i>.
    </p>
    <p>
     Therefore, I decided to use a backward propagation approach: each time the algorithm wants to assign a state to a tile, it checks for the states of the adjacing tiles and updates consequently its possible states. In this way there is no need to create extra data and the system only takes care of the "real" <i>Chunks</i>.
    </p>
</div>
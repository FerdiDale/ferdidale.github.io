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
    <h3>The base layout</h3>
    <p>To enable the use of complex obstacles in a sensible positioning in relation to the surrounding environment, I decided to build the maze as a composition of simple square rooms I will refer to as <i>ChunkPieces</i>.
    The possible layouts for each <i>ChunkPiece</i> have been chosen so that it is impossible to generate a closed shape. This guarantees the maze generated will be fully explorable. The specific shapes I chose can be seen in this note I took early on during the design phase:
    <img src="{{ site.baseurl }}/images/notes/wfc_shapes.jpg">    
    </p>
    <p>The main algorithm generating <i>ChunkPieces</i> is based on <b>Wave Function Collapse</b> and works on a grid of <i>ChunkPieces</i> I will refer to as <i>Chunk</i>. <b>WFC</b> works associating a subset of possible states to each tile and choosing them a single state in this subset randomly (<b>collapsing the tile</b> in it). Each state is characterized by local constraints that are used to update the possible states for the adjacing tiles, treating thus the procedural generation like a Constraint Satisfaction Problem.
    </p>
    <p>
     <b>Tipically the constraints are propagated forward</b>: whenever a tile is collapsed into one state, its costraint are propagated to the adjacing tiles.
     In this game the layout of a <i>Chunk</i> is conditioned by the layout of the adjacing <i>Chunks</i>, thus using a forward propagation approach during the creation of a <i>Chunk</i> would mean creating the adjacing <i>Chunks</i> beforehand. This could lead to accumulation of a lot of useless data, if the player never visits those "phantom" <i>Chunks</i>.<br>
     Therefore, <b>I decided to use a backward propagation approach</b>: each time the algorithm wants to assign a state to a tile, it checks for the states of the adjacing tiles and updates consequently its possible states. In this way there is no need to create extra data and the system only takes care of the "real" <i>Chunks</i>.
    </p>
    <h3>The static obstacles</h3>
    <p>As for the static obstacles, my goal was to place spikes and bumpers in a sensible but not repetitive way, to guarantee an entertaining experience. I drew inspiration from the deeply documented procedural generation system used in <b>Spelunky</b>, specifically the algorithm generating the rooms' layout. Each room in <b>Spelunky</b> has a number of different room templates to choose from and each one of these is a grid of tiles represented by characters.<br>
    Certain characters represent <b>static tiles</b>: if that character is in a certain position, the corresponding tile will be there. Other characters represent <b>probabilistic tiles</b>: if that character is in a certain position, there is a certain probability the corresponding tile will be there.
    This guarantees that the same room template will never appear as the same room to the player and was exactly what I was looking for.
    Additional informations about the (quite more complex) system used by <b>Spelunky</b> can be found <a href="https://tinysubversions.com/spelunkyGen2/">here</a>.
    </p>
    <p>I then wrote a template for each of the possible layouts for a <i>ChunkPiece</i>, obtaining JSON files like this one describing the T shaped layout:
    <pre><code>{
   "layout": [[1,1,1,1,1,1,1,1,1,1,1,1],
              [1,1,1,1,1,1,1,1,1,1,1,1],
              [1,1,1,1,1,1,1,1,1,1,1,1],
              [2,2,2,2,2,2,2,2,2,2,2,2],
              [0,0,0,0,0,0,0,0,0,0,0,0],
              [0,0,3,0,0,0,0,0,0,3,0,0],
              [0,0,0,0,0,0,0,0,0,0,0,0],
              [0,0,0,0,0,3,3,0,0,0,0,0],
              [2,2,2,0,0,0,0,0,0,2,2,2],
              [1,1,1,2,0,0,0,0,2,1,1,1],
              [1,1,1,2,0,0,0,0,2,1,1,1],
              [1,1,1,2,0,0,0,0,2,1,1,1]],
    "spawnPoint": true
}</code></pre>
    As you can probably imagine, "0"s and "1"s are <b>static tiles</b> (respectively empty spaces and blocks), while "2"s and "3"s are <b>probabilistic tiles</b> (respectively spikes and bumpers).
    Each <i>Chunk</i> can be in easy, medium or hard mode and the <b>probability</b> associated to the tiles is proporional to this <b>difficulty</b> property, granting more obstacles in harder <i>Chunks</i>.
    It is important to note that this <b>data driven approach</b> has been used also to <b>infer the costraints</b> associated to each layout, computed verifying which sides of a template are blocked off by "1"s.
    <div>
      <img src="{{ site.baseurl }}/images/screenshots/TShapeEasy.jpg">
      <img src="{{ site.baseurl }}/images/screenshots/TShapeMedium.jpg">
      <img src="{{ site.baseurl }}/images/screenshots/TShapeHard.jpg">
    </div>
    </p>
    <h3>The dynamic obstacles</h3>
    As for the dynamic obstacles, the procedural generation algorithm is quite simple. Each layout has a series of possible obstacles associated and based on the Chunk's difficulty, a number of them is chosen and generated. To enhance variety, each obstacle can be chosen with some <b>random characteristics</b> and is placed in the ChunkPiece randomly within a room of sensible spots. Furthermore, many obstacles exist in an <b>"automated" mode</b> or a <b>"free" mode</b>. Whereas "automated" obstacles are characterized by a motor that makes them move in a pattern, "free" obstacles move with the world's gravity, following the tilt of the player's device.
    The majority of dynamic obstacles are made of blocks and spikes connected by <b>joints</b>.
    <div>
      <img src="{{ site.baseurl }}/images/screenshots/EasyDynamic.jpg">
      <img src="{{ site.baseurl }}/images/screenshots/MediumDynamic.jpg">
      <img src="{{ site.baseurl }}/images/screenshots/HardDynamic.jpg">
    </div>
</div>
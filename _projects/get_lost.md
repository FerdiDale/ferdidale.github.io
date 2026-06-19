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
<h1>Short description</h1>
<p>
    Super Monkey Ball meets the  Bounce series by Nokia in this simple but weird arcade game for mobile focusing on the use of the accelerometer.<br><br>

    Your goal is to get as far as you can from the spawn point in a procedurally generated maze, paying attention to what you're bumping into.<br>
    You will face obstacles moving in patterns or according to physics, that you have to approach differently, if you want to get by without losing health.<br>
    If time runs out or you lose all your health it's game over, but don't worry,  you will find pickups to gain health, time or special powerups!<br><br>

    This game has been mainly developed by me, but the full list of credits can be found in-game.<br><br>
</p>
</div>
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
    The possible layouts for each <i>ChunkPiece</i> have been chosen so that it is impossible to generate a closed shape. This guarantees the maze generated will always be fully explorable. The specific shapes I chose can be seen in this note I took early on during the design phase:
    <img src="{{ site.baseurl }}/images/notes/wfc_shapes.jpg">    
    </p>
    <p>The main algorithm generating <i>ChunkPieces</i> is based on <b>Wave Function Collapse</b> and works on a grid of <i>ChunkPieces</i> I will refer to as <i>Chunk</i>. <b>WFC</b> works associating a subset of possible states to each tile and choosing for them randomly a single state in this subset (<b>collapsing the tile</b> in it). Each state is characterized by local constraints that are used to update the possible states of the adjacent tiles, treating thus the procedural generation like a Constraint Satisfaction Problem.
    </p>
    <p>
     <b>Tipically the constraints are propagated forward</b>: whenever a tile is collapsed into one state, its costraints are propagated to the adjacent tiles.
     In this game the layout of a <i>Chunk</i> is conditioned by the layout of the adjacent <i>Chunks</i>, thus using a forward propagation approach during the creation of a <i>Chunk</i> would mean creating the adjacent ones beforehand. This could lead to the accumulation of a lot of useless data belonging to "phantom" <i>Chunks</i>. This is because the number of "phantom" <i>Chunks</i> is linear to the number of <i>Chunks</i> effectively visited by the player.<br>
     Therefore, <b>I decided to use a backward propagation approach</b>: each time the algorithm wants to assign a state to a tile, its possible states are computed checking the costraints of the adjacent tiles. This way there is no need to create extra data and the system only takes care of the "real" <i>Chunks</i>.
    </p>
    <h3>The static obstacles</h3>
    <p>As for the static obstacles, my goal was to place spikes and bumpers in a sensible but not repetitive way, to provide an entertaining experience. I drew inspiration from the deeply documented procedural generation system used in <b>Spelunky</b>, specifically from the algorithm generating the rooms' layout. Each room in <b>Spelunky</b> has a number of different templates to choose from and each one of these is a grid of tiles represented by characters.<br>
    Certain characters represent <b>static tiles</b>: if that character is in a certain position, the corresponding tile will be there. Other characters represent <b>probabilistic tiles</b>: if that character is in a certain position, the corresponding tile will be there with a set probability.
    This guarantees that a single room template will never appear the same to the player and was exactly what I was looking for.
    Additional informations about the (quite more complex) system used by <b>Spelunky</b> can be found <a href="https://tinysubversions.com/spelunkyGen2/">here</a>.
    </p>
    <p>I then wrote a template for each of the possible layouts for <i>ChunkPieces</i>, obtaining JSON files like this one describing the T shaped layout:
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
    Each <i>Chunk</i> can be created in easy, medium or hard mode and the <b>probability</b> associated to the tiles is proportional to this <b>difficulty</b> property, granting more obstacles in harder <i>Chunks</i>.
    It is important to note that this <b>data driven approach</b> has been used also to <b>infer the costraints</b> associated to each layout, computed verifying which sides of a template are blocked off by "1"s.
    <div class="screenshot-getlost-images">
      <img src="{{ site.baseurl }}/images/screenshots/TShapeEasy.jpg">
      <img src="{{ site.baseurl }}/images/screenshots/TShapeMedium.jpg">
      <img src="{{ site.baseurl }}/images/screenshots/TShapeHard.jpg">
    </div>
    </p>
    <h3>The dynamic obstacles</h3>
    As for the dynamic obstacles, the procedural generation algorithm is quite simple. Each layout is associated to a series of possible obstacles and, based on the Chunk's difficulty, a number of them is chosen and generated. To enhance variety, each obstacle can be chosen with some <b>random characteristics</b> and is placed in the ChunkPiece randomly within a room of sensible spots. Furthermore, many obstacles exist in an <b>"automated" mode</b> or a <b>"free" mode</b>. Whereas "automated" obstacles are characterized by a motor that makes them move in a pattern, "free" obstacles move with the world's gravity, following the tilt of the player's device.
    The majority of dynamic obstacles are made of blocks and spikes connected by <b>joints</b>.
    <div class="screenshot-getlost-images">
      <img src="{{ site.baseurl }}/images/screenshots/EasyDynamic.jpg">
      <img src="{{ site.baseurl }}/images/screenshots/MediumDynamic.jpg">
      <img src="{{ site.baseurl }}/images/screenshots/HardDynamic.jpg">
    </div>
    <h3>Consistency and optimizations</h3>
    <p>
    The procedural generation in this game presents a key factor to be considered: a player <b>can always backtrack</b> to previously visited <i>Chunks</i> and, since we <b>can't save the whole maze</b> in memory, they have to be generated every time in a consistent manner.
    Tipically this problem is solved using a <b>set seed</b>, guaranteeing consistency in the random number generator used. Unfortunately, the algorithm for layout generation I programmed takes into account the constraints derived by adjacent <i>Chunks</i>, thus even working with a <b>set seed</b>, the <b>order</b> in which <i>Chunks</i> are explored would result in an <b>overall different maze layout</b>.
    </p>
    <p>
    To bypass this problem, each time a <i>Chunk</i> is <b>collapsed into a layout</b>, it is <b>permanently saved</b>, guaranteeing its consistency. This isn't particularly space consuming, since a layout ultimately is nothing other than an integer (or to be precise a reference to a static layout object). All the other characteristics of a <i>ChunkPiece</i>, such as <b>static and dynamic obstacles</b>, are generated each time using a <b>set seed</b>, since they aren't influenced by external factors.
    </p>
    <p>
    Furthermore, since there can be quite a few objects on screen, only the <i>ChunkPiece</i> <b>currently navigated</b> by the player and its adjacent ones are <b>rendered on screen</b> and <b>simulated</b>. Specifically, only the <b>collisions</b> in the <b>central <i>ChunkPiece</i></b> are active, since they are the only ones the player can directly interact with.
    Each time the player moves from a <i>ChunkPiece</i> to another, the ones forward are immediately built and rendered while the ones behind are set to be removed. To <b>prevent lag spikes</b>, the <i>ChunkPieces</i> are not deleted in a single frame, but instead are <b>marked and then deleted singularly</b> per frame.
    </p>
    <p>
    Finally, since this game is developed in Java, one of my goals was to keep the number of objects eligible for <b>garbage collection</b> as low as possible at all times. This, along with the need to avoid spawning large numbers of identical game objects such as blocks, spikes, and bumpers, led to the extensive use of <b>object pooling</b>.
    </p>
</div>
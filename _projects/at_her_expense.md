---
layout: project
title: At Her Expense
platforms: Windows
duration: 6+ months
tools: Unreal Engine 5
role: Game Designer and Programmer, Technical Artist
video: /videos/AtHerExpenseTrailer.mp4
thumbnail: /images/AtHerExpenseCover.png
link: https://ferdidale.itch.io/at-her-expense
status: Released
technologies:
  - Interaction system
  - Dialogue system
  - Quest system
order: 3
---
<div>
    <h1>Relevant contributions</h1>
    <h2>Interaction system</h2>
    <p>The gameplay revolves around exploring and interacting with the environment. I programmed a classic interaction system based on <b>raycasting</b>: if the line traced from the player's camera hits an interactable object, it gets highlighted and the proper widget is shown to the player.<br>
    The specific objects implement the interaction methods, declared in a common abstract superclass.
    </p>

    <div class="video-row">
      <video
        class="content-video"
        muted
        loop
        autoplay
        playsinline
        preload="metadata">
        <source
              src="/videos/AtHerExpense/AtHerExpense_TrashCanPushing.webm"
              type="video/webm">
      </video>
      <video
        class="content-video"
        muted
        loop
        autoplay
        playsinline
        preload="metadata">
        <source
              src="/videos/AtHerExpense/AtHerExpense_RingThrowing.webm"
              type="video/webm">
      </video>
    </div>
    <p>
  The interactions are quite diverse: they range from simple <b>physics-based actions</b>, such as pushing or throwing an object, to triggering distinct subsytems.
    Among the latter, the most interesting ones are a <b>real-time painting system</b> and a <b>simulated computer interface</b>.
    </p>

    <div class="video-row">
      <video
        class="content-video"
        muted
        loop
        autoplay
        playsinline
        preload="metadata">
        <source
              src="/videos/AtHerExpense/AtHerExpense_CanPainting.webm"
              type="video/webm">
      </video>
      <video
        class="content-video"
        muted
        loop
        autoplay
        playsinline
        preload="metadata">
        <source
              src="/videos/AtHerExpense/AtHerExpense_PCInteraction.webm"
              type="video/webm">
      </video>
    </div>

    <p>
  The painting system has been implemented drawing the <b>spray mark texture</b> on a <i>Render Target</i> iteratively. The <i>Render Target</i> is used as a <i>dynamic texture parameter</i> in the wall's material. The position where to draw the spray mark texture gets computed using line tracing on the wall and storing the <b>UV of the trace hit</b>.
  This flexible system allows obtaining <b>different painting effects</b> solely changing the mark texture; for instance in the game you can use a sponge to erase what you have drawn and it works by "painting" with a white square texture.
Therefore <b>this system could be easily extended to use any colors or brush effect</b>.
    </p>
    <p>
  For the computer interface I designed a class hierarchy rooted in a <i>BlankFile</i> widget class, where I implemented behaviours common to all types of files. The content of the file is displayed using a <i>Window</i> widget class, that reacts to the type of file opened by the user.
  For the sake of simplicity I avoided defining folders, since that would have required drawing overlapping windows and managing input on multiple layers. I decided to keep it simple, making the <b>windows non-blocking</b>, but <b>replacing the single window</b> on screen when a player opens multiple files in a row.
  The initial arrengement of the files and their content are defined in a <b>DataTable</b>, in line with the <b>data-driven approach</b> used extensively throughout the game.
    </p>

<h2>Quest system</h2>
    <p>Throughout the game the tasks to complete are displayed on the contract on screen, that functions as a diegetic UI. My first programming task was to design the quest system, which had to support notably:
   <ul> 
    <li>Tasks consisting of <b>repeating any type of action <i>n</i> times</b></li> 
    <li>Groups of <b>tasks that appear simultaneously</b> on the contract</li> 
    <li><b>Dynamic task sequences</b> that change based on the player's behavior</li> 
   </ul>
    Based on these requirements, I designed the task system as a <b>tree data structure</b>, since it is impossible to return to previously completed tasks. Specifically I envisioned a tree similar to a <b>Finite State Machine</b>, composed of<b> <i>TaskNodes</i> linked by guarded transition arcs</b>.<br>
    I stored all the data describing the task system in two <b>DataTables</b>, one for the tasks and the other for the transitions, where the tasks are grouped in <i>TaskNodes</i>. Each task is characterized by an <i>ObjectiveID</i>, whose purpose I will describe later, and by the number of repetitions required. Each transition is characterized by its source and target <i>TaskNodes</i>, the presence and type of guard (time-based, based on the <i>Evilness</i> value, or based on a <i>GameplayTag</i>), and the values associated with the guard's condition.
    </p>
    <p>When the Contract is spawned, a <i>ContractTreeComponent</i> scans the <b>DataTables</b> and builds the corresponding tree, rooted at the initial <i>TaskNode</i>. The flexibility of this system allowed us to modify the task sequence throughout the game's development without requiring changes to the underlying code.</p>
    </p>
   <p>To support task completion, I developed a scalable system capable of handling every type of objective. I assigned an <i>ObjectiveID</i> to each task and bound every Task object to an <b>event dispatcher</b> in the <i>ContractTreeComponent</i>. Whenever the player performs a relevant action, the corresponding interactable object invokes this dispatcher, passing the <i>ObjectiveID</i> associated with that action.</p>

  <p>Each Task object compares the received <i>ObjectiveID</i> with its own and, if they match, processes the "signal". Every Task object keeps track of how many matching "signals" it has received, and once the required number is reached, the task is marked as complete.</p>

  <p>This system was also effortlessly extended to support <b>reverting task progress</b> by decrementing the stored "signals" count. This feature is used by the main <b>NPC to interfere with the player's progress</b> by performing the opposite of what the contract requires.</p>

  <p><b>The architecture could be easily expanded to support dependencies between tasks or objectives composed of multiple sequential stages</b>, making it suitable for implementing the <b>classic quest systems commonly found in role-playing games</b>.</p>


  <h2>Dialogue system</h2>
    <p>The game begins and ends with sections inspired by <b>visual novels</b>, where the player can "hear" the higher beings involved speaking. Although the player cannot respond to their interlocutor, the system I developed still includes a <b>dynamic component</b>: in the final section, the <b>dialogue varies based on how the player has behaved</b> throughout the game.<br>
    The dialogue is divided into <i>sections</i>, each identified by a <i>tag</i> and linked to the <i>tag</i> of the following <i>section(s)</i>. The dynamic aspect comes from <b>associating each section with a set of conditions</b>. This allows<b> multiple sections</b> to share the <b>same tag</b>, with the<b> appropriate one</b> being <b>selected at runtime</b> based on the game state.
    These conditions are based on two variables: the <i>Evilness</i> value and the <i>Progress</i> value. The former is calculated by summing the evilness of the player's actions, while the latter is determined by the number of tasks the player has completed. This system enables<b> different dialogue branches and endings</b> depending on the player's behavior.
    All these informations are stored in a <b>DataTable</b>, in line with the <b>data-driven approach</b> used extensively throughout the game. The data structure for the dialogue is much <b>simpler than the one used for the tasks</b>, as it is <b>based on static values</b>. Once the player reaches the final section, the dialogue can no longer change, because the relevant <b>values</b> have <b>already been determined</b>. As a result, the <b>dialogue structure is not a tree but rather a set of parallel sequences</b>, with the appropriate one being selected when the player reaches the end of the game.
    </p>

  
</div>
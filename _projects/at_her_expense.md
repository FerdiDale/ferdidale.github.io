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
    <video
      class="project-video"
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
      class="project-video"
      muted
      loop
      autoplay
      playsinline
      preload="metadata">
      <source
            src="/videos/AtHerExpense/AtHerExpense_RingThrowing.webm"
            type="video/webm">
    </video>
    <p>
  The interactions are quite diverse: they range from simple <b>physics-based actions</b>, such as pushing or throwing an object, to triggering distinct subsytems.
    Among the latter, the most interesting ones are a <b>real-time painting system</b> and a <b>simulated computer interface</b>.
    </p>
    
    <video
      class="project-video"
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
      class="project-video"
      muted
      loop
      autoplay
      playsinline
      preload="metadata">
      <source
            src="/videos/AtHerExpense/AtHerExpense_PCInteraction.webm"
            type="video/webm">
    </video>
    <p>
  The painting system has been implemented drawing the <b>spray mark texture</b> on a <i>Render Target</i> iteratively. The <i>Render Target</i> is used as a <i>dynamic texture parameter</i> in the wall's material. The position where to draw the spray mark texture gets computed using line tracing on the wall and storing the <b>UV of the trace hit</b>.
  This flexible system allows obtaining <b>different painting effects</b> solely changing the mark texture; for instance in the game you can use a sponge to erase what you have drawn and it works by "painting" with a white square texture.
  Therefore this system could be easily extended to use any colors or brush effect.
    </p>
    <p>
  For the computer interface I designed a class hierarchy rooted in a <b>BlankFile</b> widget class, where I implemented behaviours common to all types of files. The content of the file is displayed using a <b>Window</b> widget class, that reacts to the type of file opened by the user.
  For the sake of simplicity I avoided defining folders, since that would have required drawing overlapping windows and managing input on multiple layers. I decided to keep it simple, making the <b>windows non-blocking</b>, but <b>replacing the single window</b> on screen when a player opens multiple files in a row.
  The initial arrengement of the files and their content are defined in a <b>DataTable</b>, in line with the <b>data-driven approach</b> extensively used in the game.
    </p>
</div>
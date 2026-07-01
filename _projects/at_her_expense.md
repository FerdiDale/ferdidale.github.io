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
    <p>The gameplay revolves around exploring and interacting with the environment. I programmed a classic interaction system based on <b>raycasting</b>: if the line traced from the player's camera hits an interactable object, it gets highlighted and an appropriate widget is shown to the player.<br>
    The specific objects implement the interaction methods, declared in a common abstract superclass.
    </p>
    <video
      class="project-video"
      muted
      loop
      playsinline
      preload="metadata">

        <source
            src="/videos/AtHerExpense/AtHerExpense_TrashCanPushing.mkv"
            type="video/mkv">
    </video>
    <p>
    The interactions are quite diverse: they range from simple physics-based actions, such as pushing or throwing an object, to triggering distinct subsytems.
    Among the latter, the most interesting ones are a real-time painting system and a simulated computer interface.
    </p>
    <p>
    The painting system has been implemented using the 
    </p>
</div>
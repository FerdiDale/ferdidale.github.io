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
      playsinline
      preload="metadata"
      src="/videos/AtHerExpense/AtHerExpense_TrashCanPushing.mkv">
    </video>
    <p>
    The interactions are quite diverse: they range from simple physics-based actions, such as pushing or throwing an object, to triggering distinct subsytems.
    Among the latter, the most interesting ones are a real-time painting system and a simulated computer interface.
    </p>
    <p>
  The painting system has been implemented drawing the spray mark texture on a <i>render target</i> iteratively. The <i>render target</i> is used as a <i>dynamic texture parameter</i> in the wall's material. The position where to draw the spray mark texture gets computed using line tracing on the wall and storing the UV of the trace hit.
  This flexible system allows obtaining different painting effects solely changing the mark texture; for instance in the game you can use a sponge to erase what you have drawn and it works by "painting" with a white square texture.
  Therefore this system could be easily extended to use any colors or brush effect.
    </p>
</div>
---
layout: post
title:  "First steps adding interactivity"
categories: [Hampden, jquery]
---

# First steps with scripting

Now I have the skeleton of the site tigether I can look at the buttons required to navigate through the game and add the script required for the buttons to work.

A simple piece of jquery will detect a click from our button element and we can then hide the current game screen and button and display the next.

```
$('.startBtn').click(function() {
$('.thisBtn,.thisScreen').hide();
$('.nextBtn,.nextScreen').show(); 
});
```

In the skeleton I added the welcome, teams, rounds, final, winner and loser screens for each stage of the game. The rounds screen is the only screen which will require more buttons as this is where the game action will take place. For this screen I will need a button to make the draw for the round, play the matches and finally to move to the next round. So I can now set up the buttons required to move between them.

```
<button class="startBtn" style="">Get started...</button>
<button class="playBtn" style="display: none;">Play Hampden...</button>
<button class="drawBtn" style="display: none;">Make draw...</button>
<button class="roundBtn" style="display: none;">Play <span class="roundName"></span>...</button>
<button class="nextBtn" style="display: none;">Next round...</button>
<button class="finalBtn" style="display: none;">Play Final...</button>
<button class="endBtn" style="display: none;">End game...</button>
```

With these in place the script can be added to [test the flow through the game...](https://phowie74.github.io/dev/stage2.html)

---
layout: post
title:  "Interactive match - part 1"
categories: [Hampden, html, css]
---

# Interactive match - part 1

Whilst the game is functional there is nothing to separate matches involving the players selected team from any other match in the tournament. The original intention was always to make the payers matches more interactive so they can be involved in the dice rolling process at each stage of their own match and see what their opponent is rolling too through the 90 minutes, extra time and penalties if necessary.

The first stage to this is thinking about how best to implement this in the current framework and set up the necessary html and styles ready for the additional scripting. My initial thought is to create a pop out modal window when the round loop reaches a match with the selected team so lets start be creating a simple modal window which knocks back the main game screen and highlights a new centred window with all of the match details. I found a nice simple [css only modal script on codepen](https://codepen.io/timothylong/full/HhAer) to act as a starting point...

```
<a class="btn" href="#open-modal">CSS-Only Modal</a>

<div id="open-modal" class="modal-window">
  <div>
  
  </div>
</div>
```
With a few tweaks to the css it can fit right into the game styling...
```
.modal-window {
  position: fixed;
  background-color: rgba(255, 255, 255, 0.25);
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 999;
  visibility: hidden;
  opacity: 0;
  pointer-events: none;
  transition: all 0.3s;
}
.modal-window:target {
    visibility: visible;
    opacity: 1;
    pointer-events: auto;
}
.modal-window > div {
    width: 90%;
	max-width: 800px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 2em;
    background: #222;
}
```
Now there is a basic functional modal window in place and styled to fit in with the main game I need to consider what information needs to be included in the window to play the match. A simple title stating the round name would be a good start followed by an intro paragraph with the teams - I will continue to use placeholder spans for data coming from the teams array. Finally a sub-title indicating at what stage the current match is which can be updated as the match progresses...
```
<h2>Scottish Cup <span class="roundName"></span> Tie</h2>
<p><span class="homeTeam"></span> will face <span class="awayTeam"></span> in this exciting <span class="roundName"></span> tie at <span class="homeGround"></span>...</p>
<h3>Kick Off / Normal Time / Extra Time / Penalties / Sudden Death</h3>
```
* an new `homeGround` item can be added to the teams array to give a location venue for the match.

The match details can use the same table structure as the main game screen but with only one match...
```
<table width="100%">
<tbody>
<tr class="match">
<td width="15%"></td>
<td class="text-right home" id="0" width="25%"><span class="homeTeam"></span></td>
<td class="homeScore" width="5%"></td>
<td class="" width="10%">vs</td>
<td class="awayScore" width="5%"></td>
<td class="text-left away" id="1" width="25%"><span class="awayTeam"></span></td>
<td class="ft" width="15%"></td>
</tr>
</tbody>
</table>
```
an extra row can be added to the table to add a space for the home and away dice...
```
<tr class="match">
<td width="15%"></td>
<td width="25%">HOME DICE</td>
<td width="5%"></td>
<td width="10%"></td>
<td width="5%"></td>
<td width="25%">AWAY DICE</td>
<td width="15%"></td>
</tr>
```
Underneath the table a game status paragraph to give simple updates through the match would be useful...
```
<p class="gameStatus"><span class="homeTeam"></span> score <span class="homeScore"></span> <span class="awayTeam"></span> score <span class="awayScore"></span> You WIN!  You LOSE!</p>
```
Finally the buttons required to step through the match and end the game / close the window when the match is concluded...
```
```

A [stripped down version of the game](https://phowie74.github.io/dev/stage13.html) with only this new modal window has been set up for testing. Next I will look at the dice rolling element before integrating this into the playRound function.

<!--
PEN - Goal-Goal-Goal-Goal-Saved-Missed

Archive of winners - roll of honour on welcome screen after played first time? pick random team from array when you get knocked out as champion start back in 1986 and keep tabs of each season as you play the game
-->
---
layout: post
title:  "Interactive match - part 2"
categories: [Hampden, jquery]
---

# Interactive match - part 2

With the modal window styles and placeholder content in place I can look at stepping through the matchday experience to play the match manually. To begin with, content which is not relevant to the modal window first opening can be removed / hidden as appropriate. The variable text update areas can be set to their initial state...
```
<h3 class="gamePhase">Kick Off</h3>
...
<p class="gameStatus"></p>
```
and all button options except for the first "Kick off..." button can be hidden...
```
<p>
<button class="matchKOBtn">Kick off...</button> 
<button class="matchContinueBtn" style="display: none;">Continue...</button> 
<button class="matchRollBtn" style="display: none;">Roll die...</button> 
<a href="#" title="End game" class="btn modal-close" style="display: none;">End game...</a>
</p>
```
The remaining variable text will have previously been set by the game by the roundName and myTeam variables.

In order to bypass the rest of the game so I can focus on the modal window match play I will pass the match data to the window from the temporary open modal button. I have Aberdeen (0) as the away team and Celtic (1) as the chosen team will be the opposition. Celtic are hard coded as the chosen team in this example as we are bypassing the pick team screen...
```
<a class="btn modal" href="#open-modal">CSS-Only Modal</a>
$('.btn.modal').click( function(){
  $('.roundName').text('Quarter Final');
  $('.homeGround').text('Pittodrie');
  homeTeam = 0;
  awayTeam = 1;
  $('.myTeam, .awayTeam').text(teams[1].name);
  $('.homeTeam').text(teams[0].name);
  
});
```

The only option in the game now is to click the "Kick off..." button to get the game started. Clicking this needs to change the gamePhase and show the correct button depending on wether or not the chosen team is playing at home. The computer team will get a "Continue" button whilst the player team needs a "Roll Die" button...
```
$('.matchKOBtn').click( function(){
  $(this).hide();
  $('.gamePhase').text('Normal time...');
  var playerCheck = $('.home').attr('id');
  if (teams[playerCheck].player == 1) {
    $('.matchRollBtn').show();
    $('.gameStatus').text(myTeam + ' to kick off...');
  } else {
    $('.matchContinueBtn').show();
    $('.gameStatus').text(teams[homeTeam].name + ' to kick off...');
    // start by rolling dice for computer player
  }
});
```

For the timebeing both of these will use the team dice to generate a result as they do for auto matches and I will look at the dice rolling mechanics in the next stage.
```
$('.matchContinueBtn').click( function() {
  $(this).hide();
  $('.matchRollBtn').show();
  home90(0,hPr);
});
```
and
```
$('.matchRollBtn').click( function() {
  $(this).hide();
  $('.matchContinueBtn').show();
  away90(1,aPr);
});
```
Both the home90() function and away90() function only roll the dice and enter the score into the result table.
```
function home90(home,homeDie) {
  homeDie = dice[homeDie];
  homeScore = homeDie[Math.floor(Math.random() * homeDie.length)];
  $('.gameStatus').text(teams[home].name + ' score ' + homeScore);
  $('tr.match .homeScore').text(homeScore);
}
```
After the away90() function has rolled I can check for a winner and then it is the same match loop as for previous iterations...
```
if (homeScore > awayScore) {
  $('.gameStatus').append('<br>' + teams[0].name + ' win!!');
} else {
  $('.gameStatus').append('<br>' + teams[1].name + ' win!!');
}
``` 

This is much simplified so some work is required to combine the match dice rolling into a function where it doesn't matter if the player is home or away. My first attempt at a [pop out match] (https://phowie74.github.io/dev/stage14.html) can be tested. I need to develop the dice roll animation next to build into this game structure.

<!--
PEN - Goal-Goal-Goal-Goal-Saved-Missed

Archive of winners - roll of honour on welcome screen after played first time? pick random team from array when you get knocked out as champion start back in 1986 and keep tabs of each season as you play the game
-->
---
layout: post
title:  "Play Final"
---

# Play Final

The final is actually a simpler set up than the other rounds as there are only two teams involved so no need for a draw as such. Because I want to update the final screen content and I do not need to loop through the matches in the round I made a simplified copy of the drawRound function specifically for the final. I need to ensure the final screen content is updated when the Play Final button is clicked and the stage is set...

```
function drawFinal() {
  teams.sort(function() { return 0.5 - Math.random() }); // shuffle teams array (draw teams)
	var fixtures = '<table>';
  fixtures += '<tr class="match1">';
  var homeTeam = teams[0].name;
  fixtures += '<td width="15%"></td><td class="text-right home" id="0" width="25%">' + homeTeam + '</td>';
  fixtures += '<td class="homeScore" width="5%"></td><td class="" width="10%">vs</td><td class="awayScore" width="5%"></td>';
  var awayTeam = teams[1].name;
  fixtures += '<td class="text-left away" id="1" width="25%">' + awayTeam + '</td>';
  fixtures += '<td class="ft" width="15%"></td>';
  fixtures += '</tr>';
	fixtures += '</table>';
	$('.finalContent').html(fixtures);
}
```
Because the "home" team still technically has a slight advantage with the dice I still shuffle the teams before creating the fixture table ready for the game to be played. The Final title and `myTeam` have been filled out since the beginning of the game so the only remaining content of the final screen is to add the opponent to the text. Checking the first team to see if it has the player identifier of 1 is enough to set the opposing finalist...
```
var finalist;
if (teams[0].player == 1) { finalist = teams[1].name; } else { finalist = teams[0].name; }
$('.finalist').text(finalist);
```
Whilst I can still use the match mechanics to play the actual game (play90, playET etc) the playRound framework is set up to loop through all matches and remove losers from the teams array which is not required at this stage. Once again I can use the playRound function as a basis to create a playFinal specific function...
```
function playFinal() {
  homeDie = 'h' + teams[homeTeam].division;
	homeDie = dice[homeDie];
	awayDie = 'a' + teams[awayTeam].division;
	awayDie = dice[awayDie];
	play90(homeTeam,homeDie,awayTeam,awayDie);
	$('tr.match' + match + ' .homeScore').text(homeScore);
	$('tr.match' + match + ' .awayScore').text(awayScore);
	$('tr.match' + match + ' .ft').text(ft);
	
}
```
Now, rather than removing losers from the teams array, I need to check of the match winner was the players selected team...
```
var winner;
if ((homeScore > awayScore) || (homePen > awayPen)) { winner = 0; } else { winner = 1; }
```
Which can then decide the text which is output to the screen after the match is finished. Congratulations if you are the winner and commiserations if not. 

The default text on the final screen stating what an exciting match it is going to be is not appropriate once the final has been played so I wrapped those paragraphs in a new .finalText div so it can be replaced after the match is played...
```
<div class="finalText">
  <p><span class="myTeam"></span> have made the Final of this years' Scottish Cup</p>
	<p><span class="finalist"></span> are set to play <span class="myTeam"></span> in what is sure to be a gripping final.</p>
</div>
```

```
if (teams[winner].player == 1) {
  $('.finalScreen > h2').text('Congratulations! ' + myTeam + ' are Scottish Cup Champions');
  $('.finalText').html('<p>' + myTeam + ' have made it all the way to lift the Scottish Cup.</p>');
} else {
  $('.finalScreen > h2').text('Game Over');
  $('.finalText').html('<p>Unlucky, ' + myTeam + ' have been beaten in the final of the Scottish Cup!</p><p>Better luck next time...</p>');
}
```
Finally, I need to show the end game button which will reset everything in the game and go back to the beginning...
```
$('.endBtn').show();
```
and make sure that the final screen text is restored as part of the game reset action...
```
function clearGame() {

  $('.finalScreen > h2').text('Scottish Cup Final');
	$('.finalText').html('<p><span class="myTeam"></span> have made the Final of this years\' Scottish Cup</p><p><span class="finalist"></span> are set to play <span class="myTeam"></span> in what is sure to be a gripping final.</p>');
  
}
```

This is now a fully functional game which can be payed to completion [all the way through to winning the final](https://phowie74.github.io/dev/stage12.html). The next step will be to add some interactivity for the players chosen team - manually rolling the dice at each stage to add some excitement.

<!--
PEN - Goal-Goal-Goal-Goal-Saved-Missed

Archive of winners - roll of honour on welcome screen after played first time? pick random team from array when you get knocked out as champion start back in 1986 and keep tabs of each season as you play the game
-->
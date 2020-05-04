---
layout: post
title:  "Simplified match results"
categories: [Hampden, jquery]
---

# Simplified match results

Now the competition draw is working I can begin to look at the dice aspect of the game and start to play individual matches to see who progresses into the next round. For each round I can loop through each of the matches and using the dice for each team determine the outcome of the match. To keep things simple in the first instance I will use a single dice for all teams regardless of home advantage and division which should make for some interesting Scottish Cup winners. Looking at the original dice for the Wembley board game I set up a basic array for goals scored...

```
var dice = [0,0,1,1,2,3];
```
I can now update the playRound function to loop through each match in the round and trigger the playMatch function for each match passing through the home team, away team and match id...
```
function playRound() {
	for (var i = 1; i <= matches; i++) {
		homeTeam = $('.match' + i + ' .home').attr('id');
		awayTeam = $('.match' + i + ' .away').attr('id');
		playMatch(homeTeam, awayTeam, i);
	}
}
```
The playMatch function can then roll the dice for each team and fill in the scores in the table...
```
function playMatch(homeTeam, awayTeam, i) {
	homeScore = dice[Math.floor(Math.random() * dice.length)];
	awayScore = dice[Math.floor(Math.random() * dice.length)];
	$('.match' + i + ' .homeScore').text(homeScore);
	$('.match' + i + ' .awayScore').text(awayScore);
}
```
With the scores in place I can calculate who was the winner of the tie. Without wanting to get into the intricacies of extra time or penalties at this stage a draw will temporarily come under the banner of a home win (well home advantage has to count for something!)...

```
if (homeScore < awayScore) { loser = homeTeam; } else { loser = awayTeam; }
```
As the teams array will be reduced as the game progresses we cannot assume the array key will always be equal to the team id number. Instead I need to loop through the master teams array and remove the team when the loser id equals the team id. A simple break will stop this loop running once it has done its job...
```
for (var i = 0; i < teams.length; i++) {
  if (teams[i].id == loser) { teams.splice(i, 1); break; }
}
```
Now there are some real results involved I can detect if the chosen team has made it through to the next round. Instead of the end of the round only giving the option to move to the next round regardless of the chosen teams result I can now redirect to the loser screen if they are knocked out. This would happen at the end of the playRound function...
```
var through = false;
for (var i = 0; i < teams.length; i++) {
  if (teams[i].id == myTeamID) {
    through = true;
    break;
  }
}
if (through == false) {
  
}

```

You can [make a draw for each round](https://phowie74.github.io/dev/stage8.html) and loop through the game. This will randomly half the teams left in the competition so it will look like you win the game even though you may not even be in the draw - I will come back and fix that after I have resolved the playing matches element!


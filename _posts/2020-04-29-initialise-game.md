---
layout: post
title:  "Initialise Game Structure"
---

# Initialise Game Structure

The most important element to get set up first is the competing teams which will allow the number of rounds to be calculated as well as allowing a player to choose their team to play as. The original Wembley board game used the division a team was from to decide which dice to use during a match so we will need a spread of teams across the divisions in Scotland. The teams can be stored in an array so they can be removed as the competition progresses so they will need an id number (as the array key will change as teams are removed), team name and division. It would also be useful to store the players chosen team in the array too and this could potentially allow multiple players to play the game as part of future development...

```
teams = [
	{
  id: 1,
  name: "Celtic",
  division: "Pr",
  player: 0
  },
];
```

In order to make it easy to initiate the game on page load and also to restart the game once the player has been knocked out it makes sense to initiate this teams array within a function to be called when required... 

```
var teams;

function initialiseTeams() {
	teams = [
	{
		id: 0,
		name: "Aberdeen",
		division: "Pr",
		player: 0
	},
};
```
As a knockout competition the number of teams has to end with 2 finalists and with half the teams knocked out each round the number of teams would follow the geometric pattern of 2, 4, 8, 16, 32, 64 etc. To get things started in a simple fashion it would make sense to begin with 4 teams from the Scottish Premiership to check the round screen works and moves to the final screen when only two teams remain.
```
teams = [
  {
    id: 0,
    name: "Aberdeen",
    division: "Pr",
    player: 0
  },
  {
    id: 1,
    name: "Celtic",
    division: "Pr",
    player: 0
  },
  {
    id: 2,
    name: "Hibs",
    division: "Pr",
    player: 0
  },
  {
    id: 3,
    name: "Sevco",
    division: "Pr",
    player: 0
  }
];
```
Now we have the teams initialised we can calculate the number of teams and from that the number of rounds. The number if teams is straight forward to calculate from the size of the array and the number of rounds is also straight forward calculation of looping through the total teams and dividing by 2 each time until only 2 remain...
```
var totalTeams;
var totalRounds;

totalTeams = teams.length;
totalRounds = 1;
var i = teams.length;
do {
	totalRounds++;
  i = i / 2;
} while (i > 2);
```
The rounds at the latter stages of the competition have specific names so rather than simply stating "Congratulations you made it to Round X" we need to calculate how many rounds are remaining and name the latter stage rounds appropriately. We already know the totalRounds so we need to begin with currentRound as 1 and then increment that each round so we can calculate the difference between the currentRound and totalRounds. If there are only 2 teams there is a total of 1 round so taking away 1 currentRound from 1 totalRounds leaves 0 which would be the Final, 1 currentRound from 2 totalRounds leaves 1 which would be the Semi-Final etc...
```
var currentRound = 1;
if (totalRounds - currentRound == 0) {
  roundName = 'Final';
} else if (totalRounds - currentRound == 1) {
  roundName = 'Semi-Final';
} else if (totalRounds - currentRound == 2) {
  roundName = 'Quarter-Final';
} else {
  roundName = 'Round ' + currentRound;
}

```
Once again this needs to be included in a function so it can be called at the end of each round.

We can pull this together in the first instance to update the game screens with he current data and loop through the rounds until the final to make sure the sequence of screens is working before we add in the dice elements to play the actual matches. At this stage a simple playRound function can be added which will simply halve the teams left and call the round naming function and go to the final when the time is right!
```
$(function() { // on page load
  initialiseTeams(); // set up teams array, set currentRound to 1 and calculate totalRounds
  roundNamer(); // calculate current round and update game screens to reflect this
});

function playRound() { // simple function to move game on for testing
  currentRound++;
  totalTeams = totalTeams / 2;
}
function nextRound() { // update round screen or move to final!
  if (totalTeams == 2) {
    $('').hide();
    $('').show();
  } else {
    roundNamer();
  }
}
function restartGame() { // go back to the beginning again!
  initialiseTeams();
  roundNamer();
}

```

For the sake of this test Celtic will always be the chosen team and will always be champions!! You can [see this in action here...](https://phowie74.github.io/dev/stage5.html) and can continually loop through the game. Next step will be to scale up the number of teams for further testing and to add the team selection element.


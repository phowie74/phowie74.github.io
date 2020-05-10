---
layout: post
title:  "Extra time"
categories: [Hampden, jquery]
---

# Extra time

With a much more stable round processing function in place I now need to look at the extremely unfair "home advantage" which sees a draw after 90 minutes awarding the match to the home team. A new nested check needs to be added to the playGame logic to check for a winner and, in the event of a tie, progresses to extra time and then penalties if necessary. The original board game allowed for up to 2 replays but the excitement of penalties in the age of fixture congestion would make a far more satisfactory end. Checking for a winning score is easy...

```
if (homeScore != awayScore) {
  // we have a winner!
} else {
  // extra time
}
```
Each time the score is a tie, the game dice can be rolled again and another check for a winner can be carried out. Because of the extra step I can use the extra field at the end of each row to record how the match was won. After extra time, the result can be recorded in the normal way even if it is a draw as penalty scores can be recorded in the extra field. The options for the extra field would be full time (FT), after extra time (AET), and penalties (PENS). Penalties will also need to include the penalties score...
```
$('.match' + i + ' .ft').text('5 - 3 (Pens)');
``` 
Speaking of penalties, given the "best of 5" nature and sudden death scenario the standard result dice is not really fit for purpose. A final score of 5-1 on penalties simply would never happen. It would be worth while creating special binary penalty dice and loop through the penalty shootout blow by blow. When I come to add the manual match play for the chosen team this will hopefully add to the excitement of playing the game to its thrilling conclusion! I don't know what the odds are for penalty taking but a 66:33 split in favour of the striker seems a good staring point...
```
var penDice = [0,0,1,1,1,1]; // could just be [0,1,1] really!
```
Now I can loop through the 5 penalties rolling the dice for the home team and away team for each round of penalties...

```
var homePen = 0;
var awayPen = 0;
for (var i = 0; i < 5; i++) {
  homePen += penDice[Math.floor(Math.random() * penDice.length)];
  awayPen += penDice[Math.floor(Math.random() * penDice.length)];
}
```
I need to check after each penalty if the opposition are still in able to mathematically draw or win with the remaining penalties. I can set a variable for the remaining penalties `var r;` in each round and after the each teams penalty, the home team in this example, we can check if `(awayPen + r < homePen)` to say if we need to continue...
```
for (var i = 0; i < 5; i++) {
  var r = 5 - i;
  homePen += penDice[Math.floor(Math.random() * penDice.length)];
  if (awayPen + r < homePen) { break; }
  awayPen += penDice[Math.floor(Math.random() * penDice.length)];
  if (awayPen + r < homePen) { break; }
}
```
Then I can use the same logic as the extra time check to see if sudden death is required...
```
if (homePen > awayPen) {
  // home win!
} else if (homePen < awayPen) {
  // away win!
} else {
  // sudden death!
}
```
Sudden death provides another challenge as I need to loop homePen and awayPen indefinitely until one team leads by two. I can use a do loop with an arbitrarily high number which is unlikely to ever be rquired...
```
do {

  i++;
} while (i < 100);
```
and then add the same logic in between penalties except looking for a 2 goal advantage...
```
do {
  homeScore += Math.floor((Math.random() * 2));
  if ((awayScore + 2 == homeScore) || (awayScore - 2 == homeScore)) { break; }
  awayScore += Math.floor((Math.random() * 2));
  if ((awayScore + 2 == homeScore) || (awayScore - 2 == homeScore)) { break; }
  i++;
} while (i < 100);
```

This makes for a much more accurate game result scenario [with no easy byes for the home team](https://phowie74.github.io/dev/stage10.html). The math engine looks a little complex now so I think I need to look at ways to simplify this and not repeat so much code!


<!--
Create dice roll function


playMatch(home,away)



rolldice(home,away)





90, Extra, Pens, Sudden Death


RED (1st Div home) 4 4 3 2 1 0
GREEN (2nd Div home) 4 3 2 2 1 0
BLUE (3rd Div home) 5 4 2 1 0 0
ORANGE (1st Div away) 4 3 3 2 1 0
YELLOW (2nd Div away) 4 3 2 1 1 0
WHITE (3rd Div away) 5 4 1 1 0 0 

PEN - Goal-Goal-Goal-Goal-Saved-Missed
-->
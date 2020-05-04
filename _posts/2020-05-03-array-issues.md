---
layout: post
title:  "Array issues"
---

# Array issues

Having repeatedly tested the simplified results function it soon emerged there was an issue with the way the match data was being processed. I was finding that not all teams were being removed from the teams array as expected and a little bit of research showed that looping through an array and removing items as we go would cause inconsistencies so a little refactoring is required.

For simplicity, the buttons have been taken out of the footer now and the relevant buttons added to individual game screens. I have also added a secondary bitmap font as the main heading font was a bit too heavy for all purposes.

For this next approach I am going to keep everything within the same teams array rather than abstracting the draw in a separate array. By shuffling the teams array for each draw there is no complicated removing teams from the teams array based on the teams id, I can instead stick to the teams array keys...

```
teams.sort(function() { return 0.5 - Math.random() }); // shuffle teams array (draw teams)
```
I can then loop through each match in a round using two teams objects for each match. Rather than removing the items as it loops I can capture the losers as a list of the teams array keys. 
```
var t = 0; // teams array key
for (var i = 1; i <= matches; i++) {
  fixtures += '<tr class="match' + i + '">';
  // home team, key and score
  var homeKey = t;
  var homeTeam = teams[t].name;
  var homeScore = dice[Math.floor(Math.random() * dice.length)];
  fixtures += '<td width="15%"></td><td class="text-right home" id="' + t + '" width="25%">' + homeTeam + '</td>';
  fixtures += '<td class="homeScore" width="5%"></td><td class="" width="10%">vs</td><td class="awayScore" width="5%"></td>';
  t++; // away team, key and score
  var awayKey = t;
  var awayTeam = teams[t].name;
  var awayScore = dice[Math.floor(Math.random() * dice.length)];
  fixtures += '<td class="text-left away" id="' + t + '" width="25%">' + awayTeam + '</td>';
  fixtures += '<td class="ft" width="15%"></td>';
  t++; // increment t for next round of matches
  fixtures += '</tr>';
}

```
After all matches have been played I can then make sure the losers array is reversed so we can remove keys from the end of the array without causing any of the issues we saw previously.
```
losers = losers.reverse();
```
Finally I can loop through the (reversed) losers array and remove each team from the master teams array...

```
for (var i = 0; i < losers.length; i++) {
  teams.splice(losers[i],1);
}
```

This is a much more stable approach and can now produce [results consistently for each round](https://phowie74.github.io/dev/stage9.html). Next step will be to process draws by introducing extra time, penalties and even sudden death if necessary.
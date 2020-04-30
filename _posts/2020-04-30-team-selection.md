---
layout: post
title:  "Team Selection"
---

# Team Selection

Now the flow through the game screens is working as expected I can add to the array of teams to extend that testing and introduce the team selection element so players can choose who they want to play as through the game. As mentioned previously there needs to be 2, 4, 8, 16, 32 teams for the rounds to work out and from a mix of divisions. Still scaling this up so choosing just 8 teams at this stage would start with the Quarter Finals so I have added Inverness and Dundee Utd from the Championship with Raith Rovers and Dumbarton representing League 1.

Now we have a selection of teams we can list them on the team selection page. This can happen as part of the game initialisation as it only happens at the beginning of the game but it still makes it easier to manage if we create a separate function and call it as part of the initialisation process...

```
function listTeams() {
	teams.forEach(function(team) {
		var team;
		$('.teamList').append('<button class="teamBtn button" name="' + team.name + '" value="' + team.id + '">' + team.name + ' <small>(' + team.division + ')</small></button> ');
	});
}
```
The function is set up to loop through all teams and output a button for each containing the team name, id and division. I have also added an extra class ot the button so it can be listened for in the team selection script and it will also allow us to style these buttons independently of the game buttons which show in the footer...
```
.teamList { line-height: 4; }
button.teamBtn {
	background: white;
	color: #000;
}
button.teamBtn.selected {
	background: limegreen;
}
button.teamBtn:hover,
button.teamBtn:focus {
    background: yellow;
}
```

Now the team selection buttons are all listed and evenly spaced I can add the team selection script. But first it would be sensible to hide the Play Hampden button until a team has been selected. We can remove the `$('.playBtn').fadeIn();` element from the get started button and instead apply it once a team has been selected.

As far as selecting a team goes, in the first instance we want to store the id of the selected team as a variable and highlight the button so it is clear which team is selected. This will allow players to change their mind (and team) up to the point they click the Play Hampden button at which point heir choice will be stored and the team name updated through the game screens.

Because the team selection buttons are dynamically generated after the page has loaded we need to add the pick team script to a function and call it after the listTeams function has added the buttons to the page... 

```
var myTeamID;
var myTeam;
function pickTeam() {
  $('.teamBtn').click(function() {
    myTeamID = $(this).val();
    myTeam = teams[myTeamID].name;
    $('.teamBtn').removeClass('selected');
    $(this).addClass('selected');
  	$('.selectedTeam').text('You have selected ' + myTeam);
    $('#playBtn').show();
  });
}
```
Just a small irritation to address. Having no buttons show in the footer at the start of the pick teams screen makes the persistent footer bar collapse to just the padding. To avoid this I will revert the button to show automatically when the team selection screen starts but disable the button until a team is selected with appropriate styling to make this obvious...

`<button class="playBtn" style="display: none;" disabled>Play Hampden...</button>`

```
button:disabled {
  background: #cccccc;
  color: #999999;
  cursor: default;
}
```
We can then remove the disabled attribute when a team is selected.

`$('.playBtn').removeAttr("disabled");`

Finally we can make sure the selected team is stored and the game screens text updated once the Play Hampden button has been clicked and the game started. The placeholder span throughout the game screens is `.myTeam` and the teams array player attribute can be switched from 0 to 1. We just need to add a couple of extra items to the .playBtn click function...

```
$('.playBtn').click(function() {
	...
	$('.myTeam').text(myTeam);
	teams[myTeamID].player = 1;
});

```

You can [try selecting your team](https://phowie74.github.io/dev/stage6.html) and loop through the game. This is still assuming you win every round with your selected team so enjoy invincibility while it lasts!!


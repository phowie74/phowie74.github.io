---
layout: post
title:  "Some basic styling"
---

# Some basic styling

Although the original board game had a distinct vintage 50s feeling by the time I was playing with my brother we were very much in the 1980s. When I think back to following football in the 80s without wall-to-wall sky coverage of football from all over the world it was listening to crackly football commentary on the radio and, more pertinent to a text based game, endlessly waiting for the Ceefax page to refresh with the latest score updates.

I don't want to get into the ins and outs of Ceefax in this post but for those too young to remember it was a pre-internet version of the information superhighway but running at a more sedate speed - an information slow lane if you will. Through your television you had access to news, weather and of course sport by keying in numbers to load the correct pages of information. 

The simple 8bit white text on a black background punctuated with blocks of primary colours seems to be the ideal starting point so I did a quick google font search for something appropriate. "Press Start 2P" was an ideal starting point so I linked to the google font and put by first basic styles in place including a monospace fallback...

```
<link href="https://fonts.googleapis.com/css?family=Press+Start+2P" rel="stylesheet">
<style>
body {
  background: #222;
	color: #fff;
  font-family: 'Press Start 2P', Menlo, Courier New, monospace;
}
</style>
```

With a simple wrapper to constrain the width where necessary and some basic header and footer styles it is much easier to see the structure of the game. The wrapper is set centred at 90% of the screen width but with a maximum width to stop it stretching too much... 

```
.wrapper {
	margin: 0 auto;
	max-width: 1000px;
	width: 90%;
}
```

Inspired by the Ceefax layout, the header is then split into two sections, top left will be simple text whilst the top right will be the bold blue block familiar to those of a certain age. As these will ideally be vertically centred it is an ideal opportunity to turn the header into a flexbox container so I will also set the box sizing property to make working with margins more straight forward...

```
* { box-sizing: border-box; }
header {
  display: flex;
  flex-wrap: wrap;
}
.left{
  flex-basis: 20rem;
  flex-grow: 1;
}
.right {
  flex-basis: 0;
  flex-grow: 999;
}
```

Adding a bit of padding to the header and setting the items to align vertically centre will tidy things up a little...

```
header {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  padding: 10px;
}
```

We can tidy this up a little so the left and right side stack when the screen becomes too small to display comfortably side-by-side by adding a min width to our stretchy right portion. I want it to be a roughly 1/3 2/3 split so when the left column forces the right side to be less than 60% of the available width it will wrap onto a second row...

```
.right {
  flex-basis: 0;
  flex-grow: 999;
  min-width: 60%;
}
```

Because the blue highlight box is likely to be reused elsewhere and will require additional padding because of the background colour I have made this a separate styled element added to the right column...

```
<div class="right">
  <div class="box-blue">Hampden</div>
</div>
```
```
.box-blue {
  background-color: blue;
  padding: 10px;
  text-align: center;
}
```

As we want the footer to remain fixed to the bottom of the screen we can use `position: fixed;` on the footer set to 100% of the width, `bottom: 0;` to keep it at the bottom of the screen and add the wrapper to restrict it to the same width as the remaining content ...

```
footer {
	position: fixed;
	width: 100%;
	bottom: 0;
}
```
The button bar could also make use of the .box-blue highlight  to make it stand out so this can be wrapped around our buttons and finally some basic button styles so we can [test the flow through the game again...](https://phowie74.github.io/dev/stage3.html)


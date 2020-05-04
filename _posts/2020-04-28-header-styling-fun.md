---
layout: post
title:  "Header styling fun"
categories: [Hampden, css, svg]
---

# Header styling fun

Just a quick addition now the style is starting to take shape. I wanted to replicate the Ceefax style text drop shadow to make the Hampden header more prominent. I never intended there to be many images included in the game to keep it simple and speedy but a small 8 bit style trophy in the header would add a bit more to the header.

The header blue box is not acting as a page title so it does not need to be a heading tag - it is acting as an illustration / logo really. Adding a text styling class to the container means the style can be replicated elsewhere if required. Using a flexible font size (and drop shadow size) relative to the screen size means the heading will scale appropriately to fill the box regardless of screen size...

```
.ceefax {
	color: limegreen;
	font-size: 5vw;
	line-height: 1;
	text-shadow: 0px .7vw 0px rgba(0,0,0,1);
}
```

Sticking to the simple 8bit style a small blocky trophy representing the Scottish Cup will round off the header. Using illustrator to create a trophy illustration, I can export the svg code and embed directly into the page so it does not require a separate image download... 

```
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 135 180" width="50" height="67"><defs><style>.a{fill:#999;}.b{fill:#fff;}</style></defs><title>trophy</title><polygon points="27 63 27 45 108 45 108 63 117 63 117 54 135 54 135 99 126 99 126 108 117 108 117 117 108 117 108 126 99 126 99 135 90 135 90 144 81 144 81 153 99 153 99 162 108 162 108 180 27 180 27 162 36 162 36 153 54 153 54 144 45 144 45 135 36 135 36 126 27 126 27 117 18 117 18 108 9 108 9 99 0 99 0 54 18 54 18 63 27 63"/><polygon points="54 45 54 36 45 36 45 0 63 0 63 9 72 9 72 18 81 18 81 27 90 27 90 36 81 36 81 46 54 45"/><polygon class="a" points="9 99 9 63 18 63 18 72 36 72 36 81 45 81 45 63 99 63 99 72 117 72 117 63 126 63 126 99 126 99 108 99 108 108 108 117 108 117 99 117 99 126 90 126 90 135 81 135 72 135 72 144 72 153 72 162 99 162 99 171 36 171 36 162 63 162 63 153 63 144 63 135 45 135 45 126 36 126 36 117 27 117 27 99 9 99"/><polygon class="a" points="63 55 63 36 54 36 54 9 63 9 63 18 72 18 72 27 81 27 81 36 72 36 72 55 63 55"/><polygon points="36 99 36 81 18 81 18 90 27 90 27 99 36 99"/><polygon points="99 99 99 81 117 81 117 90 108 90 108 99 99 99"/><polygon class="b" points="99 54 36 54 36 81 45 81 45 63 99 63 99 54"/><rect class="b" x="36" y="90" width="9" height="9"/><rect class="b" x="54" y="27" width="9" height="9"/></svg>
```
Finally aligning the svg with the text by adding `svg { vertical-align: middle; }` makes it all [look a little more presentable now...](https://phowie74.github.io/dev/stage4.html)


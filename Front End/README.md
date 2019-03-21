# Notes

### CSS - Cascading Style Sheets

Css starts with a rule set, which has 2 parts. (1. Selector 2. Declaration Block)

Ex: 
	
	div {
		text-align: right;
	} 
     
    Selector -> div is a HTML element that needs to be Styled, and So, Div is Selector.

    text-align: right -> is Declaration Block (written inside curly braces 
                      -> which is the code that describes the styles you want to apply.


> <head> is used for putting the meta data of the site. Style is one of the information that goes in <head> element. Anything that goes inside <style></style> is CSS.

	/* add CSS here */ is a CSS comment.
	
	<!-- This is a comment --> is a HTML comment.

In order to style all the paragraphs, p { color: red } will be enough.

In order to select only particular paragraphs, html elements can be given attributes id or class.

One element can have only 1 id attribute, and multiple elements can not have same id.

Whereas class attribute, multiple elements can have same class name. (one element can have multiple class names seperated by spaces).

so for sytling, .class-name { } or #id-name { } can be used to apply css on multiple elements.

**Color:**

body {
	background-color: rgb(255,0,255);
}

or backgorund-color: #ff00ff;

font-family: Helvetica, Arial, sans-serif;  (If Helvetica is not available on a platform, it tries to render Arial and so on)
font-weight : bold;
text-transform: uppercase;
text-decoration:underline;

**Positional Selectors:**

https://css-tricks.com/how-css-selectors-work/

:nth-child(2) {

}

**Style Sheet:**

What if you wanted to use the same CSS on more than one webpage?

CSS lives in a different file.

define your stye in style-sheet-name.css file and link in head. <link rel="stylesheet" href="style-sheet-name.css">

CSS allows content to overflow its containter. i.e, if image is bigger than container, image would overflow.

catch all (?) for image in css 

img, embed, 
object, video {
   max-width: 100%;
	}
}

### Responsive

**Flexbox**

One of the tool to build layouts.

So powerful coz, it Fills the space thats available. 

If an element has extra space around it, it wil expand to fit or if its getting crowded elements shrink so that they take up as little space as possible.

flex-container: by default, flex elements fit on a single line. (doesnt matter with the width of the element thats set, they wont wrap). Browser will size them to fit within the viewport. This can be changed with flex-wrap: wrap property

order attribute: can change the order of the element.

**Grids**

With Grids fluid system, columns end up wrapping to the next line as browser width start getting smaller.

**Media Queries:**

Few ways to selectively apply different CSS styles for different screen sizes.

Media Query is one of the way. 

Media Queries provides easy logic to apply different styles depending on device characteristics (like width, height, pixel ratio.). Logic like changing everything form background images to restructuring site layout.

For adding responsive style, just add media query to additional style sheet.

Ex: (Linked CSS)
><link rel="stylesheet" media="screen and (min-width: 300px)" href="over300.css"> 

other way is @media and @import .

Ex:
@media screen and (min-width: 500px) {
	body {background-color: green}
}

@import: performance costly.
Linked CSS: many small files but many http requests.
@media: big files and fewer http requests.

Breakpoint: point at which the page changes its layout is called breakpoint, and depending on how yout page is layed out you might have one or several breakpoints.

Breakpoints are decided based on the content (start with small size screen, and single column)

font-boosting ?

viewport:
><meta name="viewport" content="width=device-width,initial-scale=1">

https://developer.mozilla.org/en-US/docs/Mozilla/Mobile/Viewport_meta_tag

### Responsive Patterns

<img src="responsivePatterns.png" alt="Responsive Patterns" height="300" width="400">

1. Mostly fluid
2. Layout Shifter
3. Column Drop
4. Off Canvas

**Column Drop**

At its narrowest viewport, it Stacks elements vertically.

<img src="column-drop.png" alt="Responsive Patterns" height="300" width="400">
<img src="column-drop-1.png" alt="Responsive Patterns" height="300" width="400">

**Mostly fluid**










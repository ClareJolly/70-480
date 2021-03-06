# Module 2 - CSS Selectors and Style Properties <!-- omit in toc -->

**C**ascading **S**tyle **S**heets

- [**Selectors**](#Selectors)
  - [type selectors](#type-selectors)
  - [.class selectors](#class-selectors)
  - [the universal selector](#the-universal-selector)
  - [#id selectors](#id-selectors)
  - [[attribute] selectors](#attribute-selectors)
  - [:pseudo-class and ::pseudo-element selectors](#pseudo-class-and-pseudo-element-selectors)
    - [pseudo-element](#pseudo-element)
    - [pseudo-class](#pseudo-class)
  - [selector chains, selector chains, selector chains](#selector-chains-selector-chains-selector-chains)
- [**Combinators**](#Combinators)
  - [decendant combinator](#decendant-combinator)
  - [child > combinator](#child--combinator)
  - [general ~ sibling](#general--sibling)
  - [adjacent + sibling](#adjacent--sibling)
- [**Color properties**](#Color-properties)
  - [color](#color)
    - [named color](#named-color)
    - [hex (#)](#hex)
    - [rgb()/rgba()](#rgbrgba)
    - [hsl()/hsla()](#hslhsla)
  - [opacity](#opacity)
- [**Text properties**](#Text-properties)
- [**Font properties**](#Font-properties)
  - [font-style: normal | italic | oblique](#font-style-normal--italic--oblique)
  - [font-variant: normal | small-caps](#font-variant-normal--small-caps)
  - [font-weight: normal | bold | bolder | light | lighter |](#font-weight-normal--bold--bolder--light--lighter)
  - [font-size: {length}](#font-size-length)
  - [line-height: {length}](#line-height-length)
  - [font-family: {font}](#font-family-font)
  - [columns](#columns)
  - [font shorthand property](#font-shorthand-property)
- [**Box properties**](#Box-properties)
  - [Border](#Border)
  - [Margin](#Margin)
  - [Padding](#Padding)
  - [Sizing](#Sizing)
    - [Width and height](#Width-and-height)
    - [box-sizing (content-box | border-box)](#box-sizing-content-box--border-box)
    - [min/max prefixes](#minmax-prefixes)
  - [Visibility](#Visibility)
  - [box-shadow and border-radius](#box-shadow-and-border-radius)
  - [Gradients](#Gradients)
- [**Demo**](#Demo)
- [**Supplementary**](#Supplementary)
  - [Style rules syntax](#Style-rules-syntax)
- [**Example questions**](#Example-questions)

---

[🔼](#readme)

## **Selectors**

### type selectors

Refers to the type of html - e.g. table, ul

```css
table { color: red;}
```

Also cascades down to the child tags (td, tr, etc) of the table

```css
ul { color: red;}
```

### .class selectors

Classes don't need to be unique.  Way of describing the element

```html
<div class="fancy">Fancy</div>
<div class="fancy bold">Fancy AND bold</div>
```

```css
.fancy { color: red;}
.bold { font-weight: bold;}
```

div 1 will just be red, div 2 will be red and bold

![css1](../images/css1.png)

### the universal selector

Note that the above css is essentially the same as

```css
*.fancy { color: red;}
*.bold { font-weight: bold;}
```

the `*` is the universal type selector.  Which means **all** of the tags with the class of fancy.

You could also specify only a div with the class of fancy be affected

```css
div.fancy { color: red;}
```

### #id selectors

id will be unique
  
```html
<div id="div1">Fancy</div>
<div id="div2">Bold</div>
```

```css
#div1 { color: red;}
#div2 { font-weight: bold;}
```

![css2](../images/css2.png)

### [attribute] selectors

`data-<anything you want>`

```html
<div id="div1" data-author="me">Fancy</div>
<div id="div2" data-author="you">Bold</div>
<div id="div3" data-author="youAndme">Bold</div>
<input type="text" required="required">
<input type="text" >
```

```css
[data-author] { color: red;}
[data-author=you] { font-weight: bold;}
[data-author$=me] { font-weight: bold; font-size: 10pt;}
[data-author^=you] { font-weight: bold; font-size: 10pt;}
[data-author*=And] { color: green; font-size: 10pt;}
[required] { color: blue;}
```

![css3](../images/css3.png)

* `$=` = ends with
* `^=` = begins with
* `*=` = contains

### :pseudo-class and ::pseudo-element selectors

#### pseudo-element
```html
<p>I am a paragraph</p>
<p>I am another slightly longer paragraph</p>
```

```css
p::first-letter { color: red;}
```

e.g.
- `first-letter`
- `first-line`

![css4](../images/css4.png)

#### pseudo-class

```html
<p>I am a paragraph - hover over me to make me red</p>
<ul>
    <li>I am first</li>
    <li>I am second</li>
    <li>I am third</li>
</ul>
```

```css
p:hover { color: red;}
```

![css5](../images/css5.png)

other pseudo classes

```html
<p>I am a paragraph - hover over me to make me red</p>
<ul>
    <li>I am first - I can be pink</li>
    <li>I am second</li>
    <li>I am third - I am blue</li>
</ul>
<ul>
    <li>I am first - I can be pink</li>
    <li>I am second</li>
    <li>I am third - I am blue</li>
    <li>I am fourth</li>
    <li>I am fifth - I am orange</li>
</ul>
```

```css
p:hover { color: red; }
li:first-child {color: pink }
li:nth-child(3) {color: blue }
li:nth-of-type(5) {color: orange }
```

with `nth-child` - can also specify formula if desired

also `nth-of-type`

![css6](../images/css6.png)

### selector chains, selector chains, selector chains

```css
table, ul { color: red;}
```

Note that combining selectors stops at the comma

```css
div table, ul { color: red;}
```

is not the same as

```css
div table, div ul { color: red;}
```

---

[🔼](#readme)

## **Combinators**

Ways of combining simple selectors in css so that they apply to targetted sections

Helps with scoping

### decendant combinator

Denoted by a space.  Means any decendant no matter how deep

```html
<div id="div1">
    <p>I will not go red</p>
    <div>Fancy - I am red
        <div>I will also be red</div>
    </div>
</div>
<div id="div2">Bold</div>
```

```css
#div1 div { color: red;}
#div2 { font-weight: bold;}
```

In this example, `div` is a decendant of the id `div1`.  Will affect direct descendants as well as deeper decendants.

![css7](../images/css7.png)

### child > combinator

Specifies that it's only a direct decendant affected

```html
<div id="div1">
    <p>I will not go red</p>
    <div>Fancy - I am red
        <div>I will not be red this time</div>
    </div>
</div>
<div id="div2">Bold</div>
```

```css
#div1 > div { color: red;}
#div2 { font-weight: bold;}
```

### general ~ sibling

Sibling but doesn't have to be immediate sibling

```html
<div id="div1">
    <p>I will not go red because of the order in the selectors</p>
    <div>not red
        <div>not red either</div>
    </div>
    <p>paragraph</p>
    <h1>heading</h1>
    <p>I will be red because I have a div sibling before me somewhere</p>
</div>
<p id="p">I'll be red too</div>
```

```css
div ~ p { color: red;}
#div2 { font-weight: bold;}
```

Order is important.  But doesn't have to directly be next to each other

![css8](../images/css8.png)

### adjacent + sibling

Now has to be directly next to each other

```html
<div id="div1">
    <p>I will not go red because of the order in the selectors</p>
    <div>I am not red
        <div>not red</div>
    </div>
    <p>paragraph - I am red</p>
    <h1>heading</h1>
    <p>I will NOT be red this time because I have an h1 in the way between me and my preceding div sibling</p>
</div>
<p id="p">I'll be red too</div>
<ul>
    <li>I am first</li>
    <li>I am second - I am pink</li>
    <li>I am third - I am pink too</li>
</ul>
```

```css
div + p { color: red;}
#div2 { font-weight: bold;}
li + li {color: pink}
```

This is an alternate way of doing something that you could do (better) with first-child pseudo selector

![css9](../images/css9.png)

---

[🔼](#readme)

## **Color properties**

### color

#### named color

Couple of hundred available

```html
<p>I will be a named color</p>
```

```css
p {color: red;}
```

![color1](../images/color1.png)

#### hex (#)

- first 2 red
- second 2 green
- third 2 blue

So: 
- `#000000` - black
- `#ffffff` - white

3 pairs - if each pair is identical you can specify with only 3

- `#000` - black
- `#fff` - white

```html
<p>I will be a hex color</p>
```

```css
p {color: #00ff00;}
```

![color2](../images/color2.png)

#### rgb()/rgba()

Takes 3 numbers from 0 to 255

```html
<p>I will be a rgb color - green</p>
```

```css
p {color: rgb(0, 255, 0);}
```

![col3](../images/color3.png)

RGBA expects another value which is the opacity. 0-1 value

```html
<p>I will be a rgba color - opaque blue</p>
```

```css
p {color: rgba(0, 0, 255, 0.4);}
```

![col4](../images/color4.png)

#### hsl()/hsla()

hue, saturation, lightness

Takes 3 numbers:
- hue - 0-255
- saturation - percentage (lower number, lower saturation)
- lightness - how much/little black - percentage (higher number, the lighter it is)

```html
<p>I will be a hsl color - red but low saturation</p>
```

```css
p {color: hsl(0, 30%, 70%);}
```

![col5](../images/color5.png)

hsla expects another value which is the opacity. 0-1 value

```html
<p>I will be a hsla color - blue</p>
```

```css
p {color: hsla(255, 80%, 30%, 0.7);}
```
![col6](../images/color6.png)

### opacity

In addition to affecting opacity by rgba and hsla, you can do it directly

```html
<p>I will be black but look like I'm grey</p>
```

```css
p {opacity: 0.5;}
```

![col7](../images/color7.png)

---

[🔼](#readme)

## **Text properties**

- `text-decoration: overline | underline | line-through`
- `text-transform: none | lowercase | uppercase | capitalize`
- `text-shadow` (i.e. 2px 2px grey) (direction-right direction-bottom [blur] color)

    ```html
    <p>I will have a shadow</p>
    ```

    ```css
    p {text-shadow: 2px 2px red;}
    ```

    ![text1](../images/text1.1.png)

    Adding in a 3rd value adds the blur factor

    ```html
    <p>I will have a blurry shadow</p>
    ```

    ```css
    p {text-shadow: 2px 2px 5px red;}
    ```

    ![text1](../images/text1.2.png)

    Can also add another shadow on top - this one defaults to black but can add a color

    ```html
    <p>I will have a blurry shadow in red at an angle with black shadow with no angle</p>
    ```

    ```css
    p {text-shadow: 2px 2px 5px red,0 0 10px;}
    ```

    ![text1](../images/text1.3.png)

---

[🔼](#readme)

## **Font properties**

### font-style: normal | italic | oblique
    
```html
<p>I will be italic</p>
```

```css
p {font-style: italic;}
```

![font](../images/font1.png)

### font-variant: normal | small-caps

```html
<p>I will be small caps</p>
```

```css
p {font-variant: small-caps;}
```

![font](../images/font1.1.png)

### font-weight: normal | bold | bolder | light | lighter | #

```html
<p>I will be bold</p>
```

```css
p {font-weight: bold;}
```

![font](../images/font2.png)

specifying a weight from 100 lightest) to 900 (boldest)

### font-size: {length}

```html
<p>I will be huge</p>
```

```css
p {font-size: 52pt;}
```

![font](../images/font3.png)

can use pt, px, cm, in

### line-height: {length}

```html
<p>I will be huge and overlap</p>
```

```css
p {font-size: 52pt; line-height: 26pt;}
```

![font](../images/font4.png)

### font-family: {font}

Requires that the font is available on the client

You can define a font face you can use that will be usable on the client even if it doesn't have it by default

```css
@font-face {
    font-family: "mona";
    src: url("fonts/Mona-Free.woff") ;
}

.mona {
    font-family: mona;
}
```

```html
<p class="mona">cool font</p>
```

![font](../images/font5.png)

.woff = webfont - wrapper around font keeping DRM

### columns
    
```html
<p class="columns">imagine I am loads of text</p>
```

```css
.columns {
    /* columns: 40px; width */
    columns: 4; /* number of columns */
    overflow-x: scroll;
    height: 600px;
    padding: 20px;
}
```

![col1](../images/columns1.png)

### font shorthand property

specify multiple at a time denoted by spaces

---

[🔼](#readme)

## **Box properties**

![boxmodel](../images/boxmodel.png)

Content in the middle.  Specifying a size will only affect the content in the middle.  To affect the whole thing need to specify

### Border

For border need to specify:
- size - e.g. 2px
- style - e.g. dotted, solid, dashed, etc
- color

```html
<div class="lorem">
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptates accusantium suscipit aliquam esse minima perspiciatis asperiores quis totam exercitationem delectus?</p>
</div>
```

```css
body {background-color:skyblue;}
p { color: red;}
.lorem { border: 2px dotted yellow;}
```

![border](../images/border1.png)

Can also specify each side individually

```css
.lorem {
    border-left: 2px dotted yellow;
    border-top: 2px solid purple;
    border-right: 10px dashed green;
    border-bottom: 5px outset orange;
    }
```

![border](../images/border2.png)

### Margin

Margin adds space **outside** of the div

```html
<div class="lorem">
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptates accusantium suscipit aliquam esse minima perspiciatis asperiores quis totam exercitationem delectus?</p>
</div>
```

```css
body {background-color:skyblue;}
p { color: red;}
.lorem { margin: 20px;}
```

![margin](../images/margin1.1.png)

### Padding

Padding adds space **inside** of the div

```html
<div class="lorem">
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptates accusantium suscipit aliquam esse minima perspiciatis asperiores quis totam exercitationem delectus?</p>
</div>
```

```css
body {background-color:skyblue;}
p { color: red;}
.lorem { padding: 20px;}
```

![padding](../images/padding1.png)

### Sizing

#### Width and height

```html
<div class="lorem">
    <p>Lorem ipsum dolor sit amet consectetur adipisicing elit. Voluptates accusantium suscipit aliquam esse minima perspiciatis asperiores quis totam exercitationem delectus?</p>
</div>
```

```css
body {background-color:skyblue;}
p { color: red;}
.lorem { border: 50px solid yellow; width: 200px;}
```

![size](../images/size1.png)

with a thin border

![size](../images/size1.0.png)

content is still the same size but border makes the whole thing bigger

#### box-sizing (content-box | border-box)

To specify that you mean the border is the width you are interested in

```css
body {background-color:skyblue;}
p { color: red;}
.lorem { border: 50px solid yellow; width: 200px; box-sizing: border-box}
```

![size](../images/size2.png)

#### min/max prefixes

min-width and min-height can add some constraints

---

### Visibility

- display (inline | block | inline-block | none | ...)
    How something is displayed - or not shown at all
- visibility (visible | hidden)
    Hide things but keep a placeholder for them
- white-space (normal | pre | nowrap | pre-wrap | pre-line)
- overflow (visible | hidden | scroll | auto)
    What to do when contents falls outside of box

---

### box-shadow and border-radius

- box-shadow (i.e. 10px 10px 5px #888888)
  
- border-radius (i.e. 5px)
  rounded corners

---

### Gradients

- linear gradient
    i.e. background-image: linear-gradient(to right, black 0%, white 100%)
- radial gradient
    i.e. background-image: radial-gradient(circle at 50% 50%, black 0%, white 100%)

---

[🔼](#readme)

## **Demo**

---

[🔼](#readme)

## **Supplementary**

### Style rules syntax

```css
selector {
    property: value;
    property: value;
}
```

Flat language with selectors and style rules.

`selector` - What you want to affect
`property:value;` pair - How you want to affect it

---

[🔼](#readme)

## **Example questions**

[Module 2](./example-questions/2-example-questions.pdf)

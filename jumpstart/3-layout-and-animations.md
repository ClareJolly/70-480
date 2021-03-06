# Module 3 - Advanced layout and Animations <!-- omit in toc -->

- [**Legacy layouts**](#Legacy-layouts)
  - [positioning and z-index](#positioning-and-z-index)
  - [display](#display)
  - [float](#float)
  - [Legacy demo](#Legacy-demo)
- [**Flexbox**](#Flexbox)
  - [horizontal or vertical](#horizontal-or-vertical)
  - [packing](#packing)
  - [alignment](#alignment)
  - [Putting it all together](#Putting-it-all-together)
  - [flex](#flex)
  - [wrapping](#wrapping)
- [**Grid**](#Grid)
  - [Basic example](#Basic-example)
  - [Row spanning](#Row-spanning)
- [**Transforms**](#Transforms)
- [**Transitions and Animations**](#Transitions-and-Animations)
  - [Transitions](#Transitions)
  - [Animations](#Animations)
- [**Demo**](#Demo)
- [**Supplementary**](#Supplementary)
- [**Example questions**](#Example-questions)

---

[🔼](#readme)

## **Legacy layouts**

### positioning and z-index

- `position`
  - `absolute` - position based on the document position
  - `relative` - preserve where it would be in original markup and position relative to that
- `z-index` - higher number, on the top

Can be used to exactly position elements

```css
#id01 {
    background-color: lightgreen;
    width: 50px;
    height: 50px;
    position: absolute;
    top: 25px;
    left: 50px;
    z-index: 0;
}
#id02 {
    background-color: lightblue;
    width: 50px;
    height: 50px;
    position: relative;
    top: 25px;
    left: 50px;
    z-index: 1;
}
```

```html
<div id="id01">DIV 01</div>
<div id="id02">DIV 02</div>
```

![position](../images/position1.png)

---

### display

```css
.vanish {
    display: none;
}
.centered {
    display: table-cell;
    min-height: 200px;
    min-width: 200px;
    text-align: center;
    vertical-align: middle;
    border: 1px solid #ff4444;
}
```

```html
<div class="vanish">DIV 01</div>
<div class="centered">DIV 02</div>
```

without `display: none`

![display](../images/display1.png)

with `display: none`

![display](../images/display2.png)

`display: table-cell` is useful for centering.  Allows vertical align to behave itself

`vertical-align` may not always work depending on the element it's in.

---

### float


---

### Legacy demo

[demo](./demo/3-demo-legacy.html)

![demo](../images/legacy-demo.png)

on hover

![demo](../images/legacy-demo1.1.png)

---

[🔼](#readme)

## **Flexbox**

Good at laying things out in a way that flows.  Can flow automatically

Gives control over child elements

### horizontal or vertical

Child items can be laid out horizontally or vertically.  Left to right or vice versa and top to bottom and vice versa

### packing

Alignment of the child items along the axis of layout.  So for horizontal, controls spacing between child items

---

### alignment

Alignment of the child items perpendicular to the axis of layout.  So for horizontal, controls vertical alignment

---

### Putting it all together

The essential building block of a flexbox is:
- `display: flex`

```html
<div class="flexbox">
    <div id="flexbox1">
        <div></div>
        <div></div>
        <div></div>
    </div>
</div>
```

```css
.flexbox #flexbox1 {
    margin-top: 20px;
    display: flex;
    /* display:-ms-flexbox;
    display: -webkit-flex */
    width: 100%;
    /* -ms-flex-pack:distribute;
    -webkit-box-pack:justify; */
    justify-content: space-evenly;
    border: 1px solid grey;
}

.flexbox #flexbox1 > div {
    min-width: 80px;
    min-height: 80px;
}
.flexbox #flexbox1 > div:nth-child(1) {
    background-color: red;
}
.flexbox #flexbox1 > div:nth-child(2) {
    background-color: green;
}
.flexbox #flexbox1 > div:nth-child(3) {
    background-color: blue;
}

```

![flex](../images/flex1.png)

---

### flex

`flex: 1 0 auto`

- relative amount of flex - relative to all other children, how much should it flex.  `flex: 2 0 auto` is twice as big as `flex: 1 0 auto`
- recommended size

```html
<div class="flexbox">
    <div id="flexbox2">
        <div></div>
        <div></div>
        <div></div>
    </div>
</div>
```

```css
.flexbox #flexbox2 {
    margin-top: 20px;
    display: flex;
    /* display:-ms-flexbox;
    display: -webkit-flex */
}

.flexbox #flexbox2 > div {
    min-width: 80px;
    min-height: 80px;
}
.flexbox #flexbox2 > div:nth-child(1) {
    background-color: red;
    flex: 1 0 auto;
}
.flexbox #flexbox2 > div:nth-child(2) {
    background-color: green;
    flex: 2 0 auto;
}
.flexbox #flexbox2 > div:nth-child(3) {
    background-color: blue;
}

```

![flex](../images/flex2.png)

You can see that the first 2 are flexing and the 3rd is just the original div size specified (80px x 80px)

Then in the remaining space red takes up 1 space, and green takes up 2 spaces

---

### wrapping

```html
<div class="flexbox">
    <div id="flexbox3">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
    </div>
</div>
```

```css
.flexbox #flexbox3 {
    margin-top: 20px;
    display: flex;
    /* display:-ms-flexbox;
    display: -webkit-flex */
    flex-wrap: wrap;
    /* -ms-flex-wrap: wrap;
    -webkit-flex-wrap: wrap; */
    width: 100%;
}

.flexbox #flexbox3 > div {
    min-width: 40px;
    min-height: 40px;
    background-color: grey;
    margin: 5px;
    font-size: 60px;
    padding: 15px;
}

.flexbox #flexbox3 > div:nth-child(7) {
    background-color: red;
}
```

![flex](../images/flex3.png)

and vertical

```html
<div class="flexbox">
    <div id="flexbox3">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
    </div>
</div>
```

```css
.flexbox #flexbox4 {
    display: flex;
    /* display:-ms-flexbox;
    display: -webkit-flex */
    flex-direction: column;
    /* -ms-flex-direction:column;
    -webkit-flex-direction:column; */
    align-items: flex-start;
    /* -ms-flex-pack: start;
    -webkit-box-pack: start; */
    width: 100%;
    border: 1px solid gray;
}

.flexbox #flexbox4 > div {
    width: 80px;
    height: 20px;
    border: solid 1px black;
    margin:5px;
}
```

![flex](../images/flex4.png)

---

[🔼](#readme)

## **Grid**

Better at absolute layout control

Used to use tables for layout - this is a new way of this

- Power of tables but implemented in CSS
- absolute rows and columns
- fractional rows and columns
- spanning
- alignment

### Basic example

```html
<div class="grid">
    <div id="grid1">
        <div>
            <div>A</div>
            <div>B</div>
            <div>C</div>
            <div>D</div>
        </div>
    </div>
</div>
```

```css
.grid #grid1 > div {
    display: grid;
    /* display: -ms-grid; */
    grid-template-columns: 40px 40px;
    /* -ms-grid-column: 250px 250px; */
    grid-template-rows: 40px 40px;
    /* -ms-grid-row: 250px 250px; */
}

.grid #grid1 > div > div {
    border: 1px solid grey;
}

.grid #grid1 > div > div:nth-child(2) {
    grid-column: 2;
    grid-row: 1;
}

.grid #grid1 > div > div:nth-child(3) {
    grid-row: 2;
}

.grid #grid1 > div > div:nth-child(4) {
    grid-column: 2;
    grid-row: 2;
}
```

![grid](../images/grid1.1.png)

First div is what defines the grid structure

- `grid-template-columns: 40px 40px;` - 2 columns of 40px each
- `grid-template-rows: 40px 40px;` - 2 rows of 40px each

---

### Row spanning

```html
<div class="grid">
    <div class="msgrid">
        <div></div>
        <div></div>
        <div></div>
    </div>
</div>
```

```css
.grid .msgrid {
    display: grid;
    /* display: -ms-grid; */
    grid-template-columns: 100px 1fr 100px;
    /* -ms-grid-columns: 100px 1fr 100px; */
    grid-template-rows: 40px 1fr 40px;
    /* -ms-grid-rows: 80px 1fr 80px; */
    height: 180px;
}

.grid .msgrid > div:nth-child(1) {
    grid-column: 1;
    /* -ms-grid-column: 1; */
    grid-row: 1 / span 2;
    /* -ms-grid-row: 1; */
    /* -ms-grid-row-span: 2; */
    border: solid 2px blue;
}

.grid .msgrid > div:nth-child(2) {
    grid-column: 2;
    /* -ms-grid-column: 2; */
    grid-row: 2;
    /* -ms-grid-row: 2;
    -ms-grid-column-span: 2; */
    border: solid 2px green;
}

.grid .msgrid > div:nth-child(3) {
    grid-column: 2 / span 2;
    /* -ms-grid-column: 2; */
    grid-row: 3;
    /* -ms-grid-row: 3; */
    border: solid 2px red;
}
```

![grid](../images/grid2.png)

```css
grid-template-columns: 100px 1fr 100px;
grid-template-rows: 40px 1fr 40px;
```

`fr` means fractional relative to the other sizes.  Useful for responsive

Another example with really specific dimensions

```html
<div class="grid">
    <div class="msgrid">
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
        <div></div>
    </div>
</div>
```

```css
.grid .msgrid {
    display: grid;
    /* display: -ms-grid; */
    grid-template-columns: 120px 150px 120px;
    /* -ms-grid-columns: 120px 150px 120px; */
    grid-template-rows: 67px 67px 67px;
    /* -ms-grid-rows: 67px 67px 67px; */
}

.grid .msgrid > div {
    border: 1px solid; margin: 5px; 
}
.grid .msgrid > div:nth-of-type(1) {
    grid-row: 1; 
    /* -ms-grid-row: 1; */
    grid-column: 1; 
    /* -ms-grid-column: 1; */
}
.grid .msgrid > div:nth-of-type(2) {
    grid-row: 2; 
    /* -ms-grid-row: 2; */
    grid-column: 1;
    /* -ms-grid-column: 1; */
}
.grid .msgrid > div:nth-of-type(3) {
    grid-row: 3; 
    /* -ms-grid-row: 3; */
    grid-column: 1; 
    /* -ms-grid-column: 1; */
}
.grid .msgrid > div:nth-of-type(4) {
    grid-row: 1 / span 3; 
    /* -ms-grid-row: 1; */
    /* -ms-grid-row-span: 3; */
    grid-column: 2;
    /* -ms-grid-column: 2; */
}
.grid .msgrid > div:nth-of-type(5) {
    grid-row: 1; 
    /* -ms-grid-row: 1; */
    grid-column: 3; 
    /* -ms-grid-column: 3; */
}
.grid .msgrid > div:nth-of-type(6) {
    grid-row: 2; 
    /* -ms-grid-row: 2; */
    grid-column: 3; 
    /* -ms-grid-column: 3; */
}
.grid .msgrid > div:nth-of-type(7) {
    grid-row: 3; 
    /* -ms-grid-row: 3; */
    grid-column: 3; 
    /* -ms-grid-column: 3; */
}
```

![grid](../images/grid3.png)

---

[🔼](#readme)

## **Transforms**

Change something visually.  The following are the ways to change:

- rotate
- skew - angle/stretch
- scale
- translate - move

```css
 #pic {
    width: 150px;
    height: 150px;
}

#pic:hover {
    transform: scale(0.5) translate(50px, 50px) rotate(10deg) ;
}
```

```html
<figure id="pic">
    <img src="images/fg.jpg" alt="Fizzgig"/>
    <figcaption>Fizzgig</figcaption>
</figure>
```

![transform](../images/transform1.png)

on hover

![transform](../images/transform2.png)

---

[🔼](#readme)

## **Transitions and Animations**

### Transitions

Changing the state from 1 state to another.  A response to something

e.g. hover, active, programatically.

```css
.someClass {
    transition: all 1s;
}

.someClass:hover {
    border-radius: 50%;
}
```

`transition: all 1s;`

`all` - if any property changes then for 1 second show transition

In this example, on hover, it will change the border-radius over the course of 1 second


```css
 #pic {
    width: 150px;
    height: 150px;
    transition: all 2s;
}

#pic:hover {
    transform: scale(0.5) translate(50px, 50px) rotate(10deg) ;
}
```

```html
<figure id="pic">
    <img src="images/fg.jpg" alt="Fizzgig"/>
    <figcaption>Fizzgig</figcaption>
</figure>
```

---

### Animations

Using JS to call the onclick event and animate

[demo](./demo/3-demo-anim.html)

![anim](../images/anim1.png)

and at the end

![anim](../images/anim2.png)

---

[🔼](#readme)

## **Demo**

---

[🔼](#readme)

## **Supplementary**

---

[🔼](#readme)

## **Example questions**

[Module 3](./example-questions/3-example-questions.pdf)

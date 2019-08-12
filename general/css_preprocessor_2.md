#### 19.08.12

## Functions and Operations

- Operate on color values
- Iterate on lists and maps
- Apply styles based on conditions
- Assign values that result from math operations


### Arithmetic and Color
- __alpha parameter__: a mask denoting opacity in RGBA or HSLA
- can apply `fade-out` and `fade-in`

```scss
   $color: rgba(39, 39, 39, 0.5);
   $amount: 0.1;
   $color2: fade-out($color, $amount);//rgba(39, 39, 39, 0.4) 
```
```scss
    $color: rgba(55,7,56, 0.5);
    $amount: 0.1;
    $color2: fade-in($color, $amount); //rgba(55,7,56, 0.6)
```

- Computin colors in Sass:  performed on the red, green, and blue components
  
```scss
$color: #010203 + #040506;

// 01 + 04 = 05
// 02 + 05 = 07
// 03 + 06 = 09

// to be
$color: #050709;
```

```scss
$color: red + blue;

// to be
$color: magenta;
```

- addition `+`
- subtraction `-`
- multiplication `*`
- division `/`
- modulo `%`

```scss
$width: 250px;

height: $width/6;
line-height: $width/6;
border-radius: $width/30;
```

### Each Loops
- Each loops: iterate on each of the values on a list

from scss
```scss
$list: (orange, purple, teal);

@each $item in $list {
  .#{$item}{
    background: $item;
  }
}
```

to css
```css
.orange {
  background: orange;
}

.purple {
  background: purple;
}

.teal {
  background: teal;
}
```

### For Loops
- For loops: can be used to style numerous elements or assigning properties all at once

```scss
$total: 10; //Number of .ray divs in our html
$step: 360deg / $total; //Used to compute the hue based on color-wheel

.ray {
  height: 30px;
}

//Add your for-loop here:
@for $i from 1 through $total{
    .ray:nth-child(#{$i}){
        background: adjust-hue(blue, $i * $step);
    }
}
```
comes to
![1](https://user-images.githubusercontent.com/38183218/62871501-8eac3b80-bd56-11e9-8703-4c703690210e.JPG)


### Conditionals
- `if()`: a function that can only branch one of two ways based on a condition.

above added
```scss
@for $i from 1 through $total{
    .ray:nth-child(#{$i}){
        background: adjust-hue(blue, $i * $step);
        width: if($i % 2 == 0, 300px, 350px);
    margin-left: if($i % 2 == 0, 0px, 50px);
    }
}
```
![2](https://user-images.githubusercontent.com/38183218/62871824-2578f800-bd57-11e9-9625-ebd84ec2d123.JPG)

- For cases with __more than two outcomes__: the `@if`, `@else-if`, and `@else` directives allow for more flexibility.

```scss
@mixin deck($suit) {
    @if($suit == hearts || $suit == spades){
    color: blue;
    }
    @else-if($suit == clovers || $suit == diamonds){
    color: red;
    }
    @else{
    //some rule
    }
}
```


## SUSTAINABLE SCSS: 'Sasstainability'
Since Sass can be confusable unless you have decent structure, please be sure to assure following  rules.

### Sass Structure
- an example of a well-organized Sass file structure
- ![3](https://user-images.githubusercontent.com/38183218/62872264-18a8d400-bd58-11e9-98af-a4314103d596.JPG)

### @Import in SCSS
- Typically, all imported SCSS files are imported into a main SCSS file which is then __combined to make a single CSS__ output file.
- Then main/global SCSS file has access to any variables or mixins defined in its imported files.

```scss
@import url(https://fonts.googleapis.com/css?family=Pacifico); //CSS import
@import "helper/placeholders";
@import "helper/mixins";
```

### Organize with Partials
- a `_` prefix: tells Sass to __hold off on compiling__ the file individually and instead import it.

```scss
_<filename>.scss
```

- omit the underscore if you want to import this partial into the main file 

```scss
 @import "<filename>";
```

### @Extend
- "   Many times, when styling elements, we want the __styles of one class__ to be applied to another in addition to its own individual styles.
"

- traditional approach: to give the element both classes
```css
<span class="lemonade"></span>

<span class="lemonade strawberry"></span>
```
Above is a potential bug in maintainability.

Instead of it, we can `@extend`

```scss
.lemonade {
  border: 1px yellow;
  background-color: #fdd;
}
.strawberry {
  @extend .lemonade;
  border-color: pink;
}
```

### %Placeholders
- It's behave like a __Abstract Class__

from scss
```scss
 a%drink {
        font-size: 2em;
        background-color: $lemon-yellow;
    }

    .lemonade {
    @extend %drink;
    //more rules
}
```

to css

```css
a.lemonade {
        font-size: 2em;
        background-color: $lemon-yellow;
    }

    .lemonade {
    //more rules
}
```

### @Extend vs @Mixin
- Recall that __mixins__, unlike extended selectors, insert the code inside __the selector’s rules__ wherever they are included
- Thus try to only create __mixins that take in an argument__, otherwise __you should extend__.

```scss
@mixin no-variable {
  font-size: 12px;
  color: #FFF;
  opacity: .9;
}

%placeholder {
  font-size: 12px;
  color: #FFF;
  opacity: .9;
}
```


## References
- [codecademy: Learn Sass](https://www.codecademy.com/courses/learn-sass/lessons/hello-sass)
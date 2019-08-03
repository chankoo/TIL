#### 19.08.04

## CSS Preprocessing
CSS는 작업이 고도화됨에따라 선택자를 불필요하게 과용하고, 구문의 부재로 구조적인 관리가 불가능한 문제가 있다. 이 때문에 CSS Preprocessor를 도입하므로써 CSS의 문제를 해결한다. 대표적으로 Sass, Less, Stylus 같은 전처리기가 있으며 우리는 Sass를 쓴다

CSS Preprocessor에서는 많은 기능을 제공하여 CSS의 관리를 보다 용이하게 한다. 전처리기에서 작성된 소스코드는 웹에서 동작 가능한 표준의 CSS로 컴파일된다. 컴파일을 위한 명령은 다음과 같다.

`sass --watch <input.scss> <output.css>`

`sass --watch <input/sass/>:</output/css/>`

컴파일의 자동화를 위해 컴파일 프레임워크를 사용하면 좋은데 mac에서는 ruby가 기본으로 설치되어있으므로 보통 ruby를 이용한 컴파일 프레임워크인 Compass를 쓰게된다. CodeKit를 이용하면 이러한 프레임워크와 프로젝트의 설정을 쉽게 관리할 수 있다.

구체적으로 기능을 살펴보자. 우선 __변수__ 를 선언해 중복되는 셀렉터의 남용을 막을 수 있고, __Nested__ 형태를 지원하여 구조적인 관리가 가능하다. 또한 클래스 간의 __상속__ 이 가능하고 __Mixin__ 을 쓰거나 __import__ 를 이용 할 수도 있다. 특히 Sass는 @if / @else 조건문, @for / @each / @while 의 __구문__ 을 제공하고, 사칙연산과 % 연산 등의 __Operators__ 를 이용할 수 있다.

## Variables
        "In Sass if you need to tweak a value, you’ll only have to update it in one place and the change will be reflected in multiple rules."

Declaration
```scss
$translucent-white: rgba(255,255,255,0.3);
```
Reference
```scss
background-color: $translucent-white;
```

### Types of Variables

- Numbers: With 'px', still considered as a number
```scss
    $icon-square-length: 300px;
```
- Strings: With or without quotes
```scss
    "potato"; 'tomato'; span;
```
- Booleans
```scss
    true; false;
```
- null: considered an empty value
```scss
    null;
```
- Lists: can be separated by either spaces or commas

```css
    1.5em Helvetica bold;

    Helvetica, Arial, sans-serif;
```

- Maps: each object is a key-value pair
```css
    (key1: value1, key2: value2);
```

## Nesting 
### Nesting selectors
from scss 
```scss
.parent {
  color: blue;
  .child {
    font-size: 12px;
  }
}
```
to css
```css
.parent {
  color: blue;
}

.parent .child {
    font-size: 12px;
}
```

### Nesting Properties
from scss 
```scss
.parent {
  font : {
    family: Roboto, sans-serif;
    size: 12px;
    decoration: none;
  }
}
```
to css
```css
.parent {
  font-family: Roboto, sans-serif;
  font-size: 12px;
  font-decoration: none;
}
```

### The & Selector in Nesting
        "the & character is used to specify exactly where a parent selector should be inserted. It also helps write psuedo classes in a much less repetitive way."
from scss
```scss
.notecard{ 
  &:hover{
    @include transform (rotatey(-180deg));  
    }
}
```

to css
```css
.notecard:hover {
  transform: rotatey(-180deg);
}
```
## Mixin
        "a mixin lets you make groups of CSS declarations that you want to reuse throughout your site."

```scss
@mixin backface-visibility {
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
  -moz-backface-visibility: hidden;
  -ms-backface-visibility: hidden;
  -o-backface-visibility: hidden;
}
```

from scss
```scss
.notecard {
.front, .back {
    width: 100%;
    height: 100%;
    position: absolute;

    @include backface-visibility;
  }
}
```

to css
```css
.notecard .front, .notecard .back {
  width: 100%;
  height: 100%;
  position: absolute;

   backface-visibility: hidden;
  -webkit-backface-visibility: hidden; 
  -moz-backface-visibility: hidden;
  -ms-backface-visibility: hidden;
  -o-backface-visibility: hidden;
}
```

### Mixins: Arguments
Arguments is a '$visibility'
```scss
@mixin backface-visibility($visibility) {
  backface-visibility: $visibility;
  -webkit-backface-visibility: $visibility;
  -moz-backface-visibility: $visibility;
  -ms-backface-visibility: $visibility;
  -o-backface-visibility: $visibility;
}
```
```scss
@include backface-visibility(hidden);
```

- default arguments
```scss
    @mixin backface-visibility($visibility: hidden) {
    // Backface properties
    }
```
- multiple arguments
```scss
    @mixin dashed-border($width, $color: #FFF) {
  border: {
     color: $color;
     width: $width;
     style: dashed;
  }
}
```
- various form of passing params
```scss
    span { //only passes non-default argument
    @include dashed-border(3px);
    }

    p { //passes both arguments
        @include dashed-border(3px, green);
    }

    div { //passes out of order but explicitly defined
    @include dashed-border(color: purple, width: 5px); 
    }
```
- List Arguments

    " Sass allows you to pass in multiple arguments in a list or a map format."

```scss
@mixin stripes($direction, $width-percent, $stripe-color, $stripe-background: #FFF) {
  background: repeating-linear-gradient(
    $direction,
    $stripe-background,
    $stripe-background ($width-percent - 1),
    $stripe-color 1%,
    $stripe-background $width-percent
  );
}
```

```scss
$college-ruled-style: ( 
    direction: to bottom,
    width-percent: 15%,
    stripe-color: blue,
    stripe-background: white
);
```
then '...' notation
```scss
.definition {
      width: 100%;
      height: 100%;
      @include stripes($college-ruled-style...);
 }
```

### String Interpolation
"the process of placing a variable string in the middle of two other strings."

from scss
```scss
@mixin photo-content($file) {
  content: url(#{$file}.jpg); //string interpolation
  object-fit: cover;
}

//....

.photo { 
  @include photo-content('titanosaur');
  width: 60%;
  margin: 0px auto; 
  }
```

to css
```css
.photo { 
  content: url(titanosaur.jpg);
  width: 60%;
  margin: 0px auto; 
}
```

### The & Selector in Mixins
"& selector gets assigned the value of the parent at the point where the mixin is included."

from scss
```scss
@mixin text-hover($color){
  &:hover {
      color: $color; 
  }
}

//....

.word { //SCSS: 
    display: block; 
    text-align: center;
    position: relative;
    top: 40%;
    @include text-hover(red);
  }
```
to css
```css
  .word{ 
    display: block;
    text-align: center;
    position: relative;
    top: 40%;
  }
  .word:hover{
    color: red;
  }
```


### References

- [Sass(SCSS) 완전 정복!](https://heropy.blog/2018/01/31/sass/)

- [codecademy](https://www.codecademy.com/courses/learn-sass/lessons/hello-sass)
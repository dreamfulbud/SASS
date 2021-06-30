## SASS (Syntactically Awesome Style Sheet)
[참고] https://sass-lang.com/

## VS Code
- Open Preview 단축키 :  `Command + Shift + V`


### SCSS / SASS 차이점
- SCSS : 중괄호 `{}` / 세미콜론 `;` 有
- SASS : 無


### 변수(Variables)
- `$` 변수 정의 : 반복적으로 사용되는 값을 변수로 지정
- `$변수이름 : 속성값;`
- 선언된 블록 `{}` 내에서만 유효범위를 가진다.
  ```scss
    $font: Helvetica, sans-serif;
    $color: #333;

    body{
      font: 100% $font;
      color: $color;
    }
  ```
- 변수 재할당 : 변수에 변수를 할당 할 수 있음
  ```scss
    $red : #ff0000;
    $blue : #000ff;

    $color-primary : $blue;
    $color-danger : $red;

    .box{
      color: $color-primary;
      background: $color-danger;
    }
  ```

### 중첩(Nesting)
- 중괄호`{}`를 이용하여 중첩 처리   

  ```scss
    $color: #333;
    $color2: red;
    section{
      ul{ list-style:none;
        li{ display:inline-block;
          a { color: $color;
            &:hover{ color: $color2; }
          }
        }
      }
    }
  ```

- `&` 이용
  ```scss
  .fs{
    &-small{
      font-size:14px;
    }
    &-medium{
      font-size:16px;
    }
    &-large{
      font-size:24px;
    }
  }
  ```

### `#{}`문자보간
- `#{}`를 이용해서 코드의 어디든지 변수 값을 넣을 수 있음
  ```scss
    $family : unquote("Droid+Sans");
    @import url("http://fonts.googleapis.com/css?family=#{family}");
  ```
  ```css
    @import url("http://fonts.googleapis.com/css?family=Droid+Sans");

  ```
  ```
    * Sass의 내장함수 unquote()는 문자에서 따옴표를 제거함.
  ```

### 부분화(Partials) / 불러오기(Import)
- 부분화 파일 생성 `_base.scss`
  - 앞 부분에 언더바 `_`
  - 별도의 파일로 컴파일 되지 않고 사용됨.
- 부분화 파일 불러오기 `@import 'base', 'main';`
  - 확장자 없음. 언더바`_` 사용 X
  - 여러 파일을 가져올수 도 있음 `,`로 구분
- 순수 css 문법으로 `@import`로 사용하는 상황 
  - 파일 확장자가 `.css`일때
  - 파일 이름이 `http://`로 시작하는 경우
  - `url()` 이 붙었을 경우
  - 미디어쿼리가 있는 경우

### 믹스인(Mixins)
- 브라우저별 접두사를 처리하거나 반복적인 속성을 손쉽게 처리할 수 있게 해주는 역할.
- 반복되는 작업을 최대한 줄여주고, 생산성을 높이며, 작업의 효율성을 극대화!   

  ```css
  @mixin border-radius($radius){
    -webkit-border-radius:$radius;
    -moz-border-radius:$radius;
    -ms-border-radius:$radius;
    border-radius:$radius;
  }
  .class{ @include border-radius(10px); }
  .class2{ @include border-radius(15px); }
  ```

### 확장(Extend) / 상속(Inheritance)
- 중복되는 부분을 묶어서 사용   

  ```scss
    .class{ border:1px solid #ccc; padding:8px; color:#333;}

    .box1{ @extend .class; border-color: green;}
    .box2{ @extend .class; border-color: yellow;}
  ```
  ```css
    .class, .box1, .box2{ border:1px solid #ccc; padding:8px; color:#333;}
    .box1{ border-color: green;}
    .box2{ border-color: yellow;}
  ```

- 컴파일 결과 :  `,`로 구분하는 다중선택지가 만들어짐
- 고려사항
  - 내 현재 선택자가 어디에 첨부될 것인가?
  - 원치 않는 부작용이 초래될 수도 있는가?
  - 이 한번에 확장으로 얼마나 큰 css가 생성되는가?
- **Mixin을 대체기능으로 사용하는 것을 추천**

### 연산자(Operators)
- 사칙연산 `+`, `-`, `*`, `/` 처리 가능.
- 퍼센트 `%` 처리 가능.   

  ```css
    article{
      float:left;
      width: 400px / 1080px * 100%;
    }

  ```

---

### CSS 확장(CSS Extensions)
- 수도 선택자 `:hover`, `:visited`, `:active`, `:link`, `:first-child`, `:last-child`, `:nth-child()`, `::after`, `::before`
  - `&` 기호를 이용하여 처리
- font, background, border, padding, margin 등 동일한 단어가 있는 부분 쉽게 처리 가능

  ```CSS
  $color1: #007bff;
  article{
    &.section{
      font:{
        size: 16px;
        family: roboto, sans-serif;
      }
      table{
        th, td{
          padding:10px
          border: 1px solid rgba(0,0,0,0.5);
        }
        th{
          border:{
            top: 2px solid #ddd;
            bottom:2px solid #eee;
          }
          &:nth-child(2){
            padding-left: 15px;
          }
        }
        tr{
          &:hover{
            background-color:rgba($color1, 0.6);
          }
        }
      }
    }
  ```

- `&` 위치 구별 사용
  - `&`은 부모 선택자를 의미.
  - 부모 선택자와 해당 선택자를 묶는 역할 / 부모 선택자를 다른 선택자의 하위 선택자로 처리 할수 있음.
    ```CSS
      section{
        a{
          &:hover{} /* section a:hover */
        }
        .container & {} /* .container section */
      }
    ```

### 주석처리
- `/* 주석내용 */` : SASS 컴파일 결과물인 CSS에 그대로 반영
  - 저작권 또는 CSS 버전 등을 표시 할때 사용
- `// 주석내용 ` : CSS에 반영 X, 유지보수 할때 사용.
- **SASS에 한글로 주석을 달면 컴파일시 에러 발생 주의**

### 변수 (`$`)
- **1.전역변수**
  - 선택자 외부에 별도로 선언을 하거나, 지역변수 부분에 `!global`이라고 지정
- **2.지역변수**
  ```CSS
    $max-width:100%; /*전역변수, 별도 _variables.css 파일을 만들어 분리하는 것이 좋음. */
    .container{
      $width: 2px !global; /* 내부에서 사용하다가 전역변수로 선언하고 싶을 때*/
      border:$width*5 solid #ddd;
      $bg-color: #f5f5f5; /*지역변수*/
      background-color: $bg-color;
      width: $max-width/2;
    }
  ```
  ```
    (!참고) SASS에서 하이픈(-) 언더바(_) 둘 모두 동일한 기호로 인식!
  ```

### SassScript
- 사칙연산 가능.
- 7가지 데이터 타입 지원
  - 숫자 number
  - 문자열 String
    - 일반 텍스트, 쌍따옴표`""`, 따옴표`''` 모두 동일하게 취급.
  - 색상값 color
  - 불린함수 Boolean(true, false)
  - 널값 null
  - 수치값 lists of value
    - ex) 1.5em 1em 0 2em, roboto, sans-serif
    - 배열의미 margin, padding, font...
    - 배열 중간 null값 사용할수 없음.
  - 맵스 maps : 다른 프로그래밍 언어에서 연관 배열 또는 해쉬라고 불리며, SASS에서는 키 값에 할당된 값의 배열을 의미
    - ex) value1, key2: value2
    - 키 값 구분 콤마` , ` 이용
      ```
        $map : (key1: value1, key2: value2, key3: value3);
      ```


---
### 연산
1. 숫자 연산
 - `+ - / * == != < > <= >=`
 - `*` : 하나 이상의 값이 반드시 숫자 ex) 10px * 10 **10px * 10px 에러!**
 - `/` : 오른쪽 값이 반드시 숫자 ex) 10px/2
    - 예외) font 속성 `/`
      ```CSS
        a{ font:10px/8px; } /* font-size:10px; line-height:8px; */
      ```
    - `/` 나누기 연산 기능으로 사용하려면 다음과 같은 조건 충족
      - 값 또는 그 일부가 변수에 저장되거나 함수에 의해 반환되는 경우.
      - 값이 `()`로 묶여있는 경우
      - 값이 다른 산술 표현식의 일부로 사용되는 경우 (ex. 더하기 연산과 같이 사용)
- 상대적 단위의 연산의 경우 `clac()`로 연산
2. 색상 연산
  - RGBA의 Alpha 값은 더하거나 빼더라도 연산이 되지 않으며, 서로 동일해야 다른 값의 연산이 가능함.
  - Alpha 값을 연산하기 위한 다음과 같은 색상 함수를 사용할수 있음.
    - `opacify()`, `transparentize()`
    ```scss
      $color : rgba(10, 20, 30, .5)
      color : opacify($color, .3); //30% 더 불투명하게 / 0.5 +0.3
      background-color : transparentize($color, .2) //20% 더 투명하게 / 0.5 - 0.2
    ```
3. 문자열 연산
  - 보통 `+` 기호를 이용해서 결합.
  ```CSS
    p::before{ content: "Foo " + Bar;}
  ```
4. 논리 연산
  - and : 그리고 / or : 또는 / not : 부정 
5. 삽입
  - 변수명 자체를 불러오려는 경우 삽입을 이요하여 철.
  - `#{변수명}`
  ```CSS
    $name:foo;
    $attr:border;
    p.#{$name}{
      #{$attr}-color:blue;
    }
    /*
      p.foo { border-color:blue; }
    */
    p{
      $font-size:12px;
      $line-height: 30px;
      font: #{$font-size}/#{$line-height};
    }
    /*
      p{ font: 12px/30px; }
    */
  ```
  - 삽입을 이용하면 CSS의 속성값 뿐만 아니라 문자열까지 다양하게 처리 가능.
5. 변수 기본값 설정 `!default`
  - 동일한 변수 이름이 존재할 때, 마지막에 선언된 값이 최종 결과값이 됨. 하지만, `!default`로 선언하게 되면 그 값은 기본값 즉 첫번째 값이 되어, 해당 변수에 다른 값이 지정되면, 최종값은 `!default`로 지정된 값이 아닌, 마지막에 선언된 값이 결과값으로 됨.
  - 변수와 값을 설정하겠지만, 혹시 기존 변수가 있을 경우는 현재 설정하는 변수의 값은 사용하지 않겠다.


---
### 규칙과 지시어
1. **@import**
  - css 파일들을 별도의 모듈로 만들어 파일로 불러 올 수 있음.
  - 해당 파일명에는 일반 파일명도 있을 수 있고, 경로, URL을 통해 불러 올 수도 있음.

  - 일반 : 확장자까지 적어줄것.
  - .sass, .scss : 확장자는 명시하지 않더라도 가능.
    ```CSS
      @import "layout.css"
    ```
  - 일반 : 확장자까지 적어줄것.
  - .sass, .scss : 확장자는 명시하지 않더라도 가능.
  - sass 파일 내부에 포함된 별도 파일이 컴파일 되지 않도록 `_파일명.scss`와 같이 언더바`_` 적용
    ```CSS
      /* _font.scss, _base.scss*/
      @import "font", "base";
    ```
  - 중첩을 이용해서 해당 선택자 부분에만 해당 속성을 불러올 수 있음.
   ```CSS
     /* _box.scss */
     .box1{margin:0; padding:0;}

     /* style.css */
     .boxstyle{ @import "box"; font-size: 12px

     /* 결과값 :
      .boxstyle{ font-size:12px; }
      .boxstyle .box1 {margin:0; padding:0;}
     */
   ```
  - `@import` 중첩 문법 구문에만 사용 가능. `@mixin`이나 if 구문에서는 사용할 수 없음.
  - 외부 파일을 불러오는 용도로 사용.
---
2. **@media**
- 디바이스별 화면 구성 또는 PC 모니터, 스마트폰, 태블릿과 같은 스마트 기기 화면, 프린터 등에 적합하게 보여지기를 원할 때 사용.
- 반응형 웹사이트 제작할 때 많이 사용.
```CSS
  $media:screen;
  $feature: landscape;
  $value:500px;
  article{
    width: $value;
    @media #{$media} {
      @media (orientation:$feature){
        width: $value + 200;
      }
    }
  }
```
---
3. **@extend**
- 기존의 설정되어 있는 속성 재사용 + 다른 선택자의 기능을 확장.
  ```CSS
    .box1{ height:300px; padding:50px 80px; color:#333;}
    .box2{ @extend .box1; padding-top:70px;}
  ```
- extend에서만 작동하는 선택자 `%` :  투명 선택자
  - SASS 파일에만 존재, 컴파일시 CSS 파일에는 없음. @extend를 사용해야 작동
- extend로 불러온 선택자 내부에 아무런 속성이 없을 경우 에러 발생. `!optional` 로 예방
  ```css
    p{ @extend .box1 !optinoal;}
  ```
- **주의)** `@media` 내부에서 extend를 사용하게 되면 에러발생.
---
4. **@at-root**
- 독립된 선택자로 끌어올려야 하는 경우 사용.
- 해당 선택자 사용 시 부모선택자의 위치로 이동
```css
  .type{ 
    $width:100px;
    $height:200px;
    
    padding-top:10px;

    .item{
      width:$width;
      height:$height;
    }
    @at-root .box{
      width:$width;
      height:$height;
      background: #eee; 
      padding:10px;
    }
  }
  /* 결과값
  .type{ padding-top:10px;}
  .type .item{  width:100px; height:200px;}

  .box{ width:100px; height:200px; background: #eee; padding:10px;}
  */
```
- ex) `@at-root(without:media)`- 해당 선택자와 지정한 속성은 @media 내부에서 제거되어 부모선택자로 지정.
- 처음부터 사용 X, 수정시 사용하는 것.

---
## 함수
- **연산된 특정 값**을 `@return` 지시어를 통해 반환
  ```scss
    @function 함수이름($매개변수){
      @return 값
    }
  ```
  ```scss
    @function colums($numbers:1, $columns:12, $width:1200px){
      @return $width * ($numbers / $columns);
    }
    .container{
      $width:1200px;
      width:$width;
      .item:nth-child(1){
        width:colums();
      }
      .item:nth-child(2){
        width:colums(8);
      }
      .item:nth-child(3){
        width:colums(3);
      }
    }
  ```
  ```css
    /* CSS 결과 */
    .container {
      width: 1200px;
    }
    .container .item:nth-child(1) {
      width: 100px;
    }
    .container .item:nth-child(2) {
      width: 800px;
    }
    .container .item:nth-child(3) {
      width: 300px;
    }
  ```
- 동일한 이름의 내장함수가 이미 있을 수 있음! 접두어를 붙여 만들기. ex) cumstom-my-red()


### 제어문 및 표현식
1. **if(...)**
- 참과 거짓에 대한 조건에 따라 정의를 하는 곳에만 사용.
- `if(조건, 표현식1, 표현식2)` 삼항연산자와 유사.
```
  if(true, 1px, 2px) => 1px
  if(false, 1px, 2px) => 2px
```

2. **@if**
- 참에 대한 값에만 결과값을 주는 것을 의미
- @if - @else if
```scss
  $weight:bold;
  .txt1{
    @if $weight == bold{font-weight:bold;} //해당조건 만족
    @else if $weight == light {font-weight:100;}
    @else if $weight == heavy {font-weight:900;}
    @else {font-weight:normal;}  
  }
```
```scss
  @mixin positionCenter($w, $h, $p: absolute){
    @if(
      $p == absolute 
      or $p == fixed
      or not $p == relative
      or not $p == static
    ){
      width: if(unitless($w), #{$w}px, $w); //unitless : 단위 유무 확인
      width: if(unitless($h), #{$h}px, $h);
      position: $p;
      top:0;
      bottom:0;
      left:0;
      right:0;
      margin:auto;
    }
  }
```

3. **@for**
- 시작과 끝이 있는 구문
- `@for $var from <start> through <end>`
  ```scss
    /*SASS*/
    @for $i from 1 though 3{
      .col#{$i} { width:(100/5*$i)+px; }
    }
  ```
  ```css
    /*CSS*/
    .col1 { width:20px; }
    .col2 { width:40px; }
    .col3 { width:60px; }
  ```
- `@for $var from <start> to <end>`  
  ```scss
    /*SASS*/
    @for $i from 1 to 3{
      .col#{$i} { width:(100/5*$i)+px; }
    }
  ```
  ```css
    /*CSS*/
    .col1 { width:20px; }
    .col2 { width:40px; }
  ```

4. **@each**
- 여러 개의 값을 변수에 각각 대입하는 것을 의미
- `@each $var in 변수값1, 변수값2, 변수값3, ...`
```scss
  /*SASS*/
  @eash $usr in hj, js, yj{
    .#{$usr}-img{ background-image: url('/img/#{$usr}.png'); }
  }
```
```css
  /*CSS*/
  .hj-img { background-image: url('/img/hj.png'); }
  .js-img { background-image: url('/img/js.png'); }
  .yj-img { background-image: url('/img/yj.png'); }
```

```scss
  /*map 속성이용*/
  $map : (usr1:hj, usr2:js, usr3:yj);

  @each $key, $usr in $map{
      $map--key-list : map-keys($map);  
      //map-keys() : key값의 리스트 반환 
      //map-value() : value값의 리스트 반환
      
      $index : index($map--key-list, $key); 
      //index(list, value) : 리스트에서 특정값이 몇번째인지 반환
      
      .#{$usr}-img{ 
          width:100px * $index;
          background-image: url('/img/#{$usr}.png'); 
      }
  }
```
```css
  /*CSS*/
  .hj-img {
    width: 100px;
    background-image: url("/img/hj.png");
  }

  .js-img {
    width: 200px;
    background-image: url("/img/js.png");
  }

  .yj-img {
    width: 300px;
    background-image: url("/img/yj.png");
  }
```

- `@each`는 한 개 이상의 변수값을 지정할 수 있음.
```scss
  /*SASS*/
  @each $usr, $color, $value in (hj, black, 1), (js, red, 2), (yj, blue, 3){
    .#{$usr}-img{
      background-image: url('/img/#{$usr}.png');
      border $value+px solid $color;
    }
  }
  /*map 속성이용*/
  $hj : (hj, black, 1);
  $js : (js, red, 2);
  $yj : (yj, blue, 3);
  @each $usr, $color, $value in $hj, $js, $yj{
    .#{$usr}-img{
      background-image: url('/img/#{$usr}.png');
      border $value+px solid $color;
    }
  }
```
```css
  /*CSS*/
  .hj-img { background-image: url('/img/hj.png'); border: 1px solid black;}
  .js-img { background-image: url('/img/js.png'); border: 2px solid red;}
  .yj-img { background-image: url('/img/yj.png'); border: 3px solid blue;}
```

5. **@while**
- 어떠한 조건을 충족할 때까지 값을 반복하는 것을 의미
- 무한 루프에 빠질 수 있음으로 주의할 것!
```scss
  $i:1;
  @while $i <5{
    css 속성;
    $i : $i +1;
  }
```
```scss
  /*SASS*/
  $i:1;
  @while $i < 3{
    .col#{$i}{width:50*$i +px}
    $i : $i +1;
  }
```
```css
  /*CSS*/
  .col1{width:50px;}
  .col2{width:100px;}
  .col3{width:150px;}
```
---
### Mixin (@mixin - @include)
- 반복적으로 사용되는 부분을 미리 설정해 놓고 새롭게 만드는 선택자에 해당 mixin을 호출.
- 매개변수(Parameters)란, 변수의 한 종류로, 제공되는 여러 데이터 중 하나를 가르키기 위해 사용된다.
- 제공되는 여러 데이터들을 전달인수(argumanet)라고 부른다.
```scss
  @mixin 믹스인이름($매개변수1 : 기본값1, $매개변수2 : 기본값2...){ //기본값 설정
    스타일;
  }
  .class{
    @include 믹스인이름; //지정된 인수가 없을경우 기본값으로 설정됨.
  }
  .class{
    @include 믹스인이름($매개변수2 : 새로운값); //키워드 인수
  }
```
```scss
  @mixin boldtext($size, $color){
    font:{
      family: 'Malgun Gothic', sans-serif;
      weight: bold;
      size: $size;
    }
  }

  .box1{
    @include boldtext(24px, #000);
  }

  .box2{
    @include boldtext($color:red, $size:36px); //변수명과 값을 같이 써주게 되면, 값의 위치 변경 가능.
  }
```
- 가변인수 : 매개변수 뒤 `...`
  ```scss
    @mixin var ($w, $h, $bg...){
      width:$w;
      height:$h;
      background:$bg;
    }

    .box{
      @include var(
        100px, 
        200px, 
        url("img/a.png") no-repeat 10px 20px,
        url("img/b.png") no-repeat,
        url("img/c.png") ,
      );
    }
  ```
  ```scss
    @mixin font($style:normal, $weight:normal, $size:16px, $famiy:sans-serif){
      font :{
        style : $style;
        weight: $weight;
        size: $size;
        family: $family;
      }
    }

    //매개변수 순서와 개수에 맞게 전달
    div{
      $font-values:italic, bold, 16px, sans-serif;
      @include font($font-values...);
    }

    //필요한 값만 키워드 인수로 변수에 담아 전달
    span{
      $font-values: (style:italic, size:22px);
      @include font($font-values...);
    }
    //필요한 값만 키워드 인수로 전달
    a{
      @include font((weight:900, family:monospace)...);
    }
  ```
- 벤더 프리픽스에 활용하여 사용
  - 크롬/사파리 `-webkit-`
  - 파이어폭스 `-moz-`
  - 인터넷 익스플로러 `-ms-`
  - 오페라  `-o-`

- text-shadow와 box-shadow와 같이 값을 여러번 적용할때 사용
```
  @mixin box-shadow($shadows...){
    -moz-box-shadow:$shadows;
    -webkit-shadow:$shadows;
    box-shadow:$shadows;
  }
  .shadows{
    @include box-shadow(0px 4px 5px #666, 2px 6px 10px #999);
  }
```
```
  @mixin colors($text, $background, $border){
    color: $text;
    background: $background;
    border-color: $border;
  }
  $values: #ff000, #00ff00, #0000ff;

  .primary{
    @include color($values...);
  }

  $value-map (text: #00ff00, background: #0000ff, border: #ff0000);
  .secondary{
    @include colors(value-map...);
  }
```
- **@content** : mixin에 정의되지 않은 값을 추가할때 사용 (★응용고민해보기)
```scss
  /*SASS*/
  @mixin conbox{
    header{ height: 100px;
      @content;
    }
  }

  @include conbox{
    #logo{ border:1px solid red; }
  }
```
```css
  /*CSS*/
    header{ height: 100px;}
    header #logo{ border:1px solid red; }
  }
```
```scss
  @mixin txt($weight){
    color: white;
    @if $weight == bold {font-weight:bold;}
    @else if $weight == light {font-weight:100;}
    @else if $weight == heavy {font-weight:900;}
    @else {font-weight:normal;}
  }
  .txt1 { @include txt(bold); }
  .txt2 { @include txt(light); }
  .txt3 { @include txt(heavy); }
  .txt4 { @include txt(none); }
  .txt5 { @include txt(normal); }
```
---
### Output style
1. Nested : 중첩스타일
2. Expand : 가장 보기에 좋은 결과물.
3. Compact : CSS 속성을 한줄에 보여줌.
4. Compressed : **.min.** 형태

---

## Sass 내장함수
- https://sass-lang.com/documentation/modules
- `[]`는 선택 가능한 인수
- Zero-based numbering을 사용하지 않음.

### 색상 함수
- `mix($color1, $color2)` : 두개의 색을 섞음
- `lighten($color, $amount)` : 더 밝게
- `darken($color, $amount)` : 더 어둡게
- `invert($color)` : 색상 반전.
- `rgba($color, $alpha)` : 투명도 변경
- `opacify($color, $amount)` / `fade-in($color, $amount)` : 더 불투명하게
- `transparentize($color, $amount)` / `fade-out($color, $amount)`: 더 투명하게

### 문자 함수
- `unquote($string)` : 따옴표 제거
- `quote($string)` : 따옴표 추가
- `str-slice($string, $start-at, [$end-at])` : 문자에서 특정 문자(몇 번째 글자부터 몇 번째 글자까지) 추출

### Map 함수
- `map-keys($map)` : Map에서 모든 key값을 리스트로 반환
- `map-value($map)` : Map에서 모든 value값을 리스트로 반환

### 관리(Introspection) 함수
- `unit($number)` : 숫자 단위 반환
- `unitless($number)` : 숫자에 단위 유무 반환




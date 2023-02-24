# SCSS

## 주석
```plaintext
기존 css 에서 사용하던 표준 주석인 /* */ 사용가능하다.
그리고 // 식의 주석처리고 가능하다.
```
```plaintext
하지만 css 로 컴파일이 된 후 보게 된다면
// 로 주석을 처리한 부분은 삭제처리가 되며 아예 안보이게 된다.
/* */ 로 주석을 처리한 부분은 주석처리가 된 상태인 정상적인 상태로 보인다.
```

## 중복
```scss
.container {
  ul {
    li {
      font-size: 40px;
      .name {
        color: royalblue;
      }
      .age {
        color: orange;
      }
    }
  }
}
```
```css
.container ul li {
  font-size: 40px;
}
.container ul li .name {
  color: royalblue;
}
.container ul li .age {
  color: orange;
}
```
```plaintext
scss의 중첩 기능을 사용하면 위와 같이 css에서 중복으로 입력 한 부분을
중첩으로 사용이 가능하다.
```

```scss
.container {
  > ul {
    li {
      font-size: 40px;
      .name {
        color: royalblue;
      }
      .age {
        color: orange;
      }
    }
  }
}
```
```css
.container > ul li {
  font-size: 40px;
}
.container > ul li .name {
  color: royalblue;
}
.container > ul li .age {
  color: orange;
}
```
```plaintext
.container 의 자식선택자인 ul태그를 자식선택자이다 라고 표현을 하고싶으면
> 를 사용하여 표현을 해준다. 그러면 css에서는 위와같이 표현이 된다.
```

## 상위 선책자 참조
```scss
.btn{
    position: absoule;
    &.active{
        color: red;
    }
}

.list {
    li {
        &:last-child{
            margin-right: 0;
        }
    }
}
```
```css
.btn {
  position: absoule;
}
.btn.active {
  color: red;
}

.list li:last-child {
  margin-right: 0;
}
```
```plaintext
위 와같이 상위 선택자를 참조 하고 싶을 경우 & 를 사용한다.
그렇게 되면 &가 포함되어 있는 영역의 선택자로 치환이 된다.
```

## 중첩된 속성
```scss
.box{
    font: {
       size: 10px;
       weight: bold;
       family: sans-serif;
    };
    margin: {
        top: 10px;
        left: 20px;
    };
    padding: {
        top: 10px;
        bottom: 20px;
        left: 30px;
        right: 50px;
    };
}
```
```css
.box {
  font-size: 10px;
  font-weight: bold;
  font-family: sans-serif;
  margin-top: 10px;
  margin-left: 20px;
  padding-top: 10px;
  padding-bottom: 20px;
  padding-left: 30px;
  padding-right: 50px;
}
```
```plaintext
속성 부분도 네임스페이스가 동일하다면 괄호를 열어서 중첩으로 사용이 가능하다
괄호가 끝나는 지점에 ; 을 넣어줘야한다.
```

## 변수
```scss
$size: 100px;

.container {
    position: fixed;
    top: $size;
    .item{
        width: $size;
        height: $size;
        transform: translaetX($size);
    }
}
```
```css
.container {
  position: fixed;
  top: 100px;
}
.container .item {
  width: 100px;
  height: 100px;
  transform: translaetX(100px);
}
```
```plaintext
scss는 위 와같이 변수를 사용해서 값을 입력 할 수 있다. $size: 100px; 이렇게 변수를 설정해두고 사용을 하면된다.
장점으로는 변수로 지정해놓은 값은 일일이 다 바꿀 필요없이 변수의 값만 변경을 해주면 전체변경이 된다.
변수의 유효범위는 사용자가 지정을 할 수 있으며 위 와같은 상황에서는 전역변수가 되며 만약 
.container 안에서만 사용을 하고 싶다면 .container 안에서 변수 선언을 해주면 된다.
```
```scss
$size: 100px;

.container {
    position: fixed;
    top: $size;
    .item{
        $size: 200px;
        width: $size;
        height: $size;
        transform: translaetX($size);
    }
}
```
```css
.container {
  position: fixed;
  top: 100px;
}
.container .item {
  width: 200px;
  height: 200px;
  transform: translaetX(200px);
}
```
```plaintext
변수 같은 경우 위 와같이 재할당이 가능하다. javascript의 lat과 비슷하다고 보면 된다.
한가지 유의 해야할 점은 재할당은 .item에서 재할당이 되었지만 만약 
.item 범위에 벗어난 밑에 부분에 $size를 사용을 한다면 값은 100px이 아니라
200px로 지정이 된다. 재할당한 부분 범위를 벗어나도 이미 재할당이 됐다면 그 값으로 사용이 된다.
```

## 사칙연산
```scss
div {
    width: 20px + 20px;
    height: 40px - 10px;
    font-size: 10px * 2;
    margin: 30px / 2;
    padding: 20px % 7;
}
```
```css
div {
  width: 40px;
  height: 30px;
  font-size: 20px;
  margin: 30px/2;
  padding: 6px;
}
```
```plaintext
위를 보게 된다면 나누기 부분이 제대로 입력이 안된걸로 판단이 된다. 그 이유는 scss에서 
단축속성을 / 로 구분을 하기 때문이다
값의 ( )를 감싸주면 제대로 동작이 가능하다. (30px / 2)와 같이 작성을 해주면 된다. 만약 괄호를 사용하기 싫타면
변수를 입력해서 사용을하면 된다. 또 다른 방법 하나는 다른 사칙연산과 같이 사용을 하면 된다.
```
```plaintext
scss에서는 다른 단위의 계산은 불가능 하지만 css에서는 사용이 가능하다 calc() 함수를 사용하면 된다.
```

## 재활용
```scss
@mixin center {
    display: fiex;
    justify-content: conter;
    align-items: canter;
}
.container{
    @include center;
    .item{
        @include center;
    }
}
.box {
    @include center;
}
```
```css
.container {
  display: fiex;
  justify-content: conter;
  align-items: canter;
}
.container .item {
  display: fiex;
  justify-content: conter;
  align-items: canter;
}

.box {
  display: fiex;
  justify-content: conter;
  align-items: canter;
}
```
```plaintext
@mixin 를 사용해서 변수 이름을 설정하고 괄호 안에 속성을 기입한다음 사용한다.
사용을 하기 위해서는 @include 변수명 을 입력하여서 사용한다.
```

```scss
@mixin box($size: 100px, $color: red) {
    width: $size;
    height: $size;
    background-color: $color;
}

.comtainer{
    @include box(200px, blue);
    .item{
        @include box($color: green);
    }
}
.box{
    @include box;
}
```
```css
.comtainer {
  width: 200px;
  height: 200px;
  background-color: blue;
}
.comtainer .item {
  width: 100px;
  height: 100px;
  background-color: green;
}

.box {
  width: 100px;
  height: 100px;
  background-color: red;
}
```
```plaintext
@mixin으로 만든 변수를 다른 속성으로 다시 작성을 하고 싶다면. 인수를 사용하여 설정 값을 임의로 변경을 할 수 있다.
위와 같이 변수 옆에 인수를 지정해준다. 사용할 경우 원하는 수치를 입력해서 사용을 하면된다.
기본값 설정은 사용에 맞게 설정을 하면 된다. 기본값을 사용을 하면 수치변경을 사용 안할 시 
기본으로 설정한 크기가 입력되게 된다.
여러개도 가능하며 순서를 맞춰서 사용을 하면된다. 만약 매개변수는 2개를 사용중인데 뒤에 매개변수만
사용을 하고 싶다면 사용할 매개변수 이름을 직접적으로 명시를 해주면 된다.
```

## 반복문
```scss
@for $i from 1 through 10 {
    .box:nth-child(#{$i}){
        width: 100px * $i;
    }
}

```
```css
.box:nth-child(1) {
  width: 100px;
}

.box:nth-child(2) {
  width: 200px;
}

.box:nth-child(3) {
  width: 300px;
}

.box:nth-child(4) {
  width: 400px;
}

.box:nth-child(5) {
  width: 500px;
}

.box:nth-child(6) {
  width: 600px;
}

.box:nth-child(7) {
  width: 700px;
}

.box:nth-child(8) {
  width: 800px;
}

.box:nth-child(9) {
  width: 900px;
}

.box:nth-child(10) {
  width: 1000px;
}
```
```plaintext
제로 베이스 X
$ 변수 이름 정의
form 뒤에 숫자는 몇번 째 숫자부터 시작을 할 것이냐
through 뒤 숫자는 반복 횟수
scss 보관 분법 -> #{ }
```

## 함수
```scss
@mixin center {
    display: flex;
    justify-content: center;
    align-items: center;
}
@function ratio($size, $ratio){
    @return $size * $ratio
}
.box{
    $width: 100px;
    
    width: $width;
    height: ratio($width, 9/16);
    @include center;
}
```
```css
.box {
  width: 100px;
  height: 56.25px;
  display: flex;
  justify-content: center;
  align-items: center;
}
```
```plaintext
@mixin 같은경우는 모음집으로 생각하면 된다. 재활용에 쓰인다.
@function 실제로 어떤 값을 따로 연산을 해서 반환된 값을 사용하는데 쓰인다.
```

## 색상 내장 함수
```scss
.box{
    $color: royalblue;
    width: 200px;
    height: 100px;
    margin: 20px;
    border-radius: 10px;
    background-color: $color;
    &:hover{
        background-color: darken($color, 10%);
    }
    &.built-in{
        background-color: saturate($color, 10%);
    }
}
```
```css
.box {
  width: 200px;
  height: 100px;
  margin: 20px;
  border-radius: 10px;
  background-color: royalblue;
}
.box:hover {
  background-color: #214cce;
}
.box.built-in {
  background-color: #3664ec;
}
```
```plaintext
mix($cloor, blue);  mix() 내장 함수는 첫번째 인수의 색상과 두번재 인수의 색상을 합친 색이 나온다. 

lighten($cloor, 10%) 첫번째 인수의 색상을 두번째 인수의 크기만큼 더 밝게 만들어 주는 함수.

darken($cloor, 10%)첫번째 인수의 색상을 두번째 인수의 크기만큼 더 어둡게 만들어 주는 함수.
darken()는 어느상황에 많이 쓰이냐면 :hover 를 사용할 때 많이 쓰인다. 버튼 만들 때 유용하게 쓰인다.

saturate($cloor, 10%) 첫번째 인수의 색상을 두번째 인수의 크기만큼 채도를 더올려주는함수.

desaturate($cloor, 10%) 첫번째 인수의 색상을 두번째 인수의 크기만큼 채도를 더내려주는함수.

grayscale($color) 인수의 색상을 회색으롤 만들어 준다.

invert($color) 인수의 색상을 반전시킨다.

rgba($color, .5) 첫번째 인수의 색상을 두번째 인수 크기만큼 투명도를 조절한다.
```

## 가져오기

```scss
@import "./sub","./sub2.scss";

$color: royalblue;

.container{
  h1{
    color: $color;
  }
}
```
```plaintext
url 과 확장자를 따로 명시를 안해도 정상적으로 동작을 한다.
, 통해서 다른 파일도 가져오기가 가능하다.
```

## 반복문 @each
```scss
$list: orange, red, yellow;
$map: (
    o: orange,
    r: red,
    y: yellow
);

@each $c in $list {
    .box{
        color: $c;
    }
}
```
```css
.box {
  color: orange;
}

.box {
  color: red;
}

.box {
  color: yellow;
}
```
```plaintext{
 @each 와 변수 list를 사용하여 반복문을 만들 수 있다.
}
```
```scss
$list: orange, red, yellow;
$map: (
    o: orange,
    r: red,
    y: yellow
);

@each $k, $v in $map {
    .box-#{$k}{
        color: $v;
    }
}
```
```css
.box-o {
  color: orange;
}

.box-r {
  color: red;
}

.box-y {
  color: yellow;
}
```
```plaintext
@each 와 변수인 map을 사용하여 key,value를 사용하여 반복문을 만들 수 있다.
```

## 재활용 @content
```scss
@mixin left-top {
    position: absolute;
    top: 0;
    left: 0;
    @content;
}
.container {
    width: 100px;
    height: 100px;
    @include left-top;
}
.box {
    width: 200px;
    height: 200px;
    @include left-top{
        bottom: 0;
        right: 0;
        margin: auto;
    }
}
```
```css
.container {
  width: 100px;
  height: 100px;
  position: absolute;
  top: 0;
  left: 0;
}

.box {
  width: 200px;
  height: 200px;
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  margin: auto;
}
```
```plaintext
@mixin 사용 시 @content를 사용하게 되면 재활용을 하면서 다른 부분을 추가할 수 있다.
예시를 보게된다면 .box 부분에서 @include를 사용 하고 추가적으로 속성을 넣을 수 있다.
그 이유는 @mixin생성 당시 안에 @content가 있기 때문에 가능하다.
```




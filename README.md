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
위를 보게 된다면 나누기 부분이 제대로 입력이 안된걸로 판단이 된다. 그 이유는 scss에서 단축속성을 / 로 구분을 하기 때문이다
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
기본값 설정은 사용에 맞게 설정을 하면 된다. 기본값을 사용을 하면 수치변경을 사용 안할 시 기본으로 설정한 크기가 입력되게 된다.
여러개도 가능하며 순서를 맞춰서 사용을 하면된다. 만약 매개변수는 2개를 사용중인데 뒤에 매개변수만 사용을 하고 싶다면
적용할 매개변수 이름을 직접적으로 명시를 해주면 된다.

```

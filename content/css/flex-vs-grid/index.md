---
title: Flex와 Grid의 차이점
date: "2022-01-14T22:12:03.284Z"
description: " Flex와 Grid의 차이점에 대해 알아보자"
thumbnail: "./thumbnail.png"
---


## 서론

프론트엔드 레이아웃 구성에 정말 유용하게 사용되는 Flex와 Grid.

하지만, 둘의 차이점을 제대로 알지 못하면 언제, 어디서, 어떻게 flex와 grid를 사용해야 하는지 모호할 수 있다.

이번 포스팅에서는 flex와 grid의 차이점과 기본적인 속성들을 알아보고 언제 사용하면 좋을지 소개해보려 한다.

---


## 1. Flex와 Grid의 차이점


MDN에서의 둘의 차이점을 다음과 같이 정리한다.

```
"Flex는 1차원의 행 혹은 열의 레이아웃을 염두하고 설계되었으며,
```

Grid는 행과 열 모두를 염두한 2차원 레이아웃을 고려해서 설계되었다."

<img src="https://i1.wp.com/css-tricks.com/wp-content/uploads/2017/03/both-grid-and-flex.png?ssl=1" alt="css-grid-replace-flexbox" />


이를 시각화해서 표현했을 때, 오랜지 색 박스 요소는 grid로 구현되었고, 빨간 색 박스 요소는 flex로 구현이 되었다. 이는 flex는 행 또는 열 방향으로 1차원적인 나열을 구현할 수 있지만, grid는 행과 열 모두를 조절한 2차원 나열을 구현할 수 있다는 의미이다.


​flex와 grid에서 제공하는 속성을 살펴보면 더 자세히 알 수 할 수 있다. 아래 내용부터 사용할 기본적인 레이아웃 코드는 아래와 같다.

```html
<div class='container'>
   <div>1</div>
   <div>2</div>
   <div>3</div>
   <div>4</div>
   <div>5</div>
</div>
```

---

## 2. Flex 속성

### 1) 기본적인 속성, flex-direction/  flex-wrap/flex-align

​flex에서 column과 row로 나열하는 방법으로 flex-direciton을 사용한다.

```html
.container{
  display: flex
  flex-direction: column; //flex-direction: row
}
```
<img src="https://postfiles.pstatic.net/MjAyMTEwMThfMTc5/MDAxNjM0NTIyOTcwMzI3.GifCn1C8yTtMPQtj1Rv_rfFQDZzWpbV-V5gmjvqFalQg.-Wn5KlSKmqIIgyLQcw2wWnDxcyo8QBIucttTGPsFNvQg.PNG.mitty0304/image.png?type=w966" alt="flex-direction 결과" />

flex-wrap은 요소가 특정 영역를 벗어날 경우, 강제로 한줄에 배치되게 할 것인지 아니면 줄바꿈으로 여러행으로 나누어 표현할 것인지를 결정한다.

```css
.container{
  width: 500px;
  display: flex;
  border: 1px solid black;
  flex-wrap: nowrap; //wrap
  gap: 20px;
}

.container div{
  background-color: #d383f8;
  width: 200px;
}
```

만약 flex-wrap을 nowrap으로 지정한다면 다음처럼 한 줄로 표현이 되지만, wrap으로 설정하면 container 영역을 벗어났을 경우 줄바꿈으로 처리가 된다.

​<img src="https://postfiles.pstatic.net/MjAyMTEwMTlfMTQ4/MDAxNjM0NTc2MDM3MzIz.bmT1s9ygl9tJKXWby5uqOVNhuIJkiydqoSHxnqG5xYYg.x0GldfLzmszm-g4l6AyHrG6a_SD9RkF7TZJNsM-oyY0g.PNG.mitty0304/image.png?type=w966" alt="flex-nowrap"/>
<img src="https://postfiles.pstatic.net/MjAyMTEwMTlfMTE1/MDAxNjM0NTc2MDY0NDMw.GHWbNQEdZYdQh-LDPYH3_mDkpC5ENFPG3h1riRqL7Wog.AGzcXiRQ_ihtiCSoGPnP-D-gJUlQq6h2nHL83pJoLt4g.PNG.mitty0304/image.png?type=w966" alt="flex-wrap">

flex-align은 flex 내부 항목의 열의 정렬을 결정하는 방식이다. 기본적으로 stretch로 설정이 되어있으며, 그밖에 flex-start / flex-end / center가 있다.

<img src="https://postfiles.pstatic.net/MjAyMTEwMTlfMTEz/MDAxNjM0NTc2NjYyMTQ0.XPBO5JgPwe_VlF1BfUuJ-kyVk01F7h1ucnbM16vA19Ig.X7j8b1DCNlfYWhrEVAoiZvnBNGLtE-4fnnKqvL7hqaog.PNG.mitty0304/image.png?type=w966" alt="align-items" />

---

## 3. Grid 속성

### 1) 기본적인 속성, grid-template-columns / grid-template-rows / repeat


Grid의 행과 열은 grid-template-columns와 grid-template-rows 프로퍼티로 정의한다.

그리고, Grid에서의 비율로는 fr 단위를 사용할 수 있다. 일반적인 px 단위와는 달리, fr 단위를 사용해서 행과 열을 가변적으로 조정할 수 있다.

```css
.container {
  display: grid;
  grid-template-columns: 2fr 1fr 3fr 1fr 2fr;
  grid-gap: 16px;
}
```

<img src="https://postfiles.pstatic.net/MjAyMTEwMThfMjc3/MDAxNjM0NTIzNDI1NzM3.WIUT1wGJn7ifiush1LL-UKqfWdOUwVHBueFrepK_scsg.NZhnq75IQPyNTrUq4Q5_7XWudURBg7ko0Gmr2wVTrSog.PNG.mitty0304/image.png?type=w966" alt="grid-template-columns" />


```css
.container {
  display: grid;
  grid-template-rows: 2fr 1fr 3fr 1fr 2fr;
  grid-gap: 16px;
}
```

<img src="https://postfiles.pstatic.net/MjAyMTEwMThfMTUy/MDAxNjM0NTIzNDUxOTkz.J4yW5Aqm2RFYcpvtVNRw5xR6q_mO7f835mVcu8DksDwg.4jXMPxq9vYSloo86xb88CpPzi4FABgGgGLCUzHZM8aMg.PNG.mitty0304/image.png?type=w966" alt="grid-template-rows" />

repeat 속성을 사용하면, 같은 간격을 전체 또는 일부분을 반복해서 나열해줄 수도 있다.

```css
.container{
    grid-template-columns: repeat(3, 1fr);
}
```

위 코드는 grid-template-columns: 1fr 1fr 1fr과 같다.

간격의 최소와 최대치도 지정을 할 수 있는데, grid-auto-rows: minmax(100px, auto); 또는 grid-auto-rows: minmax(100px, auto);repeat 속성을 사용하면, 기본적으로 최소 100px만큼 설정이 되지만, 100px을 넘어설 경우 콘텐츠 크기만큼 자동(auto)으로 늘어난다.


### 2) 그리드 라인을 이용한 아이템 배치 방법

다음은 grid의 큰 특징인 '그리드 라인을 이용한 아이템 배치 방법'이다. 앞서 언급했듯이, flex와는 달리 grid는 2차원적 배치를 지원한다. 동시에 행과 열을 지정하여 콘텐츠 배치를 할 수 있다는 말인데, 이 속성은 '그리드 라인'과 관련이 있다.

<img src="https://mdn.mozillademos.org/files/14761/1_diagram_numbered_grid_lines.png" alt="mdn Basic concepts of grid layout"/>

위 사진처럼 그리드를 3개의 Column과 2개의 row로 이루어져있다고 생각했을 때, 총 4개의 grid line이 column으로 설정된다.

콘텐츠는 라인과 라인 사이의 트랙에 위치가 되며, gird-column-start / grid-column-end / grid-row-start / grid-row-end 속성을 사용해서 배치한다.

우선 다음과 같은 html, css가 있다고 하자.

```html
<div class="grid-layout">
  <div class="box1">1</div>
  <div class="box2">2</div>
  <div class="box3">3</div>
  <div class="box4">4</div>
  <div class="box5">5</div>
</div>
```

```css
.grid-layout{
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-auto-rows: 100px;
  gap: 20px;
}

.grid-layout div{
  background-color: #f5c4c4;
}
```

그리드를 적용한 콘텐츠를 3개씩 1fr의 비율로  column을 설정하며, row는 기본적으로 100px이지만 auto로 설정한다.

그럼 다음과 같은 그리드 배치가 나타난다.

<img src="https://postfiles.pstatic.net/MjAyMTEwMThfOTYg/MDAxNjM0NTMyMTQ4MTky.pBSNcUMWpKKO4mQ8XsqR6N6YrbjEfT3HIQ3eqhpOWZMg.KUpVW8GXnIutHHfgG3zf7KiwWPBrttchbc1t-El7CvIg.PNG.mitty0304/image.png?type=w966" alt="grid-template" />

이때 box1과 box2는 다른 box들과는 다른 배치를 보이게 하고 싶다면, gird-column-start / grid-column-end / grid-row-start / grid-row-end  속성을 활용해볼 수 있다.

```css
.box1 {
  grid-column-start: 1;
  grid-column-end: 4;
  grid-row-start: 1;
  grid-row-end: 3;
}

.box2 {
  grid-column-start: 1;
  grid-row-start: 3;
  grid-row-end: 5;
}
```

box1은 column이 1라인부터 시작해서 4라인에서 끝이 나며,  row는 1라인부터 시작해서 3라인에서 끝이 난다.

box2는 column이 1라인부터 시작하고, row는 3라인부터 시작해서 5라인에 끝이난다.

그럼 다음과 같은 box들의 배치가 보여진다.

<img src="https://postfiles.pstatic.net/MjAyMTEwMThfMjQ1/MDAxNjM0NTMyMzUwMDU2.GJFBfbXOEc1t5SgEG3ylS1VV3xWQGN6V5DgJSjqDSP0g.-XMxknXyIhIEP6UJazi59GJuXlrlzmWbAJQlZr7Ivlsg.PNG.mitty0304/image.png?type=w966" alt="grid-template" />

크롬 브라우저 기준 F12를 눌러서 grid 속성을 활성화시킨 후, 그리드라인을 확인해볼 수도 있다.

<img src="https://postfiles.pstatic.net/MjAyMTEwMThfNDQg/MDAxNjM0NTMyNDgzNzE5.KGDclzvaZGuowD5VxCQysa5eSwmLMVr0piSeCdJEELkg.S7zLr3mhtCYMTsFOXz8YuCuctOH2pENAbmZQ3XyYE-0g.PNG.mitty0304/image.png?type=w966" alt="grid-template" />


---

## 4. 그럼 언제 Flex와 Grid를 사용할까?

### Flex는 콘텐츠 위주의 정렬, Grid는 레이아웃 위주의 정렬

​앞서 말했듯이, 1차원 정렬이면 Flex, 2차원 정렬이면 Grid를 사용하는 것이 좋다. 하지만 그 밖에도 콘텐츠 위주의 정렬이냐, 레이아웃 위주의 정렬이냐에 따라 달라질 수도 있다. MDN에서 설명하는 바는 다음과 같다.

​

​

"플랙스박스는 좀 더 콘텐츠에 초점이 맞춰져 있습니다. 플랙스박스의 이상적인 사용 사례는 여러 아이템을 컨테이너에 고르게 배치하려는 경우입니다. 여기서 콘텐츠의 크기가 각 아이템이 차지하는 공간을 결정합니다. 아이템이 새로운 줄로 줄 바꿈 되면, 아이템의 크기와 해당 줄의 사용 가능한 공간에 따라 간격이 조정됩니다.

그리드는 레이아웃을 먼저 고려하게 됩니다. CSS 그리드 레이아웃을 사용할 때는 우선 레이아웃을 작성한 다음 그 위에 아이템을 배치하거나, 자동 배치 규칙을 통해 견고하게 짜인 그리드 위에 놓인 그리드 셀에 아이템을 배치하게 됩니다."

​

​

MDN의 설명에서 콘텐츠, 아이템이라는 말이 나오는데, 둘의 차이가 모호하기 때문에 다른 영어 표현을 찾아보면 다음과 같다.

(이 부분은 정확한 피드백이 필요하기 때문에 이해하신 분은 댓글로 달아주세요!)

​

"In flexbox layout, the size of a cell (flex-item) is defined inside the flex-item itself, and in the grid layout, the size of a cell (grid-item) is defined inside the grid-container."

​

이해한바로는 아이템은 size of cell (= flex-item, grid-item)이라고  볼 수 있고, 콘텐츠는 flex-item itself, item보다 좀 더 내부에 위치한 것을 의미하는 것 같다. 그래서 직역하자면, flex는 flex-item 그자체의 내부에서 flex-item의 사이즈가 정해지며, grid에서는 grid-container 내부에서 grid-item의 사이즈가 정해진다고 볼 수 있다.

​
예제를 통해 확인해보자.


```html
<div class="flex-wrapper">
  <div class="box1">One</div>
  <div class="box2">Two</div>
  <div class="box3">Three</div>
</div>
```

```css
.flex-wrapper {
  display: flex;
  align-items: flex-end;
  min-height: 200px;
}

.flex-wrapper .box1 {
  align-self: stretch;
}

.flex-wrapper .box2 {
  align-self: flex-start;
}
```

flex-wrapper에는 flex-end를 통해 아이템의 정렬을 밑에서 부터 시작하도록 한다. 그리고 내부의 box1은 stretch 속성을, box2는 flex-start 속성을 각각 부여한다.  그럼 다음과 같은 결과가 나온다.

<img src="https://postfiles.pstatic.net/MjAyMTEwMThfMjg3/MDAxNjM0NTYwNzcxMzMy.ytBYZQpNFjJBErtW_sDLdJ3CwivpVkjK-3WQbVFE9ZUg.h20K62JdHzP1LUH_57_-Orj9ZFZ2ri9Vvf3AgQgkxMsg.PNG.mitty0304/image.png?type=w966" alt="flex"/>

box1, box2, box3 모두 flex-item-itself로 사이즈가 결정이 되는 모습을 볼 수 있다.

반면, grid를 확인해보자.

```html
<div class="grid-wrapper">
  <div class="box1">One</div>
  <div class="box2">Two</div>
  <div class="box3">Three</div>
</div>
```

```css
.grid-wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  align-items: end;
  grid-auto-rows: 200px;
}
.grid-wrapper .box1 {
  align-self: stretch;
}
.grid-wrapper .box2 {
  align-self: start;
}
```

gird도 flex 예제와 마찬가지로, align-items를 통해 아이템의 정렬이 밑에서 시작하도록 한다. 그리고 grid-template-columns: repeat(3, 1fr)로 3개의 아이템을 각각 1fr의 비율을 가질 수 있도록 설정한다. 마찬가지로 box1은 stretch, box2는 start의 속성을 갖도록 한다.

<img src="https://postfiles.pstatic.net/MjAyMTEwMTlfMTU0/MDAxNjM0NTc1MzI0MzI1.LrZvhsS8zc7M3lGMYQJ62L1uZyDwQ3GCDN6z9iXvy6gg.ct-yrwh_YbUQDRQNC_ZN5GNUJaD61HkR9lt6RgDcZxIg.PNG.mitty0304/image.png?type=w966" alt="grid" />

flex와는 다른 결과물이 나온다는 것을 확인할 수 있다. flex의 사이즈는 flex-item-itself(flex-item의 내부 콘텐츠)로 결정이 되지만, grid에서는 grid-container 내부에서 사이즈가 결정이 되기 때문에 내부 콘텐츠 사이즈와는 상관없이 비율 1fr만큼을 사이즈로 갖게 된다.

​
이제 MDN에서 말한, flex는 콘텐츠 위주의 정렬에 적합하고, grid는 레이아웃 위주의 정렬에 적합하다는 것의 의미를 알 수 있을 것이다.

---

## Wrap up.

지금까지 프론트엔드의 대표적인 레이아웃 구성 방법인 flex와 grid에 대해 알아보았다.


flex의 기본적인 속성인 flex-direciton, flex-wrap, flex-align을 알아보았으며,

grid의 기본적인 속성인 grid-template-columns, grid-template-rows, repeat과 fr 단위 그리고 그리드 라인에 대해 정리해보았다.

​

둘의 속성들을 통해서 flex는 1차원적인 레이아웃, 즉 행 또는 열 중 하나의 방법으로 나열하고자 할 때 적절한 방법이며

grid는 행과 열을 동시에 설정할 수 있는 2차원적인 레이아웃에 적절하다는 것을 알게 되었다.

​

flex와 grid는 브라우저 지원 범위에 차이가 있기 때문에 크로스브라우징 이슈를 염두하여 사용하는 것이 좋고, 콘텐츠 위주의 정렬일 경우에는 flex, 레이아웃 위주의 정렬일 경우에는 grid가 적합하다는 것을 예제를 통해 알아보았다.

​

flex와 grid는 아이템의 정렬을 돕는 유용한 방식이지만, 둘의 차이점은 명확하기 때문에 제대로 파악하고 사용하면 더 견고한 프론트 레이아웃을 만들어볼 수 있을 것이다.

​





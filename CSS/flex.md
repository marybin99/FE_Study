# flex

레이아웃 배치 전용 기능  

### 기본 HTML 구조
```html
<div class="container">
    <div class="item">helloflex</div>
    <div class="item">abc</div>
    <div class="item">helloflex</div>
</div>
```

부모 요소인 `div.container`를 Flex Container라 하고,
자식 요소인 `div.item`들을 Flex Item이라 함.  

> "컨테이너가 Flex의 영향을 받는 전체 공간이고, 설정된 속성에 따라 각각의 아이템들이 어떤 형태로 배치되는 것"

Flex 속성은 크게,  
- 컨테이너에 적용하는 속성
- 아이템에 적용하는 속성

으로 나뉨

## 컨테이너에 적용하는 속성

### ➿ display: flex;
`Flex` 아이템들은 가로 방향으로 배치되고, 자신이 가진 내용물의 `width`만큼만 차지하게 됨


### ➿ 배치 방향 설정 flex-direction
아이템들이 배치되는 축의 방향을 결정하는 속성

#### row (기본값)
가로 방향 배치

#### row-reverse
역순 가로 배치

#### column
세로 방향 배치

#### column-reverse
역순 세로 배치


### ➿ 줄넘김 처리 설정 flex-wrap
컨테이너가 더 이상 아이템들을 한 줄에 담을 여유 공간이 없을 때 줄바꿈을 어떻게 할지 결정하는 속성

#### nowrap (기본값)
줄바꿈 x. 넘치는대로 둠

#### wrap
줄바꿈 o.

#### wrap-reverse
줄바꿈 o. 대신 역순으로


### ➿ flex-flow
flex-direction과 flex-wrap을 한꺼번에 지정할 수 있는 단축 속성

### ➿ 메인축 방향 정렬 justify-content

#### flex-start (기본값), flex-end, center, space-between, space-aroud, space-evenly

### ➿ 수직축 방향 정렬 align-items

#### stretch (기본값), flex-start, flex-end, center, baseline

### ➿ 여러 행 정렬 align-content

#### stretch (기본값), flex-start, flex-end, center, space-between, space-aroud, space-evenly

## 아이템에 적용하는 속성

### ➿ 유연한 박스의 기본 영역 flex-basis
`Flex` 아이템의 기본 크기를 설정

### ➿ 유연하게 늘리기 flex-grow
아이템이 `flex-basis`의 값보다 커질 수 있는지를 결정하는 속성  
0보다 큰 값이 세팅되면 해당 아이템이 유연한 박스로 변하고, 원래의 크기보다 커지며 빈 공간 메우게 됨

### ➿ 유연하게 줄이기 flex-shrink
`flex-grow`와 쌍을 이루는 속성  
아이템이 `flex-basis`의 값보다 작아질 수 있는지를 결정하는 속성  
0보다 큰 값이 세팅되면 해당 아이템이 유연한 박스로 변하고, `flex-basis`보다 작아짐

### ➿ flex
`flex-grow`, `flex-shrink`, `flex-basis`를 한 번에 쓸 수 있는 축약형 속성

### ➿ 수직축으로 아이템 정렬 align-self

#### auto (기본값), stretch, flex-start, flex-end, center, baseline

### ➿ 배치 순서 order
각 아이템들의 시각적 나열 순서를 결정하는 속성  
숫자값이 들어가고, 작은 수부터 먼저 배치

### ➿ z-index
Z축 정렬. 숫자가 클 수록 위로 올라옴

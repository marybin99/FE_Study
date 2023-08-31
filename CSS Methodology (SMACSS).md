# CSS Methodology

> CSS의 활용도가 높아지고 대규모 프로젝트가 늘어나면서 CSS에도 다양한 방법론 등장   
  → SMACSS, BEM, OOCSS

- 코드의 재사용성 증가
- 쉬운 유지보수
- 확장 가능성
- 클래스명만으로도 의미 예측 가능

## SMACSS (Scalable and Modular Architecture for CSS)
> CSS를 <b>범주화(Categorization)</b>로 패턴화하고자 하는 방법론   

작성할 css를 비슷한 종류끼리 모아 5가지 스타일로 나누고 각 유형에 맞는 선택자와 작명법, 코딩 기법을 제공

### Base
- 기본 스타일 (Reset, Default, Variables, Mixins)
- !import 사용하지 않음
- element 스타일의 default 값을 지정해주는 것
- 선택자: 요소 선택자 사용

```css
body, form, p, table, button, fieldset, input ... {
  margin: 0;
  padding: 0;
}
```

### Layout
- 레이아웃과 관련된 스타일 정의
- 구성하고자하는 페이지를 컴포넌트로 나누고 어떻게 위치해야하는지 결정
- 주요 요소(id)와 하위 요소(class)로 구분
- 주로 클래스 사용
  > css에서 id를 사용하면 재사용성이 떨어지기 때문
- 접두사 사용

```css
// layout => l-

// 주요 요소 작성
#content {
  width: 400px;
}
#aside {
  width: 40px;
}

// 하위 요소 작성
.l-width #content {
  width: 650px;
  padding: 10px;
}
.l-width #aside {
  width: 100px
}
```

### Module
- 재사용성이 높은 구성 요소를 정의
- Block, Element, Module
- 재사용을 위해 id 셀렉터와 element 사용 x

```css
.stick { ... }
.stick-name { ... }
.stick-number { ... }
```

- element 셀렉터 사용해야 한다면, .folder >span처럼 child 셀렉터 사용

```html
<div class="folder">
  <span>Folder Name</span>
</div>
<div class="box">
  ...
</div>
<div class="basket">
  ...
</div>
```
```css
.folder >span {
  padding-left: 20px;
  background: url(icon.png);
}
```
### States
- 요소의 상태 변화를 
- 다른 스타일에 덧붙이거나 덮어씌워서 상태 나타냄
  > 자바스크립트에 의존하는 스타일이 됨
- 접두사 `is-`나 `s-`사용
  > 상태를 제어하는 스타일임을 나타냄

```html
<div class="btn_area">
  <a href="#" class="btn btn_good is-active"> 좋아요 버튼 </a>
  <a href="#" class="btn btn_bad"> 나빠요 버튼 </a>
</div>
```
```css
.btn {
  display: inlin-block;
  background: #ddd;
  border-radius: 4px;
}
.btn.is-active {
  background: #43f837;
}
.btn.is-hidden {
  display: none;
}
```

### Theme
-  사용자가 선택가능하도록 스타일을 재선언하여 사용
- 접두어 `theme-` 붙여 표시
```css
.mod {
  border: 1px solid;
}
```
```css
.mod {
  border-color: blue;
}
```

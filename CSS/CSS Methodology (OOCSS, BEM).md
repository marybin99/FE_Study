# CSS Methodology

> CSS의 활용도가 높아지고 대규모 프로젝트가 늘어나면서 CSS에도 다양한 방법론 등장   
  → SMACSS, BEM, OOCSS

- 코드의 재사용성 증가
- 쉬운 유지보수
- 확장 가능성
- 클래스명만으로도 의미 예측 가능

## OOCSS (Object Oriented CSS)
> CSS를 모듈 방식으로 작성하여 중복을 줄이는 방식의 방법론

가장 많이 사용되는 방법론으로 구조와 스타일을 분리해서 작성

### 구조와 모양을 분리하거나 결합
반복적인 시각적 기능을 별도의 모양으로 정의하여 다양한 객체와 혼합해 중복코드 삭제

- 기존 방식
  ```html
  <a class="instagram_btn instagram_skin">Instagram</a>
  <a class="facebook_btn facebook_skin">Facebook</a>
  ```
- OOCSS 적용
  ```html
  <a class="btn skin instagram">Instagram</a>
  <a class="btn skin facebook">Facebook</a>
  ```
  ```css
  .btn -> 공통 버튼 스타일 정의
  .skin -> 공통 스킨 스타일 정의
  ```

### 컨테이너와 컨텐츠 분리
스타일 정의 시, 위치에 의존적인 스타일 사용하지 않기. 사물의 모양은 어디에 위치하든지 동일하게 보여야 함

```html
<div class="header common-width">Header</div>
<div class="footer common-width">Footer</div>
```
```css
.header {
  position: fixed;
  top: 0;
}
.footer {
  position: relative;
  bottom: 0;
}
.common-width {
  width: 800px;
  margin: 0;
}
```

### 장점
- 코드의 재사용성
- 코드 재사용으로 스타일시트의 용량 축소
- 용량 축소로 인한 속도 향상

### 단점
- 다중 클래스 사용으로 HTML 복잡해짐 (가독성 떨어짐)
- non-semantic한 클래스 사용
- sass와 같이 사용하면 단점 보완 가능

## BEM (Block Element Modifier)
> 블록, 요소, 상태로 구분하여 클래스 이름을 작성하는 방식의 방법론   
  웹 페이지를 각각의 컴포넌트 조합으로 바라보고 접근한 방법론

- 어떠한 목적인가에 초점
- id를 사용할 수 없음
- 엄격한 네이밍 규칙을 따름

### Naming Convention
소문자와 숫자만을 이용해 작명   
여러 단어의 조합은 하이픈 `-` 과 언더바 `_` 사용해 연결

### Block
재사용할 수 있는 기능적으로 독립적인 페이지 구성 요소   
주변 환경에 영향을 받지 않아야 하며, 여백이나 위치를 설정하면 안됨
> 일반적으로 하나의 단어 사용, 길어질 경우 `-` 사용
```css
.header {..}
.block {..}
```

### Element
블록을 구성하는 단위   
블록 안에서 특정 기능을 수행하는 컴포넌트
> `__`를 사용
```css
.header {..}
.header__tap {..}
.header__content {..} 
.header__logo__button {..}
```

### Modifier
블록이나 요소의 속성   
블록이나 요소의 외관이나 상태를 변화시킴   
수식어로 불리언 타입과 키-값 타입이 있음
> `-` 사용
```css
.header--hide {..}
.header__tap--big {..}
.header__content--disabled {..}
```

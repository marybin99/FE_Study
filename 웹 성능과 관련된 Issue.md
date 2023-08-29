# 웹 성능과 관련된 Issue 

## 네트워크 요청에 빠르게 응답하기
- `3.xx` 리다이렉트 피하기
- `meta-refresh` 사용금지
- `CDN(content delivery network)` 사용
- 동시 커넥션 수를 최소화
- 커넥션 재활용

## 최소한의 크기로 자원을 내려받기
- 777K   
  <img src="https://github.com/marybin99/CS/assets/110241993/fa26c467-13f7-4517-b1bf-5c31cabab951" width="300">

- `gzip` 압축 사용
- `HTML5 App cache` 활용
- 자원을 캐시 가능하게 하기
- 조건 요청 보내기

## 효율적인 마크업 구조를 가지기
- 레거시 IE 모드는 `http 헤더`를 사용   
  <img src="https://github.com/marybin99/CS/assets/110241993/f33090d7-3a78-459a-8321-386400798d90" width="500">

- 페이지 위에 CSS 넣기
  ```html
  <html>
    <head>
      <title>Test</title>
      <link rel="stylesheet" type="text/css" href="class.css" />
    </head>
    <body>
      ...
    </body>
  </html>
  ```
- `@import` 사용 피하기
- inline 스타일과 embedded 스타일 피하기
- 사용하는 스타일만 CSS에 포함
- 스크립트는 아래에
  ```html
  <html>
    <head>
      <title>Test</title>
    </head>
    <body>
      ...
      <script src="myscript.js" ..></script>
    </body>
  </html>
  ```
- 중복되는 코드 최소화
- 단일 프레임워크 사용
- Third Party 스크립트 삽입 금지

## 미디어 사용을 개선하기
- 이미지 스프라이트 사용   
  (* 이미지 스프라이트: 여러 개의 이미지를 하나의 이미지로 합쳐서 관리하는 이미지)

- 실제 이미지 해상도 사용
- `CSS3` 활용   
  (gradient, -webkit-border-radius..)
- 하나의 작은 크기의 이미지는 `DataURL` 사용
- 비디오 미리보기 이미지 만들기

## 빠른 자바스크립트를 작성하기
- 코드를 최소화
- 필요할 때만 스크립트 가져오기
  - flag 사용
- DOM에 대한 접근 최소화
  - Dom manipulate는 느림
- 다수의 엘리먼트를 찾을 때는 `selector api` 사용
  
  ```javascript
  document.querySelectorAll("");
  ```
- 마크업 변경은 한번에
  - temp 변수 활용
- DOM의 크기 작게 유지
- `내장 JSON 메서드` 사용
  
  ```javascript
  var jsObjStringParsed = JSON.parse(jsObjString);
  var jsObjStringBack = JSON.stringfy(jsObjStringParsed);
  ```

## 어플리케이션이 어떻게 동작되는지 알고 있기
- Timer 사용에 유의
- `requestAnimationFrame` 사용
- 활성화될 때 알고 있기

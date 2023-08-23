# Document Object Model
> 웹 페이지의 콘텐츠 및 구조, 그리고 스타일 요소를 구조화시켜 표현하여 프로그래밍 언어가 해당 문서에 접근하여 읽고 조작할 수 있도록 API를 제공하는 일종의 인터페이스   
» 스크립팅 언어(자바스크립트)가 쉽게 웹 페이지에 접근하여 조작할 수 있도록 연결시켜주는 역할

## 어떻게 생성되고 어떻게 보여질까?
- 트리 자료로 구축이 되기 때문에, HTML 문서는 최종적으로 하나의 최상위 노드에서 시작해 자식 노드들을 가지며, 아래로만 뻗어나가는 구조로 만들어지게 됨

![DOM 코드](https://github.com/marybin99/CSstudy/assets/110241993/6e96e1c8-779c-46cf-a32c-ad8fa99fe608)
위 코드를 트리 구조로 표현하면
![DOM 트리 구조](https://github.com/marybin99/CSstudy/assets/110241993/18324f2f-de07-42fe-a964-e126bd902077)
위와 같음   

> <b>"document 노드"</b>가 최상위 노드, <b>"element 노드"</b>가 밑으로 오며,   
<b>"text 노드"</b>와 <b>"attribute 노드"</b>가 오는 계층적인 구조   

이러한 노드 타입에는 총 12가지가 있음 가장 중요한 것은 위의 4가지 노드

###  document node (문서 노드)
- DOM Tree에서 최상위 루트 노드, document 객체를 가리킴
- HTML 문서 전체를 나타내는 노드이기도 함
- window 객체의 document 속성으로 바인딩되어 있어 `window.document`, `document`로 참조해 사용 가능
- HTML 문서에 이 노드는 오직 1개 존재

### element node (요소 노드)
- 모든 HTML 요소는 이 노드
- 속성 노드를 가질 수 있는 유일한 노드
- 부모-자식 관계를 가져 계층적 구조를 이룰 수 있음

### attribute node (속성 노드)
- 모든 HTML 요소의 속성은 이 노드
- 요소 노드에 대한 정보를 가짐
- 부모 노드가 아닌 해당 노드와 바인딩되어 있음

### text node (텍스트 노드)
- HTML 문서의 모든 텍스트는 이 노드
- 정보를 표현
- 가장 마지막 자식 노드라 잎사귀를 닮았다 하여 "리프 노드"라고 불리기도 함
   
## 자바스크립트 DOM
> 자바스크립트는 DOM을 조작할 수 있는 프로그래밍 언어 중 가장 유명한 언어

### 다른 개념인 이유는?
DOM은 자바스크립트없이 DOM 인터페이스 구현만으로도 조작할 수 있기 때문   
DOM은 어떤 프로그래밍 언어에 의존하지 않는 독립적인 인터페이스

### DOM이 자바스크립트라는 오해의 원인
현재 웹 브라우저에서 DOM을 조작하는 언어는 자바스크립트 뿐이기 때문   
브라우저에서 점점 동적인 기능을 요구하기 시작하면서, 내부에 동적인 기능을 지원해줄 프로그래밍 언어를 넣기로 했는데, 이 때 이후 내장 프로그래밍 언어로 쭉 자바스크립트를 고수하고 있음

### DOM의 데이터타입
- 프로퍼티 (property)   
DOM 객체의 멤버 변수, HTML 태그의 속성을 반영
- 메소드 (method)   
DOM 객체의 멤버 함수, HTML 태그를 제어
- 컬렉션 (collection)   
정보를 집합적으로 표현하는 일종의 배열
- 이벤트 리스너 (event listener)   
HTML 태그에 작성된 이벤트 리스너들을 그대로 가짐
- 스타일 (style)   
이 속성을 통해 HTML 태그에 적용된 CSS 스타일 시트에 접근 가능
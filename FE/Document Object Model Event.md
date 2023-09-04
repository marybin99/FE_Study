# Document Object Model Event

## EVENT
웹에서는
- 브라우저 (user agent) 로부터 발생하는 이벤트
- 사용자의 행동 (interaction) 에 의해 발생하는 이벤트
- DOM의 '변화'로 인해 발생하는 이벤트   

등 수많은 이벤트가 발생하고 흐름   
발생하는 이벤트는 그저 자바스크립트 객체일 뿐

## EVENT Flow
이벤트는 이벤트 각각이 갖게 되는 전파 경로를 따라 전파됨   
이 전파 경로(propagation path)는 DOM Tree구조에서 Element의 위상에 의해 결정됨

### Propagation path
- 자기 자신을 포함하여 그 부모 엘리먼트에 의존   
- 경로는 리스트 형식으로 구성, 마지막 값은 Event Phase에 따라 달라짐
- 실제로 event가 targeting된 DOM엘리먼트에 의해 결정됨

### Event Phase
<img src="https://github.com/marybin99/CSstudy/assets/110241993/2a08bfb9-b447-4fdc-8272-fe8d72febe36" width="300" height="300"/><br>
전파 경로가 결정되고 나면 이벤트 객체는 Event Phase 따라 전달   
총 세 단계의 Event Phase를 지원
1. Capture Phase   
이벤트 객체가 window부터 이벤트가 등록된 요소까지 전달되는 단계   
전파 경로의 시작은 window가 되며 마지막은 이벤트가 등록된 요소
2. Target Phase   
이벤트 객체가 event target에 도달한 단계   
만약, Event type이 다음 phase 지원하지 않는 경우, 다음 단계는 넘어가고 전파종료
3. Bubble Phase   
이벤트 객체가 capture 단계에서 전달되었던 반대 순서로 진행   
부모 엘리먼트로 전달이 진행되면서 최종적으로 window까지 이벤트가 전달됨   
이 단계에선 window가 전파 경로의 마지막이 됨

## Bubbling Event
> 이벤트 핸들러는 기본적으로 Bubbling phase 때 발생하도록 등록되며 버블링으로 이벤트를 등록한다는 것은 이벤트가 전파되는 단계 중 Bubbling Phase에서 핸들러를 실행하는 방식으로 등록하는 것

## Cancelable Event
`a` 태그를 사용했지만 기본적인 동작을 막고 싶은 경우, `preventDefault`를 호출   
```javascript
aTag.addEventListener('click', e => {
    e.preventDefault()
})
```
이렇게 하면 `a`태그를 클릭할 경우, `href`에 정의된 URL로 이동하지 않음.   
<br>
이벤트가 dispatch될 때 기본 동작을 할 것인지에 대한 기준을 `defaultPrevented` 값으로 판단하는데, 이 값이 true일 때 기본 동작을 발생시키지 않음   
`preventDefault`메서드는 Event 객체의 `cancelable` 속성 값이 true일 때만 호출가능하며 내부적으로 Event 객체의 defaultPrevented 값을 true로 변경함

### passive
addEventListener의 `option.passive`는 default값으로 false를 가짐   
이 값을 true로 지정할 경우, 이벤트가 발생되는 시점에서 `defaultPrevented` 값을 무시하게 됨   
=> 이벤트가 발생할 때마다 매번 확인했던 `defaultPrevented`를 더이상 확인하지 않아도 됨을 의미   
=> 비용을 줄여 이벤트의 성능 향상

## Trusted Event
> 사용자에 의해 발생한 이벤트인지 브라우저에 의해 발생한 이벤트인지 객체의 속성을 통해 판단 가능

## Stop propagation
> 이벤트의 전파를 멈출 수도 있음
```javascript
element.addEventListener('click', e => {
    e.stopPropagation()
})
```
이벤트 핸들러 내부에서 이벤트 객체의 `stopPropagation` 메소드를 호출하게 되면 다음으로 진행 예정인 Event Phase가 진행되지 않고 전파가 멈추게 됨

# reset vs normalize
> 브라우저마다 기본적으로 제공하는 element의 style을 통일시키기 위해 사용하는 css

## reset.css
극단적으로 브라우저의 내장 스타일을 초기화시켜줌 (날 것 그대로의 모습)   
`<H1>~<H6>`,`<p>`,`<strong>`,`<em>` 등과 같은 표준 요소는 완전히 똑같이 보임   
HTML 코드를 직접 보지 않는 이상, 어디가 무슨 코드인지 구분할 수 없음
> 최근 브라우저 간 호환성이 좋아지면서 불필요한 부분을 줄이고 최신 웹 트렌드를 반영한 <b>Elad Shechter's CSS Reset</b>이 인기

## normalize.css
가능한한 브라우저의 내장 스타일을 최대한 건들이지 않는 상태에서 브라우저 간 다른 부분만 통일시켜줌  
어느 브라우저에서든 일관적인 모습으로 나타나게 됨   
`reset.css가 모든 것을 해제한다면 `normalize.css`는 유용한 기본값을 보존   
→ <b>보다 넓은 범위를 가지며</b> HTML5 요소의 표시 설정, 양식 요소의 글꼴 상속 부족, pre-font 크기 렌더링 수정, IE9의 SVG 오버플로 및 iOS의 버튼 스타일링 버그 등에 대한 이슈 해결해줌
> `React`에서 `Styled Components`로 스타일할 경우 `styled-normalize` 많이 사용
# CORS (Cross-Origin Resource Sharing)

## 무슨 뜻일까?
HTTP 요청은 기본적으로 Cross-site HTTP Requests가 가능   
`<img>`태그로 다른 도메인의 이미지 파일을 가져오거나, `<link>`태그로 다른 도메인의 CSS를 가져오거나, `<script>`태그로 다른 도메인의 Javascript 라이브러리를 가져오는 것이 모두 가능   
BUT   
`<script></script>`로 둘러싸여 있는 스크립트에서 생성된 Cross-Site HTTP Requests는 <span style="color:#71a3c7">Same Origin Policy</span>를 적용받기 때문에 불가능   
AJAX가 널리 사용되면서 `<script></script>`로 둘러싸여 있는 스크립트에서 생성되는 `XCMLHttpRequest`에 대해서도 Cross-Site HTTP Requests가 가능해야 한다는 요구가 늘어나가 W3C에서 CORS라는 이름의 권고안이 나오게 됨

## 요청 종류
> Simple/Preflight, Credentail/Non-Credential 4가지 존재

### Simple Request
- GET, HEAD, POST 중의 한 가지 방식 사용
- POST 방식일 경우 Content-type이 아래 셋 중 하나여야 함
    - application/x-www-form-urlencoded
    - multipart/form-data
    - text/plain
- 커스텀 헤더를 전송하지 않아야 함
- <b>서버에 1번 요청하고, 서버도 1번 회신하는 것으로 처리가 종료</b>

### Prefilght Request
- GET, HEAD, POST 외의 다른 방식으로도 요청을 보낼 수 있음
- application/xml처럼 다른 Content-type으로 요청을 보낼 수 있음
- 커스텀 헤더도 사용 가능
- 예비 요청과 본 요청으로 나뉘어 전송
- <b>서버에 예비 요청을 보내고 서버는 예비 요청에 대해 응답 후, 그 다음에 본 요청을 서버에 보내고 서버도 본 요청에 응답</b>
    - 프로그래머가 Access-Control-계열의 Response Header만 적절히 정해주면, OPTIONS 요청으로 오는 예비 요청과 GET, POST, HEAD, PUT, DELETE 등으로 오는 본 요청의 처리는 서버가 알아서 처리

### Request with Credential
- HTTP Cookie와 HTTP Authentication 정보를 인식할 수 있게 해주는 요청
- 요청 시 `xhr.withCredentails = true`를 지정해서 Credential 요청을 보낼 수 있음
- 서버는 Response Header에 반드시 `Access-Control-Allow-Credentails: true`를 포함해야 하고, `Access-Control-Allow-Origin`헤더의 값에는 `*`가 오면 안되고 `http://foo.origin`과 같은 구체적인 도메인이 와야 함

### Request without Credential
- 기본적으로 Non-Credential 요청

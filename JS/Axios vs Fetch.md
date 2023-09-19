# Axios vs Fetch

## Axios
브라우저, Node.js를 위한 Promise API를 활용하는 HTTP 비동기 통신 라이브러리   
주로, API 연동과 Backend-Frontend 사이의 통신에서 사용   
설치 후 사용

### 특징
- 운영 환경에 따라 브라우저의 `XMLHttpRequest` 객체 또는 `Node.js`의 `http api` 사용
- `Promise(ES6) API` 사용
- 요청과 응답 데이터의 변형
- HTTP 요청 취소
- HTTP 요청과 응답을 JSON 형태로 자동 변경

### 사용법

```powershell
$ npm install axios
```

```js
// App.js

import axios from 'axios';

// API 연동하기
const getData = async(e) => {
    const {data} = await axios({
        method: "get",
        url: "url.com",
    }).then((res) => console.log(res));
};
```

- **method**: 데이터를 가져올 때 "get", 보낼 때 "post" 등으로 쓰임
- **URL**: 통신할 웹 서버
- **headers**: 헤더
- **responseType**: 응답데이터의 타입

#### 단축 메소드
> **GET** : axios.get(url[, config])  
> **POST** : axios.post(url, data[, config])  
> **PUT** : axios.put(url, data[, config])  
> **DELETE** : axios.delete(url[, config])

#### 전역 설정 (Config Defaults)
모든 요청에 적용되는 설정의 default 값 전역 명시 가능   
주로 서버에서 서버로 axios 사용 시, 요청 헤더를 명시하는데 자주 쓰임   

```js
axios.defaults.baseURL = 'https://api.example.com';
axios.defaults.headers.common['Authorization'] = AUTH_TOKEN;
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';
```

```js
// Instance를 만들 때 설정의 default 값 설정 가능
const instance = axios.create({
    baseURL: 'https://api.example.com'
});

// Instance 만든 후 default 값 수정 가능
instance.defaults.headers.common['Authorization'] = AUTH_TOKEN;
instance.defaults.timeout = 2500;
```

## Fetch
ES6부터 생긴 JS 내장 라이브러리로 별도의 설치가 필요없음   
Axios와 마찬가지로 Promise 기반   

### 사용법
첫번째 인자로 URL, 두번째 인자로 옵션 객체를 받고, Promise 타입의 객체 반환   
반환된 객체는 API 호출이 성공했을 경우, `.then`으로 `resolve 객체` 리턴  
　　　　　　　　　　　&nbsp;&nbsp;실패했을 경우, `.catch`로 `reject 객체` 리턴받음

```js
const getData = async(e) => {
    const { data } = await fetch("url.com", {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
        },
        body: JSON.stringfy({  // JSON으로 변환해주는 과정 필요
            id: "1234",
            description: "안녕",
        }),
    })
    .then((res) => console.log(res))
    .catch((error) => console.log(error));
};
```
Axios와 거의 비슷하지만 string으로 변환하는 과정 필요

## Axios vs Fetch
|Axios|Fetch|
|---|---|
|써드파티 라이브러리로 설치 필요 O|현대 브라우저에 빌트인이라 설치 필요 X|
|**data 속성** 사용|**body 속성** 사용|
|data는 object 포함|body는 문자열화 되어있음|
|status가 200이고 statusText가 'OK'면 성공|응답객체가 ok 속성을 포함하면 성공|
|자동으로 JSON 변환|.json()메서드 사용해야함|
|자동 문자열 변환|POST요청 시 `JSON.stringfy()`사용해 문자열 변환|
|속도가 상대적으로 느림|속도가 상대적으로 빠름|
|다양한 브라우저에서 사용 가능|지원안되는 브라우저 존재|
|CSRF 보호 기능으로 상대적으로 보안에 유리|X|
|요청 취소 가능, 타임아웃 걸 수 있음|X|
|HTTP 요청 가로채기 가능|X|
|download 진행에 대해 기본적인 지원|X|

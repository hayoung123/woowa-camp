# OAuth

> 본 글은 [생활코딩 OAuth](https://www.youtube.com/watch?v=hm2r6LtUbk8&list=PLuHgQVnccGMA4guyznDlykFJh28_R08Q-&index=1)를 보고 학습을 위해서 정리한 글입니다.

## 전체적 흐름의 표

지금은 봐도 모르겠다. 하지만 이 글을 다 읽으면 이해가 되겠찌???

![](https://images.velog.io/images/proshy/post/f911db5e-27f5-464f-aff7-a7319da932dd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-31%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.55.41.png)

## 3개의 주체

- Resource Server  (github)

- Client (내 페이지)

- Resource Owner (사용자)

- Resource Server

  데이터를 갖고 있는 서버

- Authorization Server

  인증과 관련된 처리를 전담하는 서버

합쳐서 Resource Server라고 생각해도 괜찮다.

## client에서 Resource Server에 등록하는 방법

각 Resource Server 마다 다르다. 아래의 3가지를 대표적으로 생각하면 될듯

아래의 3가지 핵심정보를 client, resource server이 알게 하는 것

- Client ID

- Client Secret (누출 x)

- Authorized redirect URIs ex)http://client/callback

  authorized code를 전달해 줄 때 위 주소로 알려주세요 라고 말하는 것

## Resource Owner의 승인

Resource Owner는 client에 접근해서 Resource Server를 사

1. Resource Owner가 소셜 로그인을 이용하고 싶다.!
2. 클라이언트에서 아래와 같은 버튼을 만들어 놓고 Resource Owner는 클릭을 한다.

### 깃허브 로그인 버튼 a href는 어떤 url을 써야될까

```jsx
<https://resource.server/>
?client_id=1
&scope=B,C
&redirect_uri=https://client/callback

//예시
<https://github.com/login/oauth/authorize>
?client_id=619d8e37e985e7ab3be6
&scope=user //(각 resource server가 정해놨다.)
&redirect_uri=http://3.37.76.224 || <http://localhost:3000>
```

1. Resource Owner 가 위의 URI로 접근을 한다.

2. Resource Server는 Resource Owner의 로그인 여부를 판단한다.

   1. 로그인 X : 로그인 화면
   2. 로그인 O : 넘어간다.

   ⇒ 로그인 성공

3. Resource Server는 2가지를 체크한다.

   - URI의 ClientID ===  자신이 갖고 있는 ClientID
   - URI의 redirect_uri === 위에 일치한 ClientID의 redirect_uri

4. Resource Server는 ~~한 권한이 맞는지 물어본다.

5. Resource Owner는 권한을 허용한다.

6. Resource Server는 아래와 같은 정보를 ClientID 아래 저장한다.

   - userId:1 scope:user

## Resource Server의 승인

위의 과정에서 Resource Owner가 승인을 해줬으면 바로 Client에게 권한을 주지는 않는다.  why? 위험하다.

1. 

> Resource Server는 Resource Owner에게  **authorization code**를 아래와 같이 넘겨준다. ex) Location : https://client/callback?code=3123123

- 여기서 Location : 이 무엇일까?

1. 응답에 Header에 Location에 위의 URI를 넣어주는 것인데 그렇게 되면 response를 받는 Resource Owner의 브라우저는 위의 URI로 자연스럽게 이동한다.

> 여기서 프론트엔드 개발자는 redirect된 저 URI에서 **authorization code**를 얻어서 서버에 다시 보내주는 역할을 하는 것

1. Client는 **authorization code를 얻게 된다.**
2. Client는 Resource Server에 아래의 주소로 직접 접속한다.

```jsx
<https://resource.server/token>
?grant_type=authorization_code
&code=3123123
&redirect_uri=https://client/callback
&client_id=1
&client_secret=2
```

1. Resource 아래의 4가지 항목을 모두 비교한다.
   - client_id
   - client_secret
   - redirect_uri
   - authorization_code

드디어 **Access Token**을 받을 수 있게 됐다.

## 엑세스 토큰 발급

드디어 OAuth의 목적인 액세스 토큰을 받을 수 있게 됐다.

1. authorization_code를 이용해서 이미 검증을 했기 때문에 **삭제한다**.
2. Resource Server는 authorization_code대신 Access Token을 발급, 저장
3. Access Token Client에게 전달한다. 전달 받은 Client는 Access Token을 저장한다.

## Access Token을 활용해서 Resource Server API 활용하기

각 Resource Server에서 주어진 API를 활용할 수 있게 됐는데 이때 갖고 있는 Access Token을 같이 보내줘야된다.

Client가 Resource Server에 Access Token을 전달하는 것에는 2가지 방법이 있다.

- 쿼리로 받기 https://...?**access_token="token"** (일단 구글은 되는데 다른데는 모름)

- 헤더의  **Authorization: Bearer** **"token"** (표준 방법)

  여기서 Bearer는??

  OAuth를 위해 나온 인증방법이라고 한다.  (token_type 이라고 생각하면 될 듯)

> 헤더로 주고 받는 방식이 안전하다고 한다. 그래서 우리는 헤더로 보냈었나보다.

## Refresh Token

각각의 Access Token은 수명이 있다.

이때 보관하고 있었던 Refresh Token을 이용해서 Access Token을 다시 받는다.

Resource Server마다 Refresh Token을 주는게 다르고 매번 갱신되거나 이런 것도 다 다르다고 한다. 각 사이트에서 확인해봐야될듯!

> 핵심은 Access Token의 유효기간이 끝나서 API 사용시 에러가 발생되면 다시 Refresh Token을 이용해서 Access Token을 받아와야 한다.

## 전체적 흐름의 표

다시 보면 조금은 이해가 된다.

![](https://images.velog.io/images/proshy/post/f911db5e-27f5-464f-aff7-a7319da932dd/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-07-31%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.55.41.png)



## 출처

[생활코딩 OAuth](https://www.youtube.com/watch?v=hm2r6LtUbk8&list=PLuHgQVnccGMA4guyznDlykFJh28_R08Q-&index=1)
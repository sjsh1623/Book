## Google Oauth

아래는 구글에서 제공하는 토큰 동작 흐름 방식이다.

<img src="https://developers.google.com/identity/protocols/oauth2/images/flows/authorization-code.png" title="Google Token flow"/>

1. Request token
   > 구글에 토큰 발급을 요청한다.
2. Authorization code
    > 구글에 Authorization code를 발급해준다.
3. Exchange code for token
   > Authroization code를 사용하여 Token 정보를 요청한다.
4. Token response
   > Token 정보를 발급해준다. (어떻게 요청하냐에 따라 JWT, Refresh token 발급 여부를 요청할수 있다.)
5. User token to call Google API
    > 토큰을 사용하여 API를 호출

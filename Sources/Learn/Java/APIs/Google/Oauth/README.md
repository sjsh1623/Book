## Google Oauth

아래는 구글에서 제공하는 토큰 동작 흐름 방식이다.
> 공식 문서 : https://developers.google.com/identity/protocols/oauth2

<img src="https://developers.google.com/identity/protocols/oauth2/images/flows/authorization-code.png" title="Google Token flow"/>

> Google OAUTH2 Flow 요약 \
> 여기서 JWT Google Authroization의 하위 개념이며 Google Authorization code (token) 발급, 해당 토큰일 사용하여 Token을 요청한다. (한마디로 카카오 API라던지 ZOOM API라던지 동작방식은 비슷하다)
1. Request Authroization token
   > 구글에 토큰 발급을 요청한다.
2. Authorization token
    > 구글에 Authorization code를 발급해준다.
3. Exchange code for token
   > Authroization token를 사용하여 Token 정보를 요청한다.
4. Token response
   > Token 정보를 발급해준다. (어떻게 요청하냐에 따라 JWT, Refresh token 발급 여부를 요청할수 있다.)
5. User token to call Google API
    > 토큰을 사용하여 API를 호출

> Tip. 한가지 주의해야할 점은 Google OAuth2 Quick Start 
> (https://developers.google.com/api-client-library/java) 은 Android 또는 Install App에서 동작한다.
> 정확히 말하면 동작을 안하는건 아니지만 "new LocalServerReceiver()"는 class 파일을 들어가면 localhost로 동작되게만 되어있다. 참고하면 좋을것 같다.
> 추가적으로 "new LocalServerReceiver()"는 Jetty jar 사용하고 있어 있다.

```java
 /** Authorizes the installed application to access user's protected data. */
 private static Credential authorize() throws Exception {
   // load client secrets
   GoogleClientSecrets clientSecrets = GoogleClientSecrets.load(JSON_FACTORY,
       new InputStreamReader(CalendarSample.class.getResourceAsStream("/client_secrets.json")));
   // set up authorization code flow
   GoogleAuthorizationCodeFlow flow = new GoogleAuthorizationCodeFlow.Builder(
       httpTransport, JSON_FACTORY, clientSecrets,
       Collections.singleton(CalendarScopes.CALENDAR)).setDataStoreFactory(dataStoreFactory)
      .build();
   // authorize
   return new AuthorizationCodeInstalledApp(flow, new LocalServerReceiver()).authorize("user");
}
```

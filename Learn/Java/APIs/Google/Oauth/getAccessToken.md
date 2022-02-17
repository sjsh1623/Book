# Access Token 얻기 / 만들기

### What is Access Token
- Access Token은 로그인 세션을 위한 보안자격 (Security Credentials)을 가진다
- 간단하게 생각하면 HTTP Request의 Authorization Header에 넣은 Token 정보이며 (보통 JWT 형식으로 사용)
- 보통 분실 위험이 있어 30분 또는 1시간의 유효기간을 가지며 Google Token 같은 경우에는 1시간으로 되어있다. (1시간 이후는 Refresh Token을 사용하여 다시 Token을 재발급을 받는다)
> Access Token에 대해서 더 자세히 알고 싶다면 아래의 문서에서 확인하면 좋을것 같다. \
> https://www.oauth.com/oauth2-servers/access-tokens/#:~:text=Access%20tokens%20are%20the%20thing,parts%20of%20a%20user's%20data.&text=Access%20tokens%20must%20be%20kept%20confidential%20in%20transit%20and%20in%20storage.

### How to get Google Access Token
아래는 Google Authentication Token (JWT X)발급을 위한 URL을 생성하는 전체 과정이다 Java로 작성되었다. (Google에서 제공하는 SDK를 사용하였다)
```java
private static final String APPLICATION_NAME = "yourApplicationName";
private static final JsonFactory JSON_FACTORY = GsonFactory.getDefaultInstance();
private static final List<String> SCOPES = Collections.singletonList(CalendarScopes.CALENDAR);
private static final String CREDENTIALS_FILE_PATH = "./credentials.json";

 --------------------------------------------------
        
InputStream in = GoogleCalendar.class.getResourceAsStream("./credentials.json");
        final NetHttpTransport HTTP_TRANSPORT = GoogleNetHttpTransport.newTrustedTransport();
        if (in == null) {
            throw new FileNotFoundException("Resource not found: ./credentials.json");
        } else {
            GoogleClientSecrets clientSecrets = GoogleClientSecrets.load(JSON_FACTORY, new InputStreamReader(in));
            String redirectURL =  new GoogleAuthorizationCodeRequestUrl(clientSecrets, "http://localhost:8080/google_calendar_callback.act", SCOPES)
                    .setAccessType("offline")
                    .setApprovalPrompt("force")
                    .setState(userId)
                    .build();
            return redirectURL;
        }
```
```java
// 아래의 코드는 모든 Credential 정보를 가지고 있는 Json 파일이다. 해당 파일은 Google API Console에서 제공하고 있다.
private static final String CREDENTIALS_FILE_PATH = "./credentials.json";
```
``` java
// Credential.json을 기반으로 GoogleClientSecrete을 생성
GoogleClientSecrets clientSecrets = GoogleClientSecrets.load(JSON_FACTORY, new InputStreamReader(in));
```

``` java
 String redirectURL =  new GoogleAuthorizationCodeRequestUrl(clientSecrets, "http://localhost:8080/google_calendar_callback.act", SCOPES) // 콜백 페이지를 선언한다. 당연히 Google API Console에서 해당 callback address를 추가시켜주어야 한다
                    .setAccessType("offline") // JWT Token 발급
                    .setApprovalPrompt("force") // Refresh Token -> 강제 발급 (한번 발급하면 그 다음부터는 Refresh Token을 주지 않는다... 하여 추가 하였다 좋은 방법이 아닐수 있지만 추가하였다. 더 좋은 방법이 있으면 알려주세요! )
                    .setState(userId) // Parameter을 추가한다. 나는 여기에 사용자 아이디를 Set 해두었다
                    .build(); // 빌드를 해야 String 타입의 URL을 return한다.
```
> 해당 공식문서를 확인하여 사용방법을 참고하면 좋을것 같다 \
> https://googleapis.dev/java/google-api-client/latest/com/google/api/client/googleapis/auth/oauth2/GoogleAuthorizationCodeRequestUrl.html

## How to get JWT Token from Google
- Google 에서 Access Token을 발급 받았다면 이제 JWT Token을 발급 받을 차례이다. 
- 아래의 코드는 Google Access Token을 사용하여 JWT Token을 발급 받는 Java 코드이다. (위와 동일하게 Google에서 제공하는 SDK를 사용하여 발급 받았다)
```java
        final NetHttpTransport HTTP_TRANSPORT = GoogleNetHttpTransport.newTrustedTransport();
        InputStream in = GoogleCalendar.class.getResourceAsStream("./credentials.json");
        if (in == null) {
            throw new FileNotFoundException("Resource not found: ./credentials.json");
        } else {
            GoogleClientSecrets clientSecrets = GoogleClientSecrets.load(JSON_FACTORY, new InputStreamReader(in));
            GoogleTokenResponse response = new GoogleAuthorizationCodeTokenRequest(
                    HTTP_TRANSPORT,
                    JSON_FACTORY,
                    clientSecrets.getDetails().getClientId(),
                    clientSecrets.getDetails().getClientSecret(),
                    code, // Google Access Token은 여기에 위치한다
                    "Your callback URL").execute();
        }
    }
```
- Google의 SDK에서 제공하는 GoogleTokenResponse 인스턴스를 활용하여 토큰을 발급받는다.
- GoogleTokenResponse는 Access Token , Refresh Token을 return하며 이외 다른 여러가지 정보를 return 한다. 지금 가장 필요한건 Access Token과 Refresh Token이기 때문에 다른 설명은 생략하겠다.
- 나는 모든 토큰 정보를 Dataase에 저장하여 관리하였다.


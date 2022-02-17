# Refresh Token을 사용하여 Access / Refresh Token 발급

### What is refresh token
- Refresh Token은, JWT로그인 방식에 사용되는 보안상의 단점을 보완하기 위해 존재한다
- Refresh Token은 Access Token에 유효기간을 짧게하여 공격받을 면적을 줄이는것으로 보안성을 높이는 또 다른 토큰이다.
- Access Token이 만료가 되면 처음에 발급은 Refresh Token을 사용해서 Access Token을 재 발급을 받는다. 
- Access Token의 생명 주기는 보통 30분 ~ 1시간으로 만료가 될때 마다 새로운 Access Token을 발급 받는다.

### How to get Google Refresh Token
- Refresh Token은 사용자가 처음 인증을 받거나 getAccessToken할때 " **.setApprovalPrompt("force")** "을 한다면 Access Token과 함께 Refresh Token도 함께 발급 받을 수 있다.
- 초기에 Token을 발급 받을때는 Google Authentication token (Also known as Google Access Token)을 가지고 JWT의 Access Token과 Refresh Token을 두개를 발급 받을 수 있다.
- 이후에 Refresh Token이 분실되거나 했을 경우 재 발급을 받아야 할 상황이 있을 수 있다. 그럴 경우 재 발급을 하면 된다. 해당 방법은 아래의 Official Document를 읽어보는걸 권장한다.
> https://developers.google.com/api-client-library/java/google-api-java-client/oauth2
- 두개의 토큰을 발급 받는 방법은 getAccessToken 하는 방법에 설명해두었으니 참고하면 좋을것 같다.

### How to get new Google JWT Token with Refresh Token
- 아래는 Refresh Token을 사용하여 새로운 토큰을 발급 받는 Java 코드이다.
```java
    private static Credential refreshCredentials(String refreshToken) throws IOException, GeneralSecurityException {
        InputStream in = GoogleCalendar.class.getResourceAsStream("./credentials.json");
        final NetHttpTransport HTTP_TRANSPORT = GoogleNetHttpTransport.newTrustedTransport();
        if (in == null) {
            throw new FileNotFoundException("Resource not found: ./credentials.json");
        } else {
            GoogleClientSecrets clientSecrets = GoogleClientSecrets.load(JSON_FACTORY, new InputStreamReader(in));
            GoogleCredential refreshTokenCredential = new GoogleCredential.Builder().setJsonFactory(JSON_FACTORY).setTransport(HTTP_TRANSPORT).setClientSecrets(clientSecrets).build().setRefreshToken(refreshToken);
            refreshTokenCredential.refreshToken();
            return refreshTokenCredential;
        }
    }
```
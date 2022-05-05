## Google 연동

## APIs
> 작성한 모든 Document들은 **Java**로 설명한다. \
> 기록을 위함이며 완벽한 가이드는 아닙니다. 참고만 부탁드립니다. 부족한 부분이 있다면 Comment를 달아주세요.
1. Google Oauth2 - 구글 로그인 인증
   1. Authorization Code 발급
   2. JSON Web Token (JWT) 발급받기
2. Google Calendar - 구글 캘린더 API
   1. Calendar 생성하기
   2. Calendar 지우기
   3. Calendar 존재 여부 확인하기
   4. Calendar Event 생성하기
   5. Calendar Event 업데이트하기
   6. Calendar Event 삭제하기

> 먼저 Google 연동하기 전에 Javascript를 사용할 수 있는지 먼저 확인해보자. Java로 작성하기에 꽤나 복잡하고 알아야하는것이 많다.
> 나의 경우 Javascript로 작성하여 연동 테스트까지 완료하였지만 서버 작업으로 하는것이 불가피 하여 Java로 작성하였다. 다행이다 Java는 1.8 이상이다.
> (내 개인적인 생각이다. Java보다는 Javascript가 연동하기에는 참고할 수 있는 자료가 많았으며 조금 더 코드가 깔끔해보였다. 특히 Oauth 할때...)

## Google Documentation
> Google documentation은 아주 정리가 잘 되어있다. 아래의 링크를 통해 확인해보자. (영문)
> https://developers.google.com/workspace 

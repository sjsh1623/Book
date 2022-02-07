## Google Calendar

> Language : Java \
> Target : Google OAuth2를 사용하여 캘린더를 생성하여 해당 캘린더에 이벤트 생성 \
> Official Document : https://developers.google.com/calendar/api

### Basic knowledge for Google Calendar?
- Google Calendar은 크게 Calendar 와 Event로 나뉜다. Calendar는 Event의 상위 개념으로 많은 Event를 묶어두는 개념이다.
조금 더 빠른 이해를 위해 구글 캘린더 웹페이로 예를 들면 아래의 이미지와 같이지 편에 있는 Calendar이고 Event는 오른편에 있는 이벤트들이다.

### Google Calendar Api Helper
- Google Calendar 연동을 더욱 쉽게 하기위해 JAR(쉽게말해 Java Class File의 Zip)을 제공하며 Google이 제공하는 
  Api Helper을 사용하면 조금 더 가독성이 좋은 코드를 작성할 수 있으며 조금 더 쉽게 연동이 가능하다.
  
- 말 그대로 Helper이다. 결국은 HTTP 통신이며 방법론의 차이이다. 
  굳이 Helper을 사용하지 않아도 POST로 Google Calendar 공식 문서를 참고하여 형식에 맞게 요청을 보내면 똑같은 응답값이 내려온다.
  하지만, 나는 조금 더 빠른 작업 속도와 조금 더 나은 가독성을 위해 Google에서 제공하는 Helper을 사용하기로 선택했다.
  
``` java
import com.google.api.client.auth.oauth2.Credential;
import com.google.api.client.extensions.java6.auth.oauth2.AuthorizationCodeInstalledApp;
import com.google.api.client.extensions.jetty.auth.oauth2.LocalServerReceiver;
import com.google.api.client.googleapis.auth.oauth2.GoogleAuthorizationCodeFlow;
import com.google.api.client.googleapis.auth.oauth2.GoogleClientSecrets;
import com.google.api.client.googleapis.javanet.GoogleNetHttpTransport;
import com.google.api.client.http.javanet.NetHttpTransport;
import com.google.api.client.json.JsonFactory;
import com.google.api.client.json.gson.GsonFactory;
import com.google.api.client.util.DateTime;
import com.google.api.client.util.store.FileDataStoreFactory;
import com.google.api.services.calendar.Calendar;
import com.google.api.services.calendar.CalendarScopes;
import com.google.api.services.calendar.model.Event;
import com.google.api.services.calendar.model.Events;
```
  
Google Calendar 연동은 생각보다 어렵지 않다고 생각이 든다. Google 공식 문서를 제대로 읽고 작성한다면 전혀 어려울 것이 없다고 생각된다. 
거기에 Helper을 사용하여 API 통신을 한다면 OAuth는 물론 Google Calendar도 어렵지 않게 연동 할 수 있다고 생각든다. 
중간 중간 어려움이 있었던 부분 좀 자세히 찾아봐야 알 수 있는 부분을 잘 정리 하고 어떤 방법을 사용하여 작성하였는지 기록하기 위한 목적으로 Book을 작성한다.

아래의 공식문서를 시작으로 하나하나 다 읽어보면 좋을것 같다.
> https://developers.google.com/calendar/api/quickstart/java


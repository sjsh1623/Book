## 크롬 브라우저에 특정 Timezone 적용

## 계기
회사 (Madrascheck-Flow)에서 해외 접속 환경을 테스트 하기 위함.
Timezone 테스트 및 개발을 하기 위함

## 명령어 (Mac)
```
TZ='US/Pacific' open -na "Google Chrome" --args "--user-data-dir=$HOME/chrome-profile"
```

- 위와 같은 방법으로 크롬을 열면 해당 브라우저에만 Los Angeles 시간이 적용된 것을 확인 할 수 있다.
- 확인은 new Date() 또는 moment()를 사용하여 확인한다.

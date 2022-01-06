# Day.js
> 회사에서 Internationalization(국제화)를 맡아 개발을 하였고 각 국가에서 현지 시간 서비스를 제공하기 위해 TimeZone localize 작업을 하였다.

> 처음에는 moment를 사용하였다. moment는 327KB 이상의 용량이며 moment-timezone 185KB 까지 합해진다면 거의 400kb 정도로 무거운 library이다.
추가 적으로는 moment.js는 2021 하반기에 Deprecated 되어 더이상 업데이트를 지원하지 않았다. (https://terodox.tech/migrating-away-from-momentjs-part1/)
moment.js가 무겁고 지원을 더 이상 지원을 하지 않아 다른 library를 찾던 중 dayjs를 찾게되었고 적용하게 되었다.
그리고 아래에 왜 day.js를 선택하였는지 조금 설명해보고 어떻게 사용했는지 기록해보려고 한다.

## Why Day.js?

### 1. 용량이 적다.
> 2KB. moment.js 327KB에 비하면 너무나도 가벼운 library이다. 
> TimeZone도 2kb 미만으로 정말로 가벼운 library이다.

### 2. moment.js와 사용방법이 거의 동일하다.
> Day.js 공식 문서(https://day.js.org/en/)에 대문짝만하게 적혀있다.
> "fast 2kB alternative to Moment.js with the same modern API"
> "Moment.js와 동일한 API를 가지고 있으며 2KB의 빠른 Moment.js의 대안이다"  
> 실제로 대부분 moment.js와 동일한 구조를 가지고 있었으며 moment()를 dayjs()로만 변경했을때 오류가 거의 없었다.
    
### 3. i18n 사용
> i18n을 사용하여 국제화 지원을 하였다.

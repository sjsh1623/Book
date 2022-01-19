# async / await

> 구글 캘린더를 연동하는데 많은 API를 호출하여 처리하여야 했다. 
> 처음에 구글 캘린더 리스트를 받고 캘린더 리스트에 아이디를 추출하여 해달 캘린더에 이벤트를 추가한다. 
> 에러코드 409 (Duplicated Id 중복 아이디) 오류가 발생했을때는 해당 이벤트를 업데이트한다.
> 
> 위 상황과 같이 API Respond값으로 처리 해야하는게 많았고 해당 조건을 Callback , Promise로만 해결을 할 경우 
> Callback hell이 되거나 가독성이 매우 떨어질 것 같다는 생각이 들어 async / await을 사용하자라고 마음 먹었다.

## 비동기 처리
> 특정 로직 실행이 끝날 때까지 기다려주지 않고 나머지 코드를 먼저 실행

## 잘못된 공부... 
> 당연하겠지만 나는 처음에 Promise 없이 await async만 사용하여 원하는 부분을 동기 처리 할 수 있다고 생각했다.
> 혹시나 나 처럼 생각하는 사람은 없기를 바라면서... 조금 쉽게 말하면 
> Promise의 then().then().then()을 깔끔하게 정리해주는 친구가 async / await이라고 생각한다


## How to use
먼저 Google Calendar 연동을 예를 들어보면 구글 캘린더 연동시 크게 캘린더 이벤트로 나뉜다.  캘린더는 구글 캘린더 들어가면 왼쪽에 나열되어 있는 그룹(?)들을 캘린더라고 한다 이벤트는 캘린더의 하위 개념으로 
캘린더 안의 상세 정보라고 보면 쉽다 몇일 또는 몇시에 시작해서 끝나는지 어떠한 이벤트 인지등을 기록하는 개념이다. (다
Google Calendar Api Integration에 대해서는 다시 다른 곳에 정리할 예정이다)
 
아래의 코드는 시용자의 캘린더 리스트를 가져와 summary(캘린더 제목)이 존재하는지의 여부를 체크하고 없다면 새로 생성해주는 로직이

```
  const calendarList = await getCalendarList();
  const calendarId = getCalendarId(calendarList, 'Personal Schedule');
  
  function getCalendarList() {
        return new Promise((resolve) => {
            gapi.client.calendar.calendarList.list().execute((listData) => {
                resolve(listData);
            })
        })
    }
    
     async function getCalendarId(list, target) {
        let result;
        const items = list.items;
        items.forEach(item => {
            if (item.summary === target) result = item.id;
        })
        if (!result) result = await executeCreateCalendar(target);
        return result;
    }
    
    function executeCreateCalendar(summary) {
        return new Promise((resolve => {
            gapi.client.calendar.calendars.insert({
                'summary': summary
            }).execute((response) => {
                resolve(response.id)
            });
        }))
    }
```
먼저 함수 앞에 async라는 예약어를 붙혀 캘린더 정보를 가져오는 비동기 처리가 필요한 코드 앞에 await을 붙혀 처리한다.
여기서 중요한점은 await 붙어있는 비동기 처리 함수는 Promise를 return 해야한다.
위 코드를 돌리면 차례대로 calendarList를 가져오고 summary를 비교하고 리스트에 존재하지 않다면 새로 insert를 해준다. 

생각보다 여럽지 않고 가독성이 좋고 깔끔한 코드를 작성할 수 있는게 아주 큰 장점이다!

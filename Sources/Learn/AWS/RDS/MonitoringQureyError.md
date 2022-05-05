## RDS 특정쿼리 무한호출

<img src="https://raw.githubusercontent.com/sjsh1623/Book/master/Image/RDSError.png" title="RDS Error"/>

> 업무를 보는중 팀장님으로 부터 해당 쿼리 확인을 해달라는 요청을 받았다. 처음에는 아무생각 없이 서버에서 무한루프가 도는것 같아 재채 확인을 하였지만... 
> 오류가 없었다 심지어 개발 서버를 1분동안 꺼서 확인을 해보기도 했다. 하지만 쿼리는 계속해서 호출이 되고 있었으며 DB Connection에 이상이 있는걸로 생각하고 접근하였다.\
> \
> 어디서 호출하는지 확인을 하기위해 AWS Application 확인하는 탭으로 확인을 해보았다. 아니나 다를까... 
> <img src="https://raw.githubusercontent.com/sjsh1623/Book/master/Image/Application.png" title="RDS Application"/>\
> 위와 같이 SQL Tool에서 무한으로 호출하고 있었다.. \
> 
### 오늘의 배움
> 꼭! 꼭! 꼭! 당연히 서버 문제겠지 생각하지말고 정확히 하기위해 Application을 들어가 호출하는 Application을 확인한 후에 장애를 대처하자!!

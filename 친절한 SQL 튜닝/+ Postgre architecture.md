# + Postgre architecture

발표자: Yes
범위: 복습
생성일시: June 8, 2022 8:33 AM
작성자: 석현임
최종편집: June 8, 2022 1:49 PM

# Postgre architecture

### **MVCC (Multi Version Concurrency Control)**

- DBMS에서 Lock을 사용하지 않고, 데이터의 일기 일관성을 보장해 주는 내부 기법. (Lock은 서버의 동시성을 크게 떨어트린다. )객체의 변경사항을 모두 버전을 나누어 저장하고, 데이터 객체는 이 버전을 모두 연속체로서 정의, 관리하게 하여 락을 사용하지 않고 일관된 읽기를 보장.

### MGA -  Multi Generation Architecture (PostgreSQL)

- 데이터를 변경할때 해당 데이터를 변경하는것이 아닌, 새로운 데이터를 추가 후 새로운 데이터를 링크를 연결하는 방식
- 데이터 저장공간인 Page 에는 페이지 헤더 / 튜플 / 튜플의 헤더 / 튜플의 위치를 가리키는 포인터들로 저장
- 각각의 튜플에는 데이터들이 저장되어 있다.
- 이 튜플의 헤더 (동그라미 점선)는 튜플의 속성값들을 저장합니다.

![img.png](+%20Postgre%20architecture%2081342e48db51485e864b8e6b0ef37d50/img.png)

### MVCC 저장 방법

- 상황 1
    
    세션0에서 Tuple1 에 A 라고 하는 데이터를 저장 후 Commit 했습니다.
    
    t_xmin 의 값은 2a9 이고, 아직 갱신이 되지 않은 값이기 때문에 t_xmax 는 0 입니다.
    
    이 상태에서 세션1에서 조회를 하면 t_xmin 2a9 에 있는 값을 조회합니다.
    
    ![download (1).png](+%20Postgre%20architecture%2081342e48db51485e864b8e6b0ef37d50/download_(1).png)
    
- 상황 2
    
    위 상태에서 세션0에서 Tuple1의 A 값을 B 로 Update 하겠습니다.
    
    Tuple1, B 에 새로운 값이 저장이 되었고 t_xmin 은 2aa 값이 되었습니다.
    
    Tuple1, A 에는 t_xmax 에서 갱신되었다는 의미로 다음값인 2aa 를 저장합니다.
    
    이제 새로 조회하는 세션들은 갱신된 2aa 의 값을 조회하게 됩니다.
    
    ![download (2).png](+%20Postgre%20architecture%2081342e48db51485e864b8e6b0ef37d50/download_(2).png)
    
- 상황 3
    
    위 상태에서 세션0에서 Tuple1의 B 값을 C 로 Update 하겠습니다.
    
    - 세션 1은 read-only 잡은 시점이므로 (tuple1, A)를 읽고, 세션 2는 (tuple1, B)를 읽습니다
    - 이때 업데이트가 되었기 때문에 세션 2에서 t_xmax가 변경됩니다
    - 세션 3의 시점은 (tuple1, C)를 읽습니다
    - 세션 1, 2, 3은 같은 레코드를 읽지만, 다른 세션이기 때문에 여러 개의 버저닝을 합니다.
        
        ![download (2).jpeg](+%20Postgre%20architecture%2081342e48db51485e864b8e6b0ef37d50/download_(2).jpeg)
        
- 상황 4
    
    Update 후 커밋을 하지 않을 경우
    
    - 위의 그림은 MVCC로 insert/update가 충돌하지 않는 것을 보여줍니다.
    - 세션 1은 (tuple1, A)를 읽는다.
    - 세션 2는 (tuple1, B) -> (tuple1, C)로 업데이트하려 했으나, 아직 커밋이 되지 않아 락 상태로 걸려있다.
    - postgreSQL과 Mysql은 스냅숏과 리드 뷰를 가지고 있는데, 이는 select를 할 때 commit하지 않은 트랜잭션 리스트를 가지고 있어야만 정확하게 데이터를 읽을 수 있는 구조이다.
        
        ![download (3).jpeg](+%20Postgre%20architecture%2081342e48db51485e864b8e6b0ef37d50/download_(3).jpeg)
        
    

### 트랜잭션 처리를 위한 ID 발급

- 트랜잭션이 실행시 따라 증가하는 시간정보인 XID값을 insert시에 헤더의 t_xmin에 저장, delete시에는 t_xmax에 저장2. SCN(System Commit Number)는 따로 할당 받지 않고, t_ctid를 통해 MVCC를 구현
- SCN는 따로 할당 받지 않고, t_ctid를 통해 MVCC를 구현
    
    ![download.png](+%20Postgre%20architecture%2081342e48db51485e864b8e6b0ef37d50/download.png)
    

### MVCC 저장 방법

### 데이터 블록 내의 레코드 관리

기본 단위를 블록 Or 페이지라 하며, 스토리지와 메모리 버퍼간의 select / write 시 블록 단위로 I/O하게 된다.

- Oracle/postgreSQL은 새로운 레코드를 insert하면 데이터 페이지 내 아래쪽에서 부터 차례로 저장하고, 헤더는 이 레코드를 가리키는 포인터가 차례로 생긴다.
- mysql은 새로운 레코드가 Insert되면 페이지의 위쪽부터 데이터 레코드가 저장되고, 아래쪽부터 레코드의 슬롯정보가 생기는데, 레코드의 정보가 아닌 탐색을 위한 장치이다.
    
    ![download.jpeg](+%20Postgre%20architecture%2081342e48db51485e864b8e6b0ef37d50/download.jpeg)
    

### Old version 관리

Postgresql는 Old version의 별도 공간이 없다. 동일한 페이지내에 old/new가 같이 존재하므로 vacuum작업을 통해 페이지를 새롭게 재구성하면서 오래된 공간을 회수한다.

![download (1).jpeg](+%20Postgre%20architecture%2081342e48db51485e864b8e6b0ef37d50/download_(1).jpeg)
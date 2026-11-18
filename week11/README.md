## 쿠버네티스 설정과 상태 점검

### 1. Secret을 base64 -d로 읽었을 때
-> 쿠버네티스 Secret는 데이터는 base64로 인코딩해서 저장하지만, base64는 암호화가 아니다.<br>
   따라서 누구나 쉽게 원래 값으로 디코딩 할 수 있기 때문에 안전하지 않다.<br>

#### 1) REDIS_PASSWORD 확인
![secret_confirm](images/secret_confirm.png) 
<br>

#### 2) REDIS_PASSWORD 디코딩
![secret_decoding](images/secret_decoding.png)
<br>
-------

### 2. ConfigMap 변경

#### 1) 기존 화면
![before_guestbook](images/before_guestbook.png)
<br>

#### 2) 제목과 색 변경
![edit_guestbook](images/edit_guestbook.png)
<br>

#### 3) 변경된 화면
![after_guestbook](images/after_guestbook.png)
------

### 3. readiness 실패(READY 0/1)

#### 1) guestbook Pod의 READY 상태 확인
![kill_redis_check_pod_ready](images/kill_redis_check_pod_ready.png)
<br>

#### 2) /readyz 확인
-> 503에러: Redis가 중단되어 guestbook 애플리케이션이 요청을 정상적으로 처리 X
![readiness_503_error](images/readiness_503_error.png)
<br>

#### 3) endpoints 항목 conditions.ready가 false인 것을 확인
![check_ready_false](images/check_ready_false.png)
<br>

#### 4) Pods 되살리기
![recover_redis_pods](images/recover_redis_pods.png)
<br>
-------

### 4. liveness 실패(RESTARTS 증가)
![liveness_restarts](images/liveness_restarts.png)
<br>

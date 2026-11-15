## 쿠버네티스 배포와 서비스

### 1. replicas를 5로 늘리기
![kubectl_get_pods_1](images/kubectl_get_pods_1.png)
![kubectl_get_replicaset](images/kubectl_get_replicaset.png)
![kubectl_get_deployments](images/kubectl_get_deployments.png)
<br>

### 2. 서로 다른 Pod hostname
![hostname1](images/hostname1.png)
![hostname2](images/hostname2.png)
![hostname3](images/hostname3.png)
![hostname4](images/hostname4.png)
![hostname5](images/hostname5.png)
<br>

### 3. Pod 하나 강제 삭제
![kubectl_delete_pod](images/kubectl_delete_pod.png)
![kubectl_get_pods_2](images/kubectl_get_pods_2.png)
<br>

### 4. v1 -> v2 롤링 업데이트
#### 1) guestbook:v1
![guestbook_v1](images/guestbook_v1.png)
<br>

#### 2) index.html 소스 수정
#### 3) docker 이미지 빌드
![fix_index_image_build](images/fix_index_image_build.png)
<br>

#### 4) ghcr에 이미지 업로드
![image_upload](images/image_upload.png)
<br>

#### 5) deployment 이미지 변경
#### 6) 롤링 업데이트 확인 및 롤백
![change_image](images/change_image.png)
<br>

#### 7) kubectl rollout history 결과 확인
![kubectl_rollout_history](images/kubectl_rollout_history.png)
<br>

#### 8) guestbook:v2
![guestbook_v2](images/guestbook_v2.png)

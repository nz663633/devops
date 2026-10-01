## Docker
- Docker: 애플리케이션을 실행하는 환경을 컨테이너로 만들어서 관리할 수 있게 해주는 플랫폼
  - 개발자의 노트북, 서버, 클라우드 등 서로 다른 환경에서도 동일한 컨테이너를 실행 가능
  - 컨테이너는 하나의 OS 커널을 공유하며 실행 환경을 서로 격리
  - "개발 -> 테스트 -> 배포" 환경을 일관되게 만들 수 있음
  - CI/CD에 적합

### Docker 구성요소
1. Image
  - 컨테이너를 만들기 위한 실행 환경의 읽기 전용 템플릿, 설계도(붕어빵 틀)
  - 프로그램과 실행 환경을 함께 압축한 파일
  - 이미지 하나로 컨테이너를 몇 개든 만들 수 있음
2. Container
  - 이미지로부터 생성된 인스턴스(붕어빵)
  - 이미지를 기반으로 실제 실행되고 있는 환경
3. Dockerfile
  - 이미지를 어떻게 만들지 작성하는 파일
4. Registry
  - 이미지를 저장 및 공유하는 곳
  - ex. Docker Hub

- Hello from Docker!
- 실행에 필요한 파일과 환경을 담은 이미지를 내려받아 컨테이너로 실행
```bash
docker run hello-world    -> Docker는 먼저 로컬에 hello-world 이미지 있는지 확인

Unable to find image 'hello-world:latest' locally   -> 로컬에 hello-world:latest 이미지 없음
latest: Pulling from library/hello-world   -> Docker Hub 같은 이미지 저장소에서 이미지 가져옴
					   -> hello-world 이미지 다운로드 시작
```

### 컨테이너 격리
1. namespace (환경 격리)
  - 프로세스가 자신만의 독립된 환경에 있는 것처럼 보이게 하는 기능
  - 프로세스, 네트워크, 파일 시스템, hostname 등의 범위를 격리
  -> "무엇을 볼 수 있는가?"
2. cgroup (자원 관리)
  - 프로세스 그룹의 시스템 자원 사용량을 관리하고 제한
  - CPU, 메모리 등의 자원을 제한
  -> "얼마나 사용할 수 있는가?"

### 컨테이너 실행
```bash
docker run -d -p 8080:80 --name web nginx   -> -d: detached (백그라운드에서 실행)
 					    -> -p: publish (외부에서 접근 가능하도록 연결)
					    -> -p 호스트포트:컨테이너포트
					    -> --name web: 컨테이너 이름을 web으로 지정
					    -> nginx: 사용할 Docker 이미지 이름
```
-> 호스트 OS에 nginx 설치 X
-> 서버 띄우기 위해 파이썬 설치 X
-> _nginx_와 _실행에 필요한 파일_을 담은 **이미지**를 받아 컨테이너로 실행

Q. 컨테이너 포트는 동일해도 되는가?
A. 컨테이너 포트는 동일해도 각각 격리되어 있기 때문에 충돌 발생 X
   각 컨테이너는 자기만의 네트워크 공간을 가짐
   단, _호스트 포트_는 서로 달라야 함

### Docker 명령어
```bash
docker ps	-> 실행 중인 컨테이너 출력

docker ps -a	-> 모든 컨테이너 출력(멈춘 컨테이너 포함)

docker images	-> 내려받은 이미지 목록 보기

docker logs <컨테이너 이름>	-> 현재까지의 로그 확인

docker logs -f <컨테이너 이름>	-> 실시간으로 로그 확인 (-f: follow)

docker stop <컨테이너 이름>	-> 컨테이너 정지

docker start <컨테이너 이름>	-> 컨테이너 다시 켜기

docker rm -f <컨테이너 이름>	-> 삭제 (-f: 실행 중이어도 강제 삭제)
```
-> nginx의 컨테이너는 삭제되었지만 nginx 이미지는 로컬에 남아 있음

### 컨테이너 내부
```bash
docker exec -it <컨테이너 이름> bash	-> 컨테이너 안에서 셸 실행
					-> -it: 대화형 터미널 환경으로 컨테이너에 접속
					-> bash: 컨테이너 안에서 bash 셸 실행

root@ed35db34e032:/# exit	-> 컨테이너에서 나가기
```
- 컨테이너를 삭제하면 컨테이너의 쓰기 계층에 저장된 변경사항도 함께 사라짐


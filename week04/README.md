## 자동화와 협업
### 파일 찾기
```bash
find . -type f -name "*.sh"    -> 셸 스크립트 파일 찾기

grep -R -n "exit 1" week03 2>/dev/null	-> exit 1을 포함하는 파일 찾기
					-> -R: 하위 디렉터리까지 모두 검색
					-> -n: 줄 번호 표시
					-> 2>/dev/null: 에러 메시지를 /dev/null에 버리기

grep -R "exit 1" week03 2>/dev/null | wc -l   -> exit 1을 포함하는 파일 개수 보기
					      -> wc -l: (word count, lines) 입력된 텍스트의 줄 수를 계산
```
- | (파이프)
  - 파일을 전달하는 것이 아닌, 앞 명령의 표준 출력을 뒤 명령의 표준 입력으로 연결
- grep (global/regular expression/print)
  - 텍스트에서 원하는 패턴이 포함된 줄을 검색하는 명령
  - -i: 대소문자 무시 / -c: 줄 개수 세기 / -n: 줄번호 표시 / -v: 찾은 것을 제외한 줄 출력 

- echo $?
  - 바로 직전에 실행한 명령어의 종료 상태
```bash
for f in *.sh; do echo "스크립트: $f"; done   -> 현재 디렉터리의 .sh 파일 이름을 출력

var.sh
greet.sh
check.sh
```
### 프로세스와 백업
- 프로그램: 디스크에 저장된 실행코드(실행 파일에 들어 있는 명령어들의 모음)
- 프로세스: 실행 중인 프로그램
```bash
sleep 300 &   -> 300초 동안 아무것도 하지 않고 대기하는 프로그램
	      -> &: 백그라운드에서 실행(화면에 PID 표시, 바로 터미널 다시 사용 가능)

jobs	      -> 현재 셸에서 실행한 백그라운드 작업 상태 표시

ps aux || grep sleep   -> PID 확인

kill `<PID>`   -> 정상 종료 요청(SIGTERM)

ps aux || grep sleep   -> 사라진 것 확인
```
- aux: a(모든 사용자) u(사용자 중심의 자세한 형식) x(터미널에 연결되지 않은 프로세스 표시, 백그라운드 등) 

- tar: 여러 파일을 하나의 아카이브로 묶는 도구
  - tzf: 내용 확인
    - t=list(목록 확인), z=gzip(압축 사용), f=뒤에 오는 파일명 지정
  - xzf: 압축 해제
  - czf: 압축 생성
  - C: Change directory(작업할 디렉터리 지정)

- diff -rq (차이가 있는지만 간단히 표시)
  - ex) diff -ru A B  -> 다른 내용을 -와 +로 보여줌
  - echo $? 시, 1 출력  -> (0: 같음, 1: 다름, 2: 오류)

## Git 협업
1. feature/backup-log 브랜치 아래서 작업 후 -> commit -> push
2. PR 올리기
```bash
gh pr create --title "백업 로그 기록 기능 추가" \    -> PR의 제목을 지정
	--body "백업이 끝나면 backup.log에 백업 일시와 파일명을 기록합니다." \
	--base main    -> PR을 최종적으로 합칠 브랜치
	--head feature/backup-log   -> PR을 보내는 쪽의 브랜치(생략 가능)

gh pr view --web
```
3. 합치기(스쿼시 머지 사용)
```bash
gh pr merge --squash --delete-branch   -> main에 합치면서 branch 삭제

git switch main

git pull --ff-only    -> 최신 main 내용 가져오기

git log --oneline -3   -> 최근 커밋 3개를 한 줄로 보여줌
```
### 충돌과 병합(Conflict와 Merge)
- 충돌 원인
  - 공통 조상 A를 기준으로 B와 C를 비교했을 때, 같은 줄이 서로 다르게 수정되어 Conflict 발생
  - 즉, Git이 B와 C 중 하나를 임의로 선택 X -> 무엇을 남겨야 할지 자동으로 결정 못함

- Github PR Merge 옵션
  - --merge: 기존 커밋 유지 + merge commit 생성
  - --squash: 여러 커밋을 하나로 합쳐서 반영
  - --rebase: feature 브랜치의 각 커밋을 새 커밋으로 만들어 main 뒤에 일렬로 적용

## 네트워크
### IP
- IP 주소: 네트워크에서 장치를 식별하기 위한 주소(건물 주소)
- 공인IP: 인터넷에서 라우팅 가능한 주소(건물의 도로명 주소)
- 사설IP: 해당 사설 네트워크 내부에서 사용하는 주소(건물 내부 층)
- 127.0.0.1: localhost = 나 자신
  - 네트워크를 통해 다른 컴퓨터를 찾아가는 주소가 아닌, '내 컴퓨터 자신'을 가리키는 주소

- 가정이나 학교 내부에서는 사설IP 사용
- 공유기 또는 게이트웨이가 내부의 사설IP를 공인IP와 매핑하여 인터넷 통신
  -> NAT(Network Address Translation) 네트워크 주소 변환

### Port
- 포트: 하나의 컴퓨터에서 실행 중인 여러 프로그램을 구분하기 위한 번호
  - ex. IP만으로는 _건물_까지만 알 수 있음. 포트가 있어야 _몇 호의 어느 프로그램_인지 알 수 있음
- 자주 사용하는 포트
포트|용도|
---|---|
22|SSH(원격 서버 접속 및 암호화된 통신)
80|HTTP(웹)
443|HTTPS(보안 웹)
3306|MySQL
6379|Redis
5000|Flask 개발 서버
5432|PostgreSQL
8080|개발용 웹 서버

### 서버 찾기, 통신 확인
- DNS(Domain Name System)
  - 도메인 이름에 대응하는 IP주소를 찾는 역할
  - 전화번호부 역할

- curl: 서버에 요청 보내고 응답 확인하는 명령어

- 대표적인 응답 코드
코드|의미
---|---
200|OK(성공)
400|Bad Request(잘못된 요청)
404|Not Found(리소스 없음)
500|Internal Server Error(서버 에러)
503|Service Unavailable(서비스 불가)

- 4xx: 클라이언트 요청 관련 오류
- 5xx: 서버 측 오류

### 서버 실행 및 문제 해결
```bash
cd ~/devops/week04/site

cp "/mnt/c/<경로>/index.html" .   -> 현재 경로로 index.html 파일 복사

python3 -m http.server 8080    -> 학습 및 테스트용 간단한 웹 서버 실행
```

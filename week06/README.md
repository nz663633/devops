### 도커 이미지 주소 읽기
- 테스트용 이미지 실행
```
docker run -d -p 8081:5000 --name guestbook-ghcr ghcr.io/hyeonji/inhatc-devops-guestbook:v1

# ghcr.io -> 레지스트리(도메일)
# hyeonji -> 사용자(계정 / 경로)
# inhatc-devops-guestbook -> 이미지 이름
# v1 -> 태그

# 레지스트리 생략하면 도커 허브
```
### Dockerfile
- Dockerfile: 도커 이미지를 자동으로 생성하기 위해 필요한 명령어와 설정을 순서대로 기록한 파일<br>
  -> 텍스트로 작성하면, 누구나 몇 번이든, 똑같이 만들 수 있음<br>
  -> Dockerfile 명령을 위에서부터 순서대로 처리하며 이미지 생성<br>
```
# 1. 재료 선택(기반 이미지)
FROM python:3.12-slim

# 2. 작업 디렉터리 설정(없으면 생성)
WORKDIR /app

# 3. 내 파일을 이미지 안으로 복사
# 호스트 파일 -> 이미지
COPY requirements.txt .

# 4. 실행할 명령
# RUN은 이미지를 만들 때 한 번만 실행
RUN pip install -r requirements.txt

# 5. 환경 변수 설정
# 환경변수 기본값을 이미지에
ENV APP_TITLE = "InhaTC DevOps 방명록"

# 6. 포트번호 문서화
EXPOSE 5000

# 7. 컨테이너 시작 시 실행할 명령
# CMD는 컨테이너 실행할 때마다 실행
CMD ["python", "app.py"]
```
- 레이어 1~4번은 변하지 않으면 캐시 재사용(빠름)<br>
  -> 한 레이어가 바뀌면, **그 아래 레이어는 전부 다시 생성**<br>
  -> 한 단계의 캐시가 무효화되면 이후 단계도 다시 처리<br>
  -> **자주 바뀌는 것을 뒤에** 두면 전체 속도가 빨라짐<br>

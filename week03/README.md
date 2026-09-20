## Git 명령어
```
git log                      ---> 커밋 기록을 자세하게 확인(커밋 해시, 작성자, 작성 날짜, 커밋 메시지 등)
git log --oneline            ---> git log를 한 줄씩 간단하게 보여줌
git switch -d `<커밋해시>`   ---> 특정 커밋으로 이동(-d: detach)
git switch main              ---> 다시 현재 브랜치로 돌아오기

git diff `<커밋 해시1>` `<커밋 해시2>`   ---> 커밋 변경 비교(1 -> 2로 변경된 내용을 보여줌)
git log --oneline --all --graph          ---> 모든 브랜치의 커밋을 한 줄 형식으로 보여주면서,
                                         ---> 브랜치의 분기/병합 구조까지 그래프로 보여줌
[Github 레포지토리 만들기]
gh repo create `<레포이름>` --public --source=. --remote=origin --push
* --public : 공개 저장소로 생성
* --source=. : 현재 디렉터리를 원본으로 사용 -> 현재 디렉터리의 Git 저장소를 Github에 연결
* --remote=origin : 원격 저장소 이름 지정
* origin : 연결된 원격 저장소에 관례적으로 사용하는 이름
* --push : 로컬의 커밋을 원격 저장소에 업로드
```
-------
## 셸 스크립트
- 셸에서 실행할 명령어들을 하나의 파일에 여러 개 작성해 놓은 것

- `<파일 이름.sh>`로 실행 불가능 ($ start.sh)
- ./`<파일 이름.sh>`으로 실행 ($ ./start.sh)
- 명령어 이름만으로 실행하면 PATH에 등록된 디렉터리에서 찾음
-------
## Owen과 Ollama
1. Owen
    * 알리바바가 개발한 생성형 AI 모델
    * 질문에 답하거나 코드를 작성하고, 문서를 분석하는 등의 작업 가능
-> 우리가 평가해야 할 것: 답변의 지능보다는 실행, 종료, 재실행의 흐름 확인
2. Ollama
    * 대규모 언어 모델(LLM)을 간단한 명령어만으로 로컬 환경에서 실행할 수 있도록 돕는 플랫폼
```
stream - 응답을 어떻게 전달받을지 결정
stream: false -> 생성 완료 후 응답을 한 번에 보여주기

think - 모델의 추론 과정과 관련된 옵션
think: false -> 생각 모드 비활성화 
```
-------
$ nano start.sh
```bash
#!/bin/bash
cd "$(dirname "$0")" || exit 1
exec python3 chat.py

#! : 셔뱅(shebang)
   -> "이 파일을 실행할 때, 바로 뒤에 적힌 프로그램으로 이 파일을 해석할 것"
#!/bin/bash
   -> "이 파일을 Bash 문법이니, Bash로 실행할 것"
cd "$(dirname "$0")" || exit 1
   -> 해당 스크립트가 있는 디렉터리로 이동, 이동 실패시 스크립트 종료
   -> cd 성공시, exit status = 0 -> True
   -> cd 실패시, exit status = 1 -> False
   -> || : 논리 OR, 앞 명령이 실패하면 뒤 명령을 실행하고 실패를 의미하는 상태코드 1을 남김
exec python3 chat.py
   -> 현재 bash를 python3으로 대체(PID 유지)
```
```bash
cat > start_with_export.sh << 'EOF'
   -> 입력하는 여러 줄의 내용을 start_with_export.sh에 저장하되, 문자열 EOF이 나오면 입력 종료
   ->  
```

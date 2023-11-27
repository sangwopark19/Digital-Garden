# Git
---


## mac git 초기 설정
---
Git 전역으로 사용자 이름과 이메일 주소를 설정, GitHub 계정과는 별개
git config --global user.name "(본인 이름)"
git config --global user.email "(본인 이메일)"
아래의 명령어들로 확인
git config --global user.name
git config --global user.email

기본 브랜치명 변경
git config --global init.defaultBranch main

줄바꿈 호환 문제 해결
git config --global core.autocrlf input

push시 로컬과 동일한 브랜치명으로
git config --global push.default current

기본 에디터 vscode 설정
git config --global core.editor "code --wait"

### 설정값 확인
---
1) 현재 모든 설정값 보기
git config (global) --list

2) 에디터에서 보기
git config (global) -e

## alias
---
명령어를 편하게 사용하기 위한 단축어 설정
```console
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```

이미 있는 명령을 편리하고 새로운 명령으로 만들 수 있다.
아래는 최근 커밋을 확인하는 명령어 alias이다.
```console
git config --global alias.last 'log -1 HEAD'
```

# gitignore
---
[[Git]]으로 작업을 하다보면 내가 작업하는 공간에는 필요하지만 remote(원격)에 push를 무시해야 하는 경우가 있다. 이런 경우는 소스 파일이라던가 올리면 충돌이 일어나 오류를 범할 수 있기 때문에 .gitignore 설정이 필요하다.

## 예시
---
```
# 파일무시
test.txt

# 다음과 같은 확장자는 전체 무시
*.text
*.exe
*.zip

# 폴더 무시
test/
```

폴더와 같은 경우는 폴더 하위의 파일이나 폴더 또한 ignore(무시) 된다.
**.gitignore 파일에서 # 뒤에 쓰는 내용은 주석처리 된다.**

.gitignore 파일에 작성 했다면 add > commit > push 까지 해야 ignore 가 적용된다.

```
git add .
git commit -m "ignore file&folder config"
git push origin main
```

## **주의사항**
---
기존의 [[git]]의 관리를 받고 있던 (commit된 것들) 파일이나 폴더를 .gitignore 파일에 작성하고 add > commit> push 하여도 ignore(무시) 되지 않는다.

이럴때는 기존에 가지고 있는 cached를 지워야 한다.
다음과 같은 명령어를 작성한다.
```
# 파일 이라면
git rm --cached test.txt

# 전체파일 이라면
git rm --cached *.txt

# 폴더 라면
git rm --cached test/ -r
```

git rm --cache 명령어는 Staging Area(add를 하고나서의 영역)에서 파일을 제거하고 working directory(Local)에서는 파일을 유지하는 명령어이다.
위의 명령어를 실행한 후 꼭 commit을 해줘야 한다.

### 추가로
기존에 관리하고 있던(commit된 것들) 파일을 cached 명령어를 안쓰고 무시하는 방법이 있다. 명령어는 다음과 같다.

`git update-index --assume-unchanged [파일명]

다음과 같이 무시를 선언하고 다시 취소하는 방법도 있다.
`git update-index --no-assume-unchanged [파일명]`

수정사항 무시 파일 조회는 다음 명령어를 사용한다.
`git ls-files -v|grep '^h'`

---
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
기존의 [[git]]의 관리를 받고 있던 (commit된 것들) 파일이나 폴더를 .gitig

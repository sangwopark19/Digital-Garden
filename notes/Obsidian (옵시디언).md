# Obsidian
---
마크다운방식 노트

> [!note] 글 구성
> 1. [팁](#팁)
> 2. [문법](#문법)
> 3. [맥 단축키](#맥-단축키)
> 4. [윈도우 단축키](#윈도우-단축키)

## 팁
---
> [!note] 외부링크
> [단축키, 문법, 플러그인등 꿀팁 한국어 블로그](https://statisticsplaybook.com/obsidian/#gsc.tab=0)
## 문법
---
### MarkDown
- [[MarkDown]]
### Obsidian MarkDown
## 옵시디언 전용 마크다운 문법 정리

### 1. 하이라이트 (Highlight)

마크다운에는 기본적으로 텍스트 하이라이트 기능이 없습니다. 그러나, 옵시디언에서는 `===` 를 사용하여 텍스트를 하이라이트 할 수 있습니다.

```r
==하이라이트 텍스트==
```

Copy

- 결과

![옵시디언 마크다운 문법 정리 노트 - 마크다운 하이라이트 삽입 방법](https://statisticsplaybook.com/wp-content/uploads/2023/10/image-28.png "매일 찾아보는 옵시디언 마크다운 문법 정리 13")

### 2. 체크박스 (Check Box)

마크다운으로 체크 박스나 체크 리스트를 삽입하려면 각 목록 항목을 하이픈(`-`)과 공백 뒤에 `[ ]`로 시작하시면 됩니다. 공백 안에 x 표시를 할 경우, 목록에 취소선이 생기게 됩니다. 공백 안을 다른 문자로 채우게 되면, 완료 표시로 바뀌게 됩니다.

```r
### 해야 할 일들

- [x] 화분 물 주기
- [-] 우편함 확인
- [ ] 밀린 일기 작성
```

Copy

- 결과

![옵시디언 마크다운 문법 정리 노트 - 마크다운 체크박스 및 마크다운 체크리스트 만들기](https://statisticsplaybook.com/wp-content/uploads/2023/10/image-29.png "매일 찾아보는 옵시디언 마크다운 문법 정리 14")

#### 참고: 옵시디언 체크박스 단축키

옵시디언에서는 체크 박스를 쉽게 만들 수 있도록 단축키가 기본으로 제공됩니다. 체크박스를 만들기 원하는 행에 커서를 놓고 `Ctrl + L` 단축키를 누르게 되면 자동으로 체크 박스로 변하게 됩니다.

### 3. 그림 크기(Images Size)

앞에서 설명한 일반적인 마크다운 이미지 삽입 코드에 옵시디언에서만 작동하는 이미지 크기를 조절하는 옵션이 존재합니다. `![대체 텍스트|가로x세로](이미지 URL)` 형식을 사용합니다.

```r
![위키피디아 흑요석|100x100](https://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/ObsidianOregon.jpg/360px-ObsidianOregon.jpg)
```

Copy

- 결과

![옵시디언 마크다운 문법 정리 노트 - 마크다운 이미지 크기 조절 방법](https://statisticsplaybook.com/wp-content/uploads/2023/10/image-30-150x150.png "매일 찾아보는 옵시디언 마크다운 문법 정리 15")

### 4. 문서 링크 (Internal Links)

내부 링크는 노트 간 또는 블록 간에 서로 연결되는 링크입니다. `[[]]` 안에 페이지 이름을 넣어서 생성할 수 있습니다. 이를 통해 빠르게 관련 정보로 이동할 수 있습니다. 예를 들어, Vault 안에 “옵시디언 사용법”이라는 노트가 있는 경우, 다음과 같이 작성하면, 옵시디언 사용법 노트로 가는 링크가 생성됩니다.

```r
[[옵시디언 사용법]]
```

Copy

- 결과

![옵시디언 마크다운 문법 정리 노트 - 마크다운 노트 간 링크 생성](https://statisticsplaybook.com/wp-content/uploads/2023/10/image-32.png "매일 찾아보는 옵시디언 마크다운 문법 정리 16")

### 5. 문서 임베딩 (Embedding Links)

내부 링크와는 다르게 노트 내용 자체를 다른 노트에 박아놓을 때 사용하는 마크다운 문법입니다. 알고 있는면 상당히 유용합니다. 아래 결과를 보면 현재 노트에 다른 노트가 임베딩 되어 있다는 표시가 되어 있는 것을 확인 할 수 있습니다.

```r
![[옵시디언 사용법]]
```

Copy

- 결과

![image 33](https://statisticsplaybook.com/wp-content/uploads/2023/10/image-33.png "매일 찾아보는 옵시디언 마크다운 문법 정리 17")

### 6. 문단 참조(Block Reference)

블록 참조는 다른 블록 내용을 현재 블록에 포함시키는 기능입니다. `![[블록 링크#^id]]` 형식으로 사용합니다.

![image 34](https://statisticsplaybook.com/wp-content/uploads/2023/10/image-34.png "매일 찾아보는 옵시디언 마크다운 문법 정리 18")

옵시디언 사용법 노트 안에 한 문단을 myobsi 아이디로 부여한 상태

```r
![[옵시디언 사용법#^myobsi]]
```

Copy

- 결과

![image 35](https://statisticsplaybook.com/wp-content/uploads/2023/10/image-35.png "매일 찾아보는 옵시디언 마크다운 문법 정리 19")

위와 같이 ID를 부여한 문단 만 임베딩 된 것을 확인 할 수 있습니다.

### 7. 강조 상자 넣기 (Callout, 말풍선)

내용을 쓰다가 강조하는 부분이 있다거나, 정리를 할 때 유용한 문법입니다.

```r
> [!note]  이것만  알아두세요!
>  옵시디언은 정말 편한 도구입니다.
```

Copy

![image 36](https://statisticsplaybook.com/wp-content/uploads/2023/10/image-36-768x146.png "매일 찾아보는 옵시디언 마크다운 문법 정리 20")

노트 Callout으로 강조된 부분

#### Callout 지원리스트

Callout 문법이 좋은 이유는 위의 문법에서  `[!note]` 부분을 다음과 같은 키워드로 바꿔주면 알맞은 아이콘에 색상이 다른 Callout 상자가 생성된다는 것입니다. 한번씩 시도해보세요! (같은 줄에 있는 단어들은 같은 아이콘 유사어들입니다.)

- [!note]
- [!abstract], [!summary], [!tldr]
- [!info]
- [!todo]
- [!tip], [!hint], [!important]
- [!success], [!check], [!done]
- [!question], [!help], [!faq]
- [!warning], [!caution], [!attention]
- [!failure], [!fail], [!missing]
- [!danger], [!error]
- [!bug]
- [!example]
- [!quote], [!cite]

## 맥단축키
---
## 옵시디언 단축키 (MacOS)

### 일반 명령어

|동작|단축키|
|---|---|
|복사하기|`Cmd + C`|
|잘라내기|`Cmd + X`|
|붙여넣기|`Cmd + V`|
|서식 없이 붙여넣기|`Cmd + Shift + V`|
|실행 취소|`Cmd + Z`|
|다시 실행|`Cmd + Shift + Z`|
|문단 전체복사|`Cmd + C`  <br>(글자 선택 없이)|
|문단 잘라내기|`Cmd + X`  <br>(글자 선택 없이)|

### 문서 편집

|동작|단축키|
|---|---|
|새로운 줄 삽입|`Enter`|
|이전 문자 삭제|`Backspace`|
|다음 문자 삭제|`Delete`|
|이전 단어 삭제|`Option + Backspace`|
|다음 단어 삭제|`Option + Delete`|
|현재 줄의 시작부터 삭제|`Cmd + Backspace`|
|현재 줄의 끝까지 삭제|`Cmd + Delete`|
|현재 줄 삭제|`Cmd + Shift + K`  <br>(글자 선택 없이)|

### 커서 이동

|동작|단축키|
|---|---|
|커서 한 문자 앞으로|`←`|
|커서 이전 단어의 처음으로|`Option + ←`|
|커서 다음 단어의 끝으로|`Option + →`|
|커서 현재 줄의 처음으로|`Cmd + ←`|
|커서 현재 줄의 끝으로|`Cmd + →`|
|커서 이전 줄로|`↑`|
|커서 다음 줄로|`↓`|
|커서 노트의 처음으로|`Cmd + ↑`|
|커서 노트의 끝으로|`Cmd + ↓`|
|커서 한 페이지 위로|`Ctrl + ↑`|
|커서 한 페이지 아래로|`Ctrl + ↓`|

### 글자 선택

|동작|단축키|
|---|---|
|선택 해제|`Esc`|
|전체 선택|`Cmd + A`|
|선택 영역을 한 문자씩 확장|`Shift + ←/→`|
|선택 영역을 이전 단어의 시작 부분까지 확장|`Option + Shift + ←`|
|선택 영역을 다음 단어의 끝 부분까지 확장|`Option + Shift + →`|
|선택 영역을 현재 줄의 시작 부분까지 확장|`Cmd + Shift + ←`|
|선택 영역을 현재 줄의 끝 부분까지 확장|`Cmd + Shift + →`|
|선택 영역을 노트의 시작 부분까지 확장|`Cmd + Shift + ↑`|
|선택 영역을 노트의 끝 부분까지 확장|`Cmd + Shift + ↓`|
|선택 영역을 한 페이지 위로 확장|`Ctrl + Shift + ↑`|
|선택 영역을 한 페이지 아래로 확장|`Ctrl + Shift + ↓`|


## 윈도우 단축키
---
## 옵시디언 단축키 (Windows & Linux)

### 일반 명령어

|동작|단축키|
|---|---|
|복사하기|`Ctrl + C`|
|잘라내기|`Ctrl + X`|
|붙여넣기|`Ctrl + V`|
|서식 없이 붙여넣기|`Ctrl + Shift + V`|
|실행 취소|`Ctrl + Z`|
|다시 실행|`Ctrl + Shift + Z` or `Ctrl + Y`|
|문단 전체복사|`Ctrl + C`  <br>(글자 선택 없이)|
|문단 잘라내기|`Ctrl + X`  <br>(글자 선택 없이)|

### 문서 편집

|동작|단축키|
|---|---|
|새로운 줄 삽입|`Enter`|
|이전 문자 삭제|`Backspace`|
|다음 문자 삭제|`Delete`|
|이전 단어 삭제|`Ctrl + Backspace`|
|다음 단어 삭제|`Ctrl + Delete`|
|현재 줄 삭제|`Ctrl + Shift + K`  <br>(글자 선택 없이)|

### 커서 이동

|동작|단축키|
|---|---|
|커서 한 문자 앞으로|`←`|
|커서 이전 단어의 처음으로|`Ctrl + ←`|
|커서 다음 단어의 끝으로|`Ctrl + →`|
|커서 현재 줄의 처음으로|`Home`|
|커서 현재 줄의 끝으로|`End`|
|커서 이전 줄로|`↑`|
|커서 다음 줄로|`↓`|
|커서 노트의 처음으로|`Ctrl + Home`|
|커서 노트의 끝으로|`Ctrl + End`|
|커서 한 페이지 위로|`Page Up`|
|커서 한 페이지 아래로|`Page Down`|

### 글자 선택

|동작|단축키|
|---|---|
|선택 해제|`Esc`|
|전체 선택|`Ctrl + A`|
|선택 영역을 한 문자씩 확장|`Shift + ←/→`|
|선택 영역을 이전 단어의 시작 부분까지 확장|`Ctrl + Shift + ←`|
|선택 영역을 다음 단어의 끝 부분까지 확장|`Ctrl + Shift + →`|
|선택 영역을 현재 줄의 시작 부분까지 확장|`Shift + Home`|
|선택 영역을 현재 줄의 끝 부분까지 확장|`Shift + End`|
|선택 영역을 노트의 시작 부분까지 확장|`Ctrl + Shift + Home`|
|선택 영역을 노트의 끝 부분까지 확장|`Ctrl + Shift + End`|
|선택 영역을 한 페이지 위로 확장|`Shift + Page Up`|
|선택 영역을 한 페이지 아래로 확장|`Shift + Page Down`|

### 창 관리

|동작|단축키|
|---|---|
|새 창 열기|`Ctrl + N`|
|창 닫기|`Ctrl + W`|
|이전 창으로 이동|`Ctrl + Page Up`|
|다음 창으로 이동|`Ctrl + Page Down`|



# asdf
---
각종 언어, 도구들의 버전 관리를 쉽게 해주는 프로그램


## 시작
---
1. 종속성 설치
2. asdf 코어 다운로드
3. asdf 설치
4. 관리하려는 각 도구/런타임에 대한 플러그인 설치
5. 도구/런타임 버전 설치
6. `.tool-versions`구성 파일을 통해 전역 및 프로젝트 버전 설정

### 1. 종속성 설치
**최근에 나온 hombrew는 이 단계를 건너뛰어도 됨**

맥OS: 
- homebrew: `brew install coreutils curl git`

### 2. asdf 다운로드
`brew install asdf`

### 3. asdf 설치
ZSH & homebrew 기준
~/.zshrc에 스크립트 추가
`echo -e "\n. $(brew --prefix asdf)/libexec/asdf.sh" >> ${ZDOTDIR:-~}/.zshrc` 

### 4. 플러그인 설치
`Node.js`기준
플러그인 설치
`asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.git`

### 5. 버전 설치
`asdf list all nodejs` 로 모든 버전 검색 또는 
`asdf list all nodejs 14` 14 이하의 모든 버전을 검색할 수 있다.
`asdf install nodejs latest`는 사용가능한 안정적인 최신버전을 설치한다.

### 6. 버전 설정
`asdf`는 **현재 작업 디렉토리**에서 `$HOME`(사용자 홈 디렉토리)까지의 모든 `.tool-versions`파일에서 tool의 버전 조회를 한다. 현재 작업영역에서 가장 가까운 디렉토리의 `.tool-versions`설정이 적용된다.
> 경고
> tool에 대한 버전이 나열되어 있지 않으면 실행에 **오류**가 발생한다. `asdf current`는 현재 디렉토리에서 tool 및 버전 오류를 표시하며 어떤 tool이 실행되지 않는지 확인할 수 있다.

#### 글로벌
전역 설정은 `$HOME/.tool-versions`에서 관리된다. 
명령어:
`asdf global nodejs latest`
현재 `$HOME/.tool-versions`는 다음과 같다
`nodejs 21.x.x`

> 일부 os에는 asdf가 아닌 시스템에서 관리하는 tool이 이미 설치되어 있으며 맥에선 `python`이 일반적인 예이다. `asdf`에 관리를 시스템으로 다시 전달하도록 지시해야 한다.
> https://asdf-vm.com/manage/versions.html

#### 로컬
로컬 설정은 `$PWD/.tool-versions`파일 **(현재 작업 디렉토리)** 에 정의되어 있다. 일반적으로 이것은 프로젝트의 **GIt 저장소에 들어간다**. 
원하는 디렉토리에서 다음 코드를 실행한다.
`asdf local nodejs latest`
현재 `$PWD/.tool-versions`는 다음과 같다.
`nodejs 21.x.x`


## 플러그인

### 추가
git url로 추가
`asdf plugin add <name> <git-url>`
또는 단축명 으로 추가
`asdf plugin add <name>`

### 플러그인 목록
`asdf plugin list`

url 포함 보기
`asdf plugin list --urls`

### 모든 플러그인 보기
````
asdf plugin list all
````



### 업데이트
모든 플러그인 업데이트
`asdf plugin update --all`
특정 플러그인 업데이트
`asdf plugin update <name>`



## 관련자료
---
[asdf 공식문서](https://asdf-vm.com/guide/getting-started.html)
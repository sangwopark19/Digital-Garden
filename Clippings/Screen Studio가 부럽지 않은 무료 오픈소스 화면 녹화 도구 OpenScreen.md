---
title: "siddharthvaddem/openscreen: Create stunning demos for free. Open-source, no subscriptions, no watermarks, and free for commercial use. An alternative to Screen Studio."
source: "https://github.com/siddharthvaddem/openscreen"
author:
published:
created: 2026-04-06
description: "Create stunning demos for free. Open-source, no subscriptions, no watermarks, and free for commercial use. An alternative to Screen Studio.  - siddharthvaddem/openscreen: Create stunning demos for free. Open-source, no subscriptions, no watermarks, and free for commercial use. An alternative to Screen Studio."
tags:
  - "clippings"
---
구독료와 워터마크 없이 전문적인 제품 데모 영상을 제작할 수 있는 Electron 기반 오픈소스 화면 녹화 도구이다.

## 요약

유료 서비스인 Screen Studio의 핵심 기능을 무료로 제공하기 위해 개발된 오픈소스 프로젝트이다. Electron과 PixiJS를 기반으로 구축되어 부드러운 줌 효과, 모션 블러, 주석 추가 등 고품질 데모 제작에 필요한 필수 기능을 구현했다. 복잡한 설정 없이 직관적인 인터페이스를 통해 전문적인 제품 워크스루 영상을 제작하려는 환경에서 활용도가 높다.

## 주요 기능

- 전체 화면 및 특정 윈도우 대상 고화질 녹화 수행
- 자동 및 수동 줌 기능을 통한 시각적 강조 효과 적용
- 마이크와 시스템 오디오 동시 캡처 및 동기화 처리
- 모션 블러 효과를 적용한 부드러운 화면 전환 구현
- 텍스트, 화살표, 이미지를 활용한 실시간 주석 추가

## 어떻게 동작하는가

Electron의 desktopCapturer API로 화면 소스를 확보하고 PixiJS 렌더링 엔진을 통해 실시간 줌 및 그래픽 효과를 합성하여 비디오 스트림을 생성한다.

> [!warning] Warning
> 이것은 아직 베타 버전이며 곳곳에 버그가 있을 수 있습니다(하지만 좋은 경험이 되기를 바랍니다!).

[![OpenScreen Logo](https://github.com/siddharthvaddem/openscreen/raw/main/public/openscreen.png)](https://github.com/siddharthvaddem/openscreen/blob/main/public/openscreen.png)  
  

## OpenScreen

**OpenScreen은 Screen Studio의 무료 오픈소스 대안입니다(일종의).**

Screen Studio에 월 $29를 지불하고 싶지 않지만 대부분의 사람들이 필요로 하는 것, 즉 아름다운 제품 데모와 워크스루를 만들 수 있는 훨씬 더 간단한 버전을 원한다면, 여기 무료로 사용할 수 있는 앱이 있습니다. OpenScreen은 Screen Studio의 모든 기능을 제공하지는 않지만 기본 기능은 잘 다룹니다!

Screen Studio는 훌륭한 제품이며 이것은 절대 1:1 클론이 아닙니다. OpenScreen은 훨씬 더 간단한 접근 방식으로, 제어권을 원하면서 비용을 지불하고 싶지 않은 사람들을 위한 기본 기능만 제공합니다. 모든 멋진 기능이 필요하다면 Screen Studio를 지원하는 것이 최선입니다 (정말 훌륭한 일을 하고 있어요, 하하). 하지만 단순히 무료(숨겨진 비용 없음)이고 오픈소스인 것을 원한다면 이 프로젝트가 제 역할을 합니다!

OpenScreen은 개인 및 상업적 용도로 100% 무료입니다. 사용하고, 수정하고, 배포하세요. (그냥 멋있게 😁 하고 싶으면 한 번 언급해 주세요!)

[![OpenScreen App Preview 3](https://github.com/siddharthvaddem/openscreen/raw/main/public/preview3.png)](https://github.com/siddharthvaddem/openscreen/blob/main/public/preview3.png) [![OpenScreen App Preview 4](https://github.com/siddharthvaddem/openscreen/raw/main/public/preview4.png)](https://github.com/siddharthvaddem/openscreen/blob/main/public/preview4.png)

## Core Features

- 특정 창 또는 전체 화면을 녹화합니다.
- 자동 또는 수동 줌을 추가하고(조정 가능한 깊이 수준) 지속 시간과 위치를 사용자 정의합니다.
- 마이크 및 시스템 오디오를 녹화합니다.
- 비디오 녹화를 자르기하여 특정 부분을 숨깁니다.
- 배경화면, 단색, 그래디언트 또는 사용자 정의 배경 중에서 선택하세요.
- 더 부드러운 팬 및 줌 효과를 위한 모션 블러.
- 주석(텍스트, 화살표, 이미지)을 추가하세요.
- 클립의 섹션을 자르세요.
- 다양한 세그먼트의 속도를 사용자 정의합니다.
- 다양한 종횡비 및 해상도로 내보냅니다.

## 설치

[GitHub Releases](https://github.com/siddharthvaddem/openscreen/releases) 페이지에서 플랫폼에 맞는 최신 설치 프로그램을 다운로드하세요.

### macOS

macOS Gatekeeper가 앱을 차단하는 문제가 발생하면(개발자 인증서가 없기 때문), 설치 후 터미널에서 다음 명령을 실행하여 이를 우회할 수 있습니다:

```
xattr -rd com.apple.quarantine /Applications/Openscreen.app
```

**시스템 설정 > 개인정보 보호 및 보안** 에서 터미널에 전체 디스크 접근 권한을 부여한 후 위의 명령어를 실행하세요.

이 명령어를 실행한 후 **시스템 환경설정 > 보안 및 개인정보 보호** 로 이동하여 "화면 녹화" 및 "손쉬운 사용" 권한을 부여하세요. 권한이 부여되면 앱을 실행할 수 있습니다.

### Linux

릴리스 페이지에서 `.AppImage` 파일을 다운로드하세요. 실행 가능하도록 설정한 후 실행하세요:

```
chmod +x Openscreen-Linux-*.AppImage
./Openscreen-Linux-*.AppImage
```

데스크톱 환경에 따라 화면 녹화 권한을 부여해야 할 수 있습니다.

**참고:** "sandbox" 오류로 인해 앱이 실행되지 않으면 --no-sandbox 옵션으로 실행하세요:

```
./Openscreen-Linux-*.AppImage --no-sandbox
```

### 제한사항

시스템 오디오 캡처는 Electron의 [desktopCapturer](https://www.electronjs.org/docs/latest/api/desktop-capturer) 에 의존하며 플랫폼별로 몇 가지 특이사항이 있습니다:

- **macOS**: macOS 13 이상이 필요합니다. macOS 14.2 이상에서는 오디오 캡처 권한을 부여하라는 메시지가 표시됩니다. macOS 12 이하는 시스템 오디오를 지원하지 않습니다(마이크는 여전히 작동함).
- **Windows**: 별도의 설정 없이 작동합니다.
- **Linux**: PipeWire가 필요합니다(Ubuntu 22.04+, Fedora 34+에서 기본값). 이전의 PulseAudio 전용 설정은 시스템 오디오를 지원하지 않을 수 있습니다(마이크는 여전히 작동해야 함).

## Built with

---

*오픈소스는 처음이라 뭘 하는지 모르겠어요 ㅋㅋ 뭔가 잘못되면 이슈를 올려주세요 🙏*

## 기여하기

기여를 환영합니다! 도움을 주고 싶거나 현재 진행 중인 작업을 보고 싶다면, 오픈 이슈와 [프로젝트 로드맵](https://github.com/users/siddharthvaddem/projects/3) 을 확인하여 프로젝트의 현재 방향을 이해하고 기여할 수 있는 방법을 찾아보세요.

## 라이선스

이 프로젝트는 [MIT 라이선스](https://github.com/siddharthvaddem/openscreen/blob/main/LICENSE) 에 따라 라이선스가 부여됩니다. 이 소프트웨어를 사용함으로써 저자들이 그 사용으로 인해 발생하는 어떤 문제, 손해 또는 청구에 대해서도 책임이 없음에 동의합니다.
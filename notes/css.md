## inline / block 깨달음
---
가운데 정렬을 하기위해 text-aline: center를 할때 가 있다
이 때 inline요소는 text로 취급되어 가운데 정렬이 되지만 block 요소는 적용되지 않는다.
이는 특히 원래 인라인 요소 였던 button같은 요소를 다음 줄로 넘기기 위해 block으로 바꿨을 때 많이 일어난다. 이 때 해결 방법은 무엇일까?
바로 `margin: (원하는 상하 마진 값) auto`를 적용하면 된다 또는 상하 마진값을 서로 다르게 주고 싶다면 `margin: (top) auto (bottom)`을 적용하면 된다.

이 해결방법의 원리는 무엇일까? 사실 간단하다.
inline과 block의 속성을 다시 생각해보면 되는데 inline은 width, hegiht 값이 적용되지 않고 margin, padding 값이 존중되지 않는다. 즉 block 요소의 값들이 전부 제대로 적용이 안되는 대신 글자 취급을 받아 text-aline의 적용을 받는 것이다.

반대로 block 요소 같은 경우엔 width, hegiht가 적용되고 maring, padding 을 존중받고 또한 block 요소에 특별한 값을 주지않으면 기본적으로 그 줄의 모든 가로 값을 자신이 먹기 때문에 text-aline의 위치값이 적용되지 않는것이다.


## css 재설정
---
### 1. 재설정 라이브러리들이 많이 있다

### 2. 줄높이 재설정
기본 줄높이는 1.15
`:root { line-height: 1.5; }` 로 1.5로 설정해주자
>[Josh Comeau](https://www.joshwcomeau.com/css/custom-css-reset/)가 CSS 재설정 기사에서 지적했듯이 [WCAG 기준](https://www.w3.org/WAI/WCAG21/Understanding/text-spacing.html)은 줄 높이가 1.5 이상이어야한다고 명시합니다.

### 3. 여백 지우기
기본 여백을 제거하면 레이아웃 골치거리를 많이 줄일 수 있다.
`h1, h2, h3, h4, h5, figure, p, ol, ul { margin: 0; }`

### 4. h태그 스타일 재설정
`h1, h2, h3, h4, h5 { font-size: inherit; font-weight: inherit; }`

### 5. 기본 리스트 스타일 제거
li, ol의 `list-style`을 평범하게 제거 하면 Safari가 VoiceOver에서 콘텐츠를 다르게 알리므로 문제가 될 수 있다.
[이를 위해 Andy Bell의 솔루션을](https://piccalil.li/blog/a-more-modern-css-reset/) 채택했는데, `role="list"` 가있을 때만 목록 스타일을 제거하는 것이다. 역할을 지정하면 VoiceOver 회귀를 방지할 수 있습니다.

`ol[role="list"], ul[role="list"] { list-style: none; padding-inline: 0; }`


### 6. 반응형 이미지
최신 휴대폰에는 2x 및 3x hi-dpi 디스플레이가 있다. 즉, 예를 들어서 css에서 `300px`로 측정된 페이지 영역이 멋지고 선명하게 보이려면 600 또는 900 기기 픽셀 상당의 이미지 데이터가 필요하다. 즉, 큰 이미지를 더 작은 공간에 "압축"해야 하는 경우가 많다.

그러나 기본 브라우저 `<img>` 스타일은 큰 이미지가 단순히 오버플로되는 결과를 초래하므로 바람직하지 않다.
`img { display: block; max-inline-size: 100%; }`
 이렇게 하면 큰 이미지가 오버플로되어 레이아웃 문제가 발생하는 대신 컨테이너 내에 맞게 축소된다.

### 재설정을 취소 해야 하는 경우 어떻게 해야 하나?

때로는 스타일 세트를 지우는것이 올바른 결정이지만 브라우저의 기본스타일이 있었으면 좋을때가 있다. `h`태그가 좋은 예이다. 대부분의 경우 브라우저 기본값 대신 제목 스타일을 완전히 제어하기를 원한다.

그러나 이용 약관 페이지와 같은 긴 형식의 콘텐츠를 포맷하는경우 재설정을 "취소"하고 싶을 것이다. 이러한 경우 css `revert` 키워드를 사용할 수 있다.
`.article :where(h1, h2, h3, h4, h5) { all: revert; }`

`.article`내에서 `h`태그는 원래 브라우저의 기본 스타일을 사용하여 적절한 글꼴 크기와 굵기로 표시되며, revert 기능으로 인해 원래대로 표시된다.


### 결론
---
새 프로젝트에서 CSS에 대한 접근 방식을 선택할 때 다음 사항을 고려하자.

1. 1. Tailwind와 같은 CSS 프레임 워크가 프로젝트에 적합합니까? 프레임워크는 일반적으로 자체 정규화 및/또는 재설정을 번들로 제공합니다.
2. 1. CSS를 처음부터 작성하시겠습니까? [먼저 modern-normalize](https://github.com/sindresorhus/modern-normalize)를 추가합니다. 이렇게 하면 `border-box`가 제공되고 버그, 접근성 문제 및 개발 중에 눈에 띄지 않고 사용자에게 영향을 미칠 수 있는 기타 바람직하지 않은 동작을 수정할 수 있습니다.
3. 1. 브라우저 기본 디자인을 원한다면 CSS 재설정이 필요하지 않을 수 있습니다. 그렇지 않으면 아래의 경량 버전과 같이 하나를 추가하는 것을 진지하게 고려하십시오. 장기적으로 반복적인 스타일링을 많이 줄일 수 있습니다. 또한 프로젝트 후반에 CSS 재설정을 추가하는 것은 위험할 수 있으므로 미리 결정하는 것이 가장 좋습니다.
```css
@import "modern-normalize"; :root { line-height: 1.5; } h1, h2, h3, h4, h5, figure, p, ol, ul { margin: 0; } ol[role="list"], ul[role="list"] { list-style: none; padding-inline: 0; } h1, h2, h3, h4, h5 { font-size: inherit; font-weight: inherit; } img { display: block; max-inline-size: 100%; }
```

## 모바일 접근성
---
### 글꼴이 작을때 자동으로 확대됨

예를 들어 `<textarea>`은 시스템 기본 고정 폭 글꼴을 사용합니다. 텍스트 입력에는 시스템 기본 sans-serif 글꼴이 사용됩니다. 그리고 둘 다 아주 작은 글꼴 크기(Chrome에서는 13.333px)를 선택합니다.

상상할 수 있듯이 모바일 장치에서 13px 텍스트를 읽는 것은 매우 어렵습니다. 작은 글꼴 크기로 입력에 초점을 맞추면 브라우저가 자동으로 확대되어 텍스트를 더 쉽게 읽을 수 있습니다.

안타깝게도 이는 좋은 경험이 아닙니다.

이러한 자동 확대 동작을 피하려면 입력의 글꼴 크기가 최소한 1rem / 16px이어야 합니다. 문제를 해결하는 한 가지 방법은 다음과 같습니다.

```css
input, button, textarea, select {

  font-size: 1rem;

}
```

이렇게 하면 자동 확대/축소 문제가 해결되지만 반창고일 뿐입니다. 대신 근본 원인을 해결해 보겠습니다. 양식 입력에는 고유한 인쇄 스타일이 있어서는 안 됩니다!

```css
input, button, textarea, select {

  font: inherit;

}
```


## 일부 콘텐츠만 컨테이너 크기를 넘어 확장하고 싶을때
---
![[Pasted image 20240614172211.png]]

다음과 같이 컨테이너 크기를 제한하고 웹페이지를 작성하는데 특정 콘텐츠만 컨테이너를 넘어 확장하고 싶을 때가 있다.

과거에는 다음과 같이 확장된콘텐츠를 제외한 모든것을 래핑하고 특정 하위요소 (p, ul태그 등)의 너비를 제한했었다.
```html
<div class="u-containProse">
  <p>...</p>
</div>
<img src="..." alt="...">
<div class="u-containProse">
  <p>...</p>
</div>
```
```css
.u-containProse p,
.u-containProse ul,
.u-containProse ol,
.u-containProse blockquote/*, etc. */ {
  max-width: 40em;
  margin-left: auto;
  margin-right: auto;
}
Code language: CSS (css)

```

하지만 이 기술은 혼란스러울 뿐만 아니라 글 콘텐츠 내에서 `width, margin, float` overide가 예기치 않게 작동할 수 있다.

이 솔루션들의 문제점은 특정한 요소(full-width image) 를 제외한 다른 모든 일반적인 요소(`p`태그 및 기타 일반 플로우 컨텐츠)를 **복잡**하게 만든다는 것이다.

이를 해결하기 위해선 다음과 같은 솔루션을 적용한다.
```css
.u-release {
  margin-left: calc(-50vw + 50%);
  margin-right: calc(-50vw + 50%);
}
```


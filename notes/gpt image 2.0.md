아래 문서는 **OpenAI 공식 Cookbook의 “GPT Image Generation Models Prompting Guide”**를 회사 실무 관점으로 재해석한 분석입니다. 이 가이드는 단순히 “예쁜 이미지 만드는 법”이 아니라, **상품 기획 → 디자인 콘셉트 → 작업지시서 → 시안 생성 → 수정 반복 → 최종 검수**까지 이어지는 **AI 이미지 제작 워크플로우 가이드**에 가깝습니다. 문서 기준일은 **2026년 4월 21일**입니다.  

## **1. 이 문서의 핵심 한 줄 요약**

**이미지 생성 AI는 ‘마법처럼 알아서 그려주는 도구’가 아니라, 좋은 작업지시서를 주면 빠르게 시안을 만들어주는 비주얼 제작 파트너다.**

OpenAI 가이드는 `gpt-image` 모델을 **프로덕션 품질의 이미지 제작, 반복 수정, 정교한 편집, 텍스트 포함 이미지, 브랜드 스타일 제어, 상품 목업, 광고, 인포그래픽, UI 목업** 등에 사용할 수 있다고 설명합니다. 특히 `gpt-image-2`는 고품질 생성·편집, 텍스트 렌더링, 포토리얼리즘, 복합 구성, 스타일 제어, 실제 작업 흐름에 적합한 모델로 소개됩니다.  

회사 사람들에게 설명할 때는 이렇게 말하면 됩니다.

“AI 이미지 프롬프트는 감으로 쓰는 문장이 아니라, 디자이너에게 주는 작업지시서처럼 써야 한다. 목표, 대상, 스타일, 구도, 텍스트, 변경할 것, 유지할 것, 금지할 것을 분명히 적을수록 결과물이 좋아진다.”

---

## **2. 이 가이드에서 가장 중요한 원칙 8가지**

### **1) 프롬프트는 “명령어”가 아니라 “작업지시서”다**

가이드는 프롬프트를 쓸 때 **배경/장면 → 주제 → 핵심 디테일 → 제약조건** 순서로 구조화하라고 말합니다. 또한 광고, UI 목업, 인포그래픽처럼 **최종 사용 목적**을 함께 적어야 모델이 결과물의 완성도와 방향을 맞출 수 있다고 설명합니다. 복잡한 요청은 긴 문단 하나보다 짧은 라벨형 섹션으로 나누는 것이 좋습니다.  

실무적으로는 이렇게 바꾸면 됩니다.

나쁜 요청:

“고급스러운 상품 이미지 만들어줘.”

좋은 요청:

“프리미엄 화장품 상세페이지 상단 배너용 이미지. 흰색 세라믹 용기의 수분크림을 중앙에 배치. 배경은 밝은 베이지 톤의 욕실 선반. 자연광, 깨끗한 그림자, 고급 스킨케어 브랜드 느낌. 제품 라벨은 그대로 유지. 추가 로고, 워터마크, 임의 텍스트 금지.”

핵심은 **무엇을 만들지**보다 **어디에 쓸 이미지인지**를 먼저 알려주는 것입니다.

---

### **2) 구체적으로 쓸수록 좋지만, 불필요하게 장황하면 안 된다**

OpenAI는 소재, 형태, 질감, 매체를 구체적으로 적으라고 합니다. 예를 들어 사진처럼 보이게 하려면 **photorealistic**, 실제 사진, 자연광, 피부 질감, 섬유 마모감, 제품 재질 같은 단어가 도움이 됩니다. 다만 카메라 렌즈나 조리개 같은 세부 스펙은 “정확한 물리 시뮬레이션”보다 전체적인 사진 느낌을 유도하는 용도로 보는 것이 좋다고 설명합니다.  

즉, 회사에서는 이렇게 이해하면 됩니다.

|**잘못된 방식**|**좋은 방식**|
|---|---|
|예쁘게|미니멀하고 프리미엄한|
|고급스럽게|무광 세라믹, 부드러운 자연광, 넓은 여백|
|실사처럼|photorealistic product photography|
|느낌 있게|따뜻한 베이지 톤, 은은한 그림자, 차분한 무드|
|알아서 구성|제품 중앙, 좌측 여백, 우상단 짧은 카피 영역|

---

### **3) “바꿀 것”과 “절대 바꾸면 안 되는 것”을 분리해야 한다**

편집 작업에서 가장 중요한 원칙은 **change only X + keep everything else the same**입니다. OpenAI 가이드는 “무엇을 바꿀지”뿐 아니라 **정체성, 구조, 레이아웃, 브랜드 요소, 주변 사물, 카메라 각도, 명도/채도/대비, 라벨, 화살표**처럼 유지해야 할 요소를 반복해서 명시하라고 합니다. 반복 수정할 때도 보존 조건을 매번 다시 적어야 결과물 드리프트를 줄일 수 있습니다.  

상품 이미지 편집에서는 이게 매우 중요합니다.

예시:

“제품 병의 배경만 흰색으로 변경. 제품의 형태, 비율, 라벨 문구, 로고 위치, 뚜껑 색상, 그림자 방향은 유지. 라벨 텍스트를 수정하거나 새 장식을 추가하지 말 것.”

디자이너에게 설명할 때는 이렇게 말하면 됩니다.

“AI는 수정 요청을 받으면 주변까지 같이 바꾸려는 경향이 있으므로, 유지해야 할 요소를 작업지시서에 계속 박아 넣어야 한다.”

---

### **4) 이미지 안의 텍스트는 반드시 따옴표로 정확히 적어야 한다**

문서에서는 이미지 안에 들어갈 문구를 **따옴표 또는 대문자**로 명확히 쓰고, 폰트 스타일, 크기, 색상, 위치를 제약조건으로 지정하라고 설명합니다. 브랜드명이나 철자가 어려운 단어는 글자 단위로 풀어 쓰는 것도 정확도를 높이는 방법입니다. 작은 글자, 정보가 많은 패널, 여러 폰트가 섞이는 작업은 `medium`이나 `high` 품질을 비교하라고 안내합니다.  

실무 예시:

패키지 전면 문구는 정확히 한 번만 표시:  
“MOISTURE BALM”  
굵은 산세리프, 흰색, 중앙 정렬, 자간은 넓게.  
오탈자, 추가 문자, 임의 슬로건 금지.

이 원칙은 **상세페이지 배너, 패키지 목업, 광고 이미지, 포스터, 카드뉴스, 제품 라벨** 작업에서 필수입니다.

---

### **5) 여러 이미지를 넣을 때는 “이미지 1, 이미지 2”처럼 역할을 지정해야 한다**

가이드는 다중 이미지 입력을 사용할 때 각 이미지를 **번호와 설명으로 참조**하라고 합니다. 예를 들어 “Image 1은 제품 사진, Image 2는 스타일 레퍼런스, Image 3은 배경”처럼 정리하고, 어떤 요소를 어디에 적용할지 명확히 써야 합니다. 합성 작업에서는 어떤 대상을 어디로 옮길지, 어떤 배경·구도·조명은 유지할지까지 지정해야 합니다.  

실무 예시:

Image 1: 실제 제품 사진. 제품 형태와 라벨을 보존해야 함.  
Image 2: 원하는 촬영 무드 레퍼런스. 조명, 색감, 배경 톤만 참고.  
작업: Image 1의 제품을 Image 2 스타일의 프리미엄 스튜디오 배경에 배치. 제품 라벨과 비율은 변경 금지.

이 방식은 **상품 상세페이지, 광고 소재, 패키지 목업, 룩북, 인테리어 시뮬레이션**에 바로 적용할 수 있습니다.

---

### **6) 한 번에 완벽하게 만들려고 하지 말고, 작은 수정으로 반복해야 한다**

OpenAI는 복잡한 프롬프트도 가능하지만, 디버깅이 어렵기 때문에 **깨끗한 기본 프롬프트로 시작하고 작은 단위의 후속 수정**을 권장합니다. 예를 들어 “조명만 더 따뜻하게”, “나무 하나 제거”, “원래 배경 복원”처럼 한 번에 하나씩 수정하는 방식입니다.  

회사 워크플로우로 바꾸면 다음 순서가 좋습니다.

1. **기본 시안 생성**
2. 방향성 선택
3. 조명/색감 수정
4. 구도 수정
5. 텍스트/라벨 수정
6. 제품 보존 검수
7. 최종 후보 3~5개 비교

즉, AI 이미지 작업은 “한 방에 최종본”이 아니라 **초안 → 선택 → 보정 → 검수** 프로세스로 운영해야 합니다.

---

### **7) 품질 설정은 목적에 따라 다르게 써야 한다**

문서에서는 속도와 단가가 중요하면 `quality="low"`부터 시작하고, 작은 텍스트·촘촘한 인포그래픽·인물 얼굴·정체성 보존·고해상도 출력에는 `medium` 또는 `high`를 비교하라고 설명합니다. 또한 `gpt-image-2`는 대부분의 프로덕션 워크플로우에서 기본 선택지로 권장되고, 대량 탐색이나 비용 중심 작업에서는 낮은 품질 설정이나 mini 계열 모델을 고려할 수 있다고 설명합니다.  

회사 기준으로는 이렇게 나누면 됩니다.

|**목적**|**추천 품질**|
|---|---|
|아이디어 탐색, 러프 시안, 콘셉트 후보 많이 뽑기|low 또는 medium|
|상세페이지 대표 이미지|medium 이상|
|패키지 목업, 광고 배너|medium 이상|
|작은 텍스트가 많은 이미지|high|
|인포그래픽, 슬라이드, 차트|high|
|얼굴, 착장, 인물 정체성 보존|medium/high 비교|
|최종 납품 전 검수용|high 후보 생성 후 디자이너 보정|

---

### **8) 해상도는 무조건 크게 뽑는다고 좋은 게 아니다**

`gpt-image-2`는 특정 조건 안에서 다양한 해상도를 지원하지만, 문서에서는 **2K/QHD인 2560×1440을 신뢰성 있는 상한선으로 보고**, 그보다 큰 이미지는 결과 변동성이 커질 수 있으므로 실험적으로 다루라고 설명합니다. 또한 긴 변은 3840px 미만, 양쪽 변은 16의 배수, 장단변 비율은 3:1 이하, 총 픽셀 수 제한 등의 조건이 있습니다.  

실무에서는 이렇게 쓰면 됩니다.

|**용도**|**추천 비율/크기**|
|---|---|
|정사각형 상품 썸네일|1024×1024|
|세로형 상세페이지/카드뉴스/포스터|1024×1536|
|가로형 배너/슬라이드|1536×1024 또는 1536×864|
|발표자료/와이드 시안|2560×1440까지 테스트|
|4K급 최종물|AI 원본만 믿지 말고 후처리/업스케일/디자이너 검수 필요|

---

## **3. 직무별로 어떻게 설명하면 좋은가**

## **A. 상품 제작 기획자에게 설명할 내용**

상품 기획자에게 가장 중요한 메시지는 이겁니다.

“AI에게는 ‘예쁘게’가 아니라 ‘왜 이 이미지를 만드는지’를 알려줘야 한다.”

광고 이미지는 단순 기술 사양보다 **크리에이티브 브리프처럼** 쓰는 것이 좋다고 가이드가 설명합니다. 브랜드, 타깃, 문화적 맥락, 콘셉트, 구도, 정확한 카피를 넣고, 그 안에서 모델이 미감 있는 결정을 하도록 해야 합니다.  

상품 기획자가 프롬프트에 넣어야 할 정보는 다음입니다.

|**항목**|**예시**|
|---|---|
|상품명|비건 수분크림|
|타깃|20~30대 민감성 피부 여성|
|판매 채널|자사몰 상세페이지, 인스타그램 광고|
|핵심 메시지|가볍지만 깊은 보습|
|무드|깨끗함, 신뢰감, 자연 유래|
|금지 요소|과장된 의학적 표현, 병원 느낌, 임의 로고|
|필요한 결과물|썸네일 3종, 상세페이지 배너 2종, 광고 시안 4종|

상품 기획자는 이미지 프롬프트를 쓸 때 **제품 USP와 타깃 감정**을 책임져야 합니다.

---

## **B. 디자이너에게 설명할 내용**

디자이너에게 중요한 메시지는 이겁니다.

“AI는 디자인 감각을 대체한다기보다, 레퍼런스 확장과 초안 생산 속도를 크게 올려주는 도구다.”

UI 목업의 경우 가이드는 “제품이 이미 존재하는 것처럼” 설명하고, 레이아웃·위계·간격·실제 인터페이스 요소에 집중하라고 합니다. 콘셉트 아트처럼 요청하면 실제 출시 가능한 UI보다 스케치처럼 나올 수 있으므로 피해야 한다고 설명합니다.  

디자이너가 프롬프트에 넣어야 할 정보는 다음입니다.

|**항목**|**예시**|
|---|---|
|레이아웃|제품 중앙, 좌측 여백, 상단 카피 영역|
|시각 위계|제품 > 헤드라인 > 서브카피 > 버튼|
|색상|아이보리 배경, 세이지 그린 포인트|
|타이포|모던 산세리프, 큰 헤드라인, 충분한 행간|
|질감|무광 세라믹, 수분감 있는 젤 텍스처|
|조명|부드러운 자연광, 약한 그림자|
|금지|과한 그라데이션, 클립아트, 임의 장식|

디자이너에게는 “AI가 결과물을 만들었더라도 **최종 아트디렉션과 디테일 검수는 사람이 한다**”고 설명하는 게 좋습니다.

---

## **C. 작업지시서 제작자에게 설명할 내용**

작업지시서 제작자에게 가장 중요한 메시지는 이겁니다.

“좋은 AI 프롬프트는 좋은 작업지시서와 거의 같다.”

OpenAI 가이드는 슬라이드, 차트, 다이어그램 같은 생산성 이미지를 만들 때 **정확한 산출물 이름, 캔버스, 정보 위계, 실제 텍스트/데이터, 시각 언어**를 넣으라고 설명합니다. 숫자와 라벨은 프롬프트 안에 직접 넣고, 작은 텍스트·범례·축·각주가 있으면 `quality="high"`를 쓰는 것이 좋다고 합니다.  

작업지시서 제작자는 아래 구조를 표준 템플릿으로 쓰면 됩니다.

```text
[작업 목적]
- 이 이미지는 어디에 쓰이는가?
- 광고 / 상세페이지 / 패키지 / SNS / 내부 기획안 / 제품 목업 중 무엇인가?

[입력 자료]
- 제품 사진:
- 브랜드 가이드:
- 참고 이미지:
- 반드시 유지할 로고/라벨/색상:

[생성할 이미지]
- 이미지 유형:
- 구도:
- 배경:
- 조명:
- 스타일:
- 분위기:

[포함할 텍스트]
- 정확한 문구:
- 위치:
- 폰트 느낌:
- 색상:
- 크기:

[반드시 유지할 것]
- 제품 형태:
- 라벨:
- 로고:
- 색상:
- 카메라 각도:
- 주변 사물:

[변경할 것]
- 배경:
- 조명:
- 계절감:
- 소품:
- 제품 배치:

[금지할 것]
- 워터마크 금지
- 임의 로고 금지
- 추가 텍스트 금지
- 제품 라벨 변형 금지
- 과한 보정 금지

[출력 조건]
- 비율:
- 해상도:
- 품질:
- 후보 개수:
```

이 템플릿을 쓰면 기획자, 디자이너, 외주 작업자, AI 작업자가 같은 언어로 소통할 수 있습니다.

---

## **D. 상품 디자이너에게 설명할 내용**

상품 디자이너에게 가장 중요한 메시지는 이겁니다.

“AI는 상품 자체를 ‘최종 설계’해주는 도구가 아니라, 형태·재질·패키지·연출 가능성을 빠르게 탐색하는 도구다.”

가이드의 상품 목업 예시는 제품 추출, 흰 배경 정리, 라벨 보존, 가장자리 품질, 그림자, 배경 제거 후처리 등을 강조합니다. 특히 제품 카탈로그나 마켓플레이스용 목업에서는 **제품 실루엣, 라벨 선명도, 텍스트 보존**이 중요하며, `gpt-image-2`에서는 투명 배경 최종물이 필요할 경우 우선 불투명 배경으로 생성한 뒤 후처리 배경 제거를 권장한다고 설명합니다.  

또한 굿즈, 피규어, 플러시 키링 같은 머천다이즈 콘셉트는 **초기 아이데이션과 피치 비주얼**에 유용하며, 프리미엄 제품 사진 느낌, 재질, 패키지 인쇄 선명도, 오리지널 디자인, 비침해 조건을 넣는 방식이 제시됩니다.  

상품 디자이너용 프롬프트 핵심 요소는 다음입니다.

|**항목**|**예시**|
|---|---|
|제품 종류|키링, 피규어, 패키지 박스, 카드, 스티커|
|형태|둥근 모서리, 작은 손잡이, 볼륨감 있는 실루엣|
|재질|무광 플라스틱, 실리콘, 금속 링, 종이 패키지|
|사용감|손에 잡히는 크기, 가방에 매달 수 있음|
|생산성|너무 얇은 부품 금지, 복잡한 구조 금지|
|패키지|블리스터 포장, 행택, 투명창 박스|
|브랜드 안정성|원작 캐릭터/상표권 침해 금지, 오리지널 디자인|

---

## **4. 상품 제작 회사에 가장 유용한 사용 사례**

## **1) 상품 대표 이미지 / 상세페이지 이미지**

가이드의 포토리얼 이미지 원칙을 상품 촬영에 적용하면 됩니다. 실제 사진처럼 보이게 하려면 “photorealistic”, 자연광, 실제 재질, 미세한 질감, 과한 보정 금지 등을 넣어야 합니다. OpenAI는 믿을 만한 포토리얼 이미지를 원하면 실제 순간을 촬영하는 것처럼 프롬프트를 쓰고, 피부·섬유·재질의 실제 질감을 명시하라고 설명합니다.  

예시 프롬프트:

```text
Create a photorealistic product photography image for an ecommerce detail page.

Product:
A premium white ceramic moisturizing cream jar with a clean minimal label.

Scene:
Placed on a beige stone bathroom shelf with a small amount of water reflection.
Soft morning natural light from the left side.
Subtle realistic contact shadow.

Composition:
Product centered, enough negative space on the left for Korean headline copy.
Eye-level camera angle, close-up product hero shot.

Style:
Premium skincare brand, clean, calm, trustworthy, natural.

Preserve:
Product shape, label position, lid color, proportions.

Constraints:
No extra logos, no watermark, no random text, no exaggerated glow, no medical claims.
```

---

## **2) 패키지 목업**

패키지 목업에서는 “디자인을 새로 꾸며줘”보다 **라벨 정확도와 인쇄 선명도**가 중요합니다. 문서에서는 제품 목업에서 제품 실루엣, 가장자리 품질, 라벨 가독성, 배경 정리, 리스타일 금지를 강조합니다.  

예시 프롬프트:

```text
Create a premium packaging mockup for a skincare cream.

Package:
Matte ivory paper box with subtle embossed texture.
Front label text must be rendered exactly once:
"DEEP MOISTURE CREAM"

Typography:
Modern sans-serif, centered, dark charcoal, clean kerning.

Scene:
Plain warm gray studio background.
Soft diffused studio lighting.
Subtle contact shadow.

Preserve:
Minimal luxury skincare look.
Clean rectangular package geometry.
Sharp label printing.

Constraints:
Original design only.
No trademarks.
No extra text.
No watermark.
No random logo.
```

---

## **3) 광고 배너 / SNS 소재**

광고 이미지는 단순한 기술 스펙보다 **브랜드 브리프**처럼 써야 합니다. OpenAI 가이드는 광고 생성에서 브랜드, 타깃, 문화, 콘셉트, 구도, 정확한 카피를 포함하고, 이미지 안 텍스트가 필요하면 정확히 따옴표로 적으라고 합니다.  

예시 프롬프트:

```text
Create a polished Instagram ad image for a Korean premium skincare brand.

Brand:
Clean, gentle, dermatology-inspired but not clinical.

Audience:
Women in their 20s and 30s with sensitive skin who prefer simple daily skincare.

Concept:
A calm morning routine image showing the moisturizer as the hero product.

Composition:
Product on the right, soft beige bathroom background, negative space on the left.

Ad copy:
Render the following Korean text exactly once:
"가볍지만 깊은 보습"

Typography:
Clean modern Korean sans-serif, dark beige-gray, large headline, highly legible.

Constraints:
No extra text.
No watermark.
No unrelated logos.
No exaggerated medical or whitening claims.
```

---

## **4) 상품 기획 단계의 굿즈 콘셉트**

가이드에는 수집용 액션 피규어/플러시 키링 같은 머천다이즈 콘셉트 예시가 있습니다. 이 사용 사례는 초기 상품 아이디어를 빠르게 시각화하고, 여러 캐릭터·패키지 변형을 테스트하는 데 적합하다고 설명합니다.  

예시 프롬프트:

```text
Create a product concept image of a cute original plush keychain.

Character:
A small round cloud-shaped character with tiny embroidered eyes,
soft pastel blue cheeks, and a gentle smiling expression.

Product:
Plush keychain with a small silver metal ring attached at the top.
Soft fabric texture, visible stitching, palm-sized proportions.

Packaging:
Displayed in a premium transparent retail pouch with a simple hang tag.

Style:
High-end product photography, soft studio lighting, realistic fabric texture,
clean ecommerce presentation.

Constraints:
Original character only.
No copyrighted characters.
No trademarks.
No watermark.
No extra text except:
"CLOUD BUDDY"
```

---

## **5) 인테리어/공간/제품 배치 시뮬레이션**

문서의 인테리어 “swap” 예시는 실제 공간 사진에서 특정 가구나 장식만 바꾸고, 카메라 각도·조명·그림자·주변 맥락은 유지하는 정밀 편집에 적합하다고 설명합니다.  

상품 회사에서는 매장 진열, 팝업스토어, 전시 부스, 촬영 세트 시뮬레이션에 활용할 수 있습니다.

예시:

```text
In this store display photo, replace ONLY the center display stand with a matte white cylindrical product stand.

Preserve:
Camera angle, store lighting, floor shadows, wall color, surrounding shelves, product spacing.

Change:
Only the center stand material and shape.

Style:
Photorealistic retail display, realistic contact shadows, clean premium presentation.

Do not:
Do not change the products.
Do not add new logos.
Do not alter the background.
Do not change the camera angle.
```

---

## **6) 카드뉴스 / 인포그래픽 / 설명 이미지**

OpenAI는 인포그래픽을 학생, 임원, 고객, 일반 대중 등 특정 독자에게 구조화된 정보를 설명하는 용도로 적합하다고 설명합니다. 설명 포스터, 라벨 다이어그램, 타임라인, 비주얼 위키 등에 쓸 수 있으며, 텍스트가 많거나 밀도 높은 레이아웃은 높은 품질 설정이 권장됩니다.  

상품 회사에서는 다음에 유용합니다.

|**용도**|**예시**|
|---|---|
|상세페이지 설명|성분 구조, 사용 순서, 효과 원리|
|내부 교육자료|제품 제조 공정, 품질 체크리스트|
|고객 안내|사용법, 보관법, 주의사항|
|B2B 제안서|제품 라인업, 시장 포지션, 비교표|

예시 프롬프트:

```text
Create a clean Korean infographic for an ecommerce product detail page.

Topic:
How to use a moisturizing cream in a 3-step skincare routine.

Audience:
Korean customers in their 20s and 30s.

Layout:
Vertical infographic with 3 clear sections.
Each section has a simple icon, short Korean title, and one-line explanation.

Text:
1. "세안 후 피부 정돈"
2. "적당량을 얼굴 전체에 도포"
3. "건조한 부위는 한 번 더 레이어링"

Style:
Clean beauty brand design, white background, soft beige accents,
clear Korean typography, generous spacing.

Constraints:
No tiny text.
No extra claims.
No watermark.
No unrelated icons.
```

---

## **5. 회사 공통 프롬프트 표준 템플릿**

실무에서는 아래 템플릿 하나만 공유해도 충분히 효과가 큽니다.

```text
[1. 목적]
이 이미지는 어디에 사용할 것인가?
예: 상세페이지 대표 이미지 / SNS 광고 / 패키지 목업 / 내부 기획안 / 제품 콘셉트 시안

[2. 대상]
누가 보는 이미지인가?
예: 20대 여성 고객 / B2B 바이어 / 내부 의사결정자 / 디자이너 / 생산업체

[3. 이미지 유형]
어떤 결과물을 원하는가?
예: photorealistic product photography / clean infographic / premium packaging mockup / UI mockup / character concept

[4. 주 피사체]
무엇을 보여줘야 하는가?
예: 제품명, 색상, 형태, 재질, 크기, 주요 부품, 패키지 구조

[5. 장면/배경]
어디에 놓이는가?
예: 흰색 스튜디오, 욕실 선반, 매장 진열대, 책상 위, 야외 자연광

[6. 구도]
어떻게 배치할 것인가?
예: 제품 중앙, 좌측 여백, 상단 카피 영역, 정면, 45도 각도, 클로즈업

[7. 스타일]
어떤 브랜드 느낌인가?
예: 미니멀, 프리미엄, 귀여움, 키치, 자연주의, 테크, 고급 백화점 브랜드

[8. 조명/질감]
어떤 촬영 느낌인가?
예: soft diffused studio lighting, natural morning light, matte ceramic texture, realistic fabric texture

[9. 포함할 텍스트]
이미지 안에 들어갈 문구를 정확히 적는다.
예: "가볍지만 깊은 보습"
오탈자, 추가 문자, 임의 문구 금지.

[10. 반드시 유지할 것]
예: 제품 형태, 라벨, 로고 위치, 색상, 비율, 카메라 각도, 배경 구조

[11. 변경할 것]
예: 배경만 변경, 조명만 변경, 옷만 변경, 패키지 색상만 변경

[12. 금지할 것]
예: 워터마크 금지, 임의 로고 금지, 추가 텍스트 금지, 상표권 침해 금지, 제품 라벨 변형 금지

[13. 출력 조건]
예: 1024x1536, vertical, high quality, 4 variations
```

---

## **6. 실무에서 자주 생기는 문제와 해결법**

|**문제**|**원인**|**해결법**|
|---|---|---|
|제품 라벨이 바뀜|“보존” 조건 부족|Preserve product label exactly 추가|
|임의 문구가 생김|텍스트 금지 조건 부족|No extra text, no watermark 명시|
|고급스럽지 않고 촌스러움|스타일이 추상적|재질, 조명, 색상, 브랜드 레벨 구체화|
|제품 형태가 변형됨|AI가 재해석함|Preserve geometry, proportions, silhouette 명시|
|광고 카피 오탈자|텍스트 지시가 약함|문구를 따옴표로 쓰고 EXACT/verbatim 지정|
|배경만 바꾸고 싶은데 전체가 바뀜|편집 범위 불명확|Change only background, keep everything else same|
|시안마다 캐릭터가 달라짐|기준 이미지 없음|character anchor 생성 후 후속 편집에 참조|
|인포그래픽 글자가 작음|정보 과밀|섹션 줄이기, high 품질, “avoid tiny text” 추가|
|목업이 실제 제품 같지 않음|재질·조명 설명 부족|realistic material, contact shadow, studio lighting 추가|

---

## **7. 주의할 점: 문서 안에서 확인되는 애매한 부분**

문서의 모델 요약 표에서는 `gpt-image-2`의 `input_fidelity`가 **Disabled**이며, 이 모델에서는 작동하지 않는다고 적혀 있습니다. 그런데 뒤쪽 일부 편집 예시 코드에는 `gpt-image-2`와 함께 `input_fidelity="high"`가 사용됩니다.  

따라서 회사에서 개발/API 적용까지 할 경우에는 이렇게 정리하는 게 안전합니다.

“기획·디자인팀은 프롬프트 원칙을 그대로 쓰면 되고, 개발팀은 실제 API 적용 시 최신 API Reference 기준으로 `input_fidelity` 지원 여부를 반드시 테스트해야 한다.”

즉, 프롬프트 전략 자체는 유효하지만, 파라미터는 실제 계정/API 버전에서 검증해야 합니다.

---

## **8. 회사 내부 교육용으로 가장 쉽게 설명하는 버전**

팀원들에게 5분 안에 설명한다면 이렇게 말하면 됩니다.

**첫째, AI 이미지 프롬프트는 작업지시서처럼 써야 한다.**  
“예쁘게”가 아니라 목적, 타깃, 구도, 스타일, 텍스트, 금지사항을 적어야 한다.

**둘째, 바꿀 것과 유지할 것을 반드시 나눠야 한다.**  
상품 이미지에서는 제품 형태, 라벨, 로고, 비율이 바뀌면 안 된다.

**셋째, 텍스트는 따옴표로 정확히 적어야 한다.**  
광고 카피, 패키지 문구, 라벨명은 `"문구"` 형태로 쓰고, 추가 텍스트 금지를 적어야 한다.

**넷째, 한 번에 완성하려 하지 말고 단계별로 수정해야 한다.**  
기본 시안 → 구도 수정 → 조명 수정 → 텍스트 수정 → 최종 검수 순서가 좋다.

**다섯째, 상품 제작에는 특히 목업, 패키지, 광고 시안, 상세페이지, 굿즈 콘셉트에 강하다.**  
단, 최종 인쇄물이나 실제 양산 설계는 디자이너·생산 담당자의 검수가 필요하다.

---

## **9. 최종 결론**

이 OpenAI 프롬프트 가이드의 핵심은 **“AI에게 감성어를 던지지 말고, 전문가용 작업지시서를 줘라”**입니다. 문서는 프롬프트 구조, 명시적 제약, 작은 반복 수정이 현실감·레이아웃·텍스트 정확도·정체성 보존을 제어하는 핵심 수단이라고 결론 내립니다. 또한 인포그래픽, 포토리얼 이미지, UI 목업, 로고, 번역, 스타일 전환, 가상 착장, 합성, 조명 변경 등 생성과 편집 패턴 전반을 실무 워크플로우로 제시합니다.  

회사 적용 관점에서 가장 좋은 운영 방식은 다음입니다.

**기획자는 목적과 메시지를 정리하고, 디자이너는 시각 언어와 보존 조건을 정리하고, 작업지시서 제작자는 이를 표준 템플릿으로 구조화하고, AI는 빠른 시안 생성과 반복 수정에 사용한다. 최종 품질·브랜드·법적 리스크·생산 가능성은 사람이 검수한다.**
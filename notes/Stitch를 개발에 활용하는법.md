# Stitch를 개발에 활용하는법

이 글에서는 Stitch를 “디자인을 실제 웹사이트로 변환하는 도구”로 두고, Vooster·Cursor와 엮어서 퍼스널 블로그 화면을 만드는 흐름 안에서 단계별로 활용합니다.[[gpters](https://www.gpters.org/dev/post/create-personal-blog-vooster-12YzJJPYXmCSDrG)]​

## Stitch가 맡는 역할

- Google이 만든 웹 디자인 도구로, Gemini AI에게 프롬프트를 주면 웹앱/웹페이지 UI를 생성해 줍니다.[[gpters](https://www.gpters.org/dev/post/trial-error-developing-websites-dpL7tF85M5jKVzX)]​[[youtube](https://www.youtube.com/watch?v=vdr2KcGD23g)]​
    
- 이 글의 워크플로우에서는 **Figma/MCP → Stitch(화면 구현) → React 변환 → 배포** 중 “화면을 실제로 만드는 구간”을 담당합니다.[[vooster](https://www.vooster.ai/ko/blog/top-5-vibe-coding-tools)]​
    

## Stitch 프롬프트 준비

- 먼저 Vooster ASK 모드에서 “어떤 블로그를 만들지”를 대화하며 구체화하고, 여기서 정리된 요구사항을 Stitch용 프롬프트로 추려냅니다.[[gpters](https://www.gpters.org/dev/post/create-personal-blog-vooster-12YzJJPYXmCSDrG)]​
    
- 글 구조, 홈/목록/상세 페이지, 네비게이션, 다크 톤 같은 스타일 요구사항 등을 텍스트로 정리해 “Stitch 전용 프롬프트”를 만든 뒤, 나머지 잡다한 파일·프롬프트는 과감히 삭제해 관리 대상을 줄입니다.[[gpters](https://www.gpters.org/dev/post/trial-error-developing-websites-dpL7tF85M5jKVzX)]​
    

## Stitch에서 화면 만들기

- Stitch에 준비한 프롬프트를 입력해 블로그 UI 초안을 생성합니다.[[youtube](https://www.youtube.com/watch?v=vdr2KcGD23g)]​[[gpters](https://www.gpters.org/dev/post/trial-error-developing-websites-dpL7tF85M5jKVzX)]​
    
- 글 작성자는 “다크한 걸 원했는데 다른 느낌이 나왔지만, 결과물이 마음에 들면 그대로 사용한다”는 식으로, 완벽히 이해·통제하려 하기보다 **실행→결과 확인→괜찮으면 채택**의 실무적 접근을 취합니다.[[gpters](https://www.gpters.org/dev/post/create-personal-blog-vooster-12YzJJPYXmCSDrG)]​
    
- 한 번에 모든 화면을 요구하지 않고,
    
    - 1. 홈페이지
            
    - 2. 글 상세 페이지
            
    - 3. 네비게이션 연결
            
    - 4. 스타일링  
            순으로 쪼개서 Stitch에 요청하는 것이 훨씬 수월했다고 정리합니다.[[gpters](https://www.gpters.org/dev/post/trial-error-developing-websites-dpL7tF85M5jKVzX)]​
            

## 데이터/구조를 염두에 둔 설계

- Stitch로 만드는 화면은 나중에 Jekyll 기반 블로그에 연결할 것을 가정하고, 포스트 데이터는 YAML을 기본 형식으로 상정합니다.[[gpters](https://www.gpters.org/dev/post/stitch-reul-iyonghan-web-saiteu-gaebal-sihaengcago-1-dpL7tF85M5jKVzX)]​
    
- XML·JSON·YAML 중에서 사람이 읽기 쉬운 YAML을 예시로 보여주며, 제목·날짜·태그 같은 메타데이터를 어떻게 화면에 매핑할지 Stitch 결과를 보며 점진적으로 맞춥니다.[[gpters](https://www.gpters.org/dev/post/stitch-reul-iyonghan-web-saiteu-gaebal-sihaengcago-1-dpL7tF85M5jKVzX)]​
    

## Stitch 결과를 React/코드로 옮길 때

- Cursor와 함께 사용할 것을 전제로, Stitch에서 만든 화면을 **HTML 그대로 구현하라고 Cursor에게 지시하는 방식**이 안정적이었다고 정리합니다.[[gpters](https://www.gpters.org/dev/post/create-personal-blog-vooster-12YzJJPYXmCSDrG)]​
    
- React 변환 프로세스를 별도 축으로 두고, 폴더 구조·컬러 토큰 등은 “완전한 이해보다는 일단 통일해 두고 나중에 학습”하는 전략을 추천합니다.[[gpters](https://www.gpters.org/dev/post/create-personal-blog-vooster-12YzJJPYXmCSDrG)]​
    
- 이 과정에서 중요한 포인트로
    
    - Cursor가 구현 가능한 컬러코딩(토큰)을 사전에 체크
        
    - 폴더 구조를 먼저 설계해 두고 Stitch·Cursor 결과물을 그 구조에 맞춰 배치
        
    - README 작성과 Git push로 중간 결과를 수시 백업  
        같은 체크리스트를 운영합니다.[[gpters](https://www.gpters.org/dev/post/create-personal-blog-vooster-12YzJJPYXmCSDrG)]​
        

## 글에서 제안하는 Stitch 활용 요령 요약

- Vooster ASK로 니즈를 먼저 정리한 뒤, 그 결과를 Stitch 프롬프트로 재구성한다.[[gpters](https://www.gpters.org/dev/post/create-personal-blog-vooster-12YzJJPYXmCSDrG)]​
    
- 한 방에 완성하려 하지 말고, 홈 → 상세 → 네비 → 스타일 순으로 화면을 쪼개서 만든다.[[gpters](https://www.gpters.org/dev/post/trial-error-developing-websites-dpL7tF85M5jKVzX)]​
    
- 데이터 구조(Jekyll의 YAML 등)를 미리 정해 두고, Stitch 화면 요소와 매핑을 계속 조정한다.[[gpters](https://www.gpters.org/dev/post/stitch-reul-iyonghan-web-saiteu-gaebal-sihaengcago-1-dpL7tF85M5jKVzX)]​
    
- Stitch에서 얻은 HTML을 기준으로 Cursor에 “이 HTML 그대로 구현/React로 변환하라”고 명시적으로 지시해 오류를 줄인다.[[gpters](https://www.gpters.org/dev/post/create-personal-blog-vooster-12YzJJPYXmCSDrG)]​
    
- 컬러코딩·폴더 구조·백업 전략을 사전 체크리스트로 만들어, 매번 같은 패턴으로 반복 가능하게 템플릿화하는 것을 목표로 삼는다.[[gpters](https://www.gpters.org/dev/post/create-personal-blog-vooster-12YzJJPYXmCSDrG)]​
    

현재 우리 프로젝트의 UI/UX가 별로라서 밑의 내용대로 https://wecord.replit.app/ 이 사이트의 여러 페이지들을 전부 탐색해서 UI/UX를 똑같이 가져오고 싶어. (디자인 뿐만 아니라 마우스 호버시 이미지가 살짝 커지는 애니메이션이, 이미지 확대방식, 알림 등 전부 다)

• 사용 스킬: vercel-react-native-skills (현재 Expo/React Native 구조 기준으로 판단)

이 프로젝트 기준 가장 적합한 조합은 Playwright 기반 추출 + Tamagui token화 + 기존 React Native View 교체입니다.

가장 현실적인 기술 선택:

1. Playwright MCP(또는 CDP)로 대상 UI의 DOM + computed styles + animation timing 추출

2. 추출값을 design token으로 변환해 Tamagui/StyleSheet 공통 상수로 관리

3. UI 구현은 기존 로직 파일 유지, Presentational 컴포넌트만 교체 (StyleSheet.create, expo-image, react-native-reanimated)

4. app/test-ui 같은 격리 라우트에서 먼저 복제 후 merge

5. 스크린샷 visual diff(baseline vs 구현)로 픽셀 오차 검증
1. svg 좌석맵에서 좌석 선택 후 새로고침 하거나 다른페이지에 다녀왔을 때 시간 세션이 유지 되지 않음 (개발환경에선 작동했는데 프로덕션 배포 환경에서 작동하지 않음)
	1. 그런데 다시 테스트 해보니 또 이번엔 작동함. 뭔가 불안정한거 같음
2. 결제페이지(booking)에서 결제 수단에서 토스 페이먼츠 UI가 작동안함. 결제 설정이 완료되지 않았습니다. 관리자에게 문의해주세요. 라고 뜸


## 04/15
1. 회원가입 시 이미 있는 이메일일 때 바로 뜨지 않고 추가정보를 입력하는 3번째 스텝 즉 가입완료 버튼을 눌러야 뜸, 그이전에 떠야함

2. 회원가입 시 전화번호 플레이스 홀더가 한국번호 고정임 (제거해야함)


pnpm dev 로그 분석
  3. Experiments                                                                                                           
                                          
  web 쪽에 clientTraceMetadata 활성화 ("use with caution"). Next.js 16의 OTel traceparent 전파 실험. 프로덕션 영향 있는    
  기능이라 토글 출처 확인 권장 (아마 next.config.ts에는 없고 Sentry 래퍼 또는 Next.js 기본값일 가능성).
                                                                                                                           
  4. 라우트 매핑 관찰                                                                                                      
                                               
  총 7개 컨트롤러 × 약 50개 라우트. app.setGlobalPrefix('api/v1') 적용됨.                                                  
                                               
  눈에 띄는 비일관성:                                                                                                      
  - ReservationController {/api/v1} — 컨트롤러 path가 비어 있고 각 핸들러에 경로가 박혀 있음. 그래서 /api/v1/reservations,
  /api/v1/payments/confirm, /api/v1/users/me/reservations가 한 컨트롤러에서 튀어나옴. 동일한 자원을 다루는 라우트가 여러   
  컨트롤러로 갈릴 수 있는 구조.                                                                                         
  - SearchController {/api/v1}, PerformanceController {/api/v1} 도 같은 방식 (비어 있는 base + home/banners, home/hot,     
  home/new가 performance 쪽에 있음).                                                                                  
  - Admin 영역은 5개 컨트롤러로 분산 (AdminPerformanceController, AdminBannerController, AdminBookingController,           
  AdminDashboardController, LocalUploadController) — 이건 적절한 분리.                                          
                                                                                                                           
  5. 보안/미들웨어 스택 (main.ts)              
                                                                                                                           
  순서: helmet → cookieParser → global filters (HttpExceptionFilter, TossPaymentExceptionFilter) → global pipes            
  (ZodValidationPipe) → enableCors(credentials:true, origin: array).                                                       
  - FRONTEND_URL production validation (빈값/non-https 거부) 구현됨.                                                       
  - helmet({ crossOriginResourcePolicy: 'cross-origin' }) — R2 이미지 서빙 고려한 완화.                                    
  - 로그에는 이 스택 자체는 드러나지 않음(정상).                                       
                                                                                                                           
  6. 잠재적 이슈 — 로그에는 안 보이지만 점검 필요                                                                          
                                                                                                                           
  7. Swagger/OpenAPI 미설정: API 규모가 50+ 라우트인데 OpenAPI 스펙 생성 흔적이 main.ts에 없음. 관리자/예매/인증 등 다중   
  도메인이라 API 문서 자동화 부재가 협업·테스트에 불리.                                                                    
  8. LocalUploadController 로드: Admin prefix에 있지만 모듈 로딩에서 dev/prod 분기 여부 미확인. 프로덕션에 로컬 파일시스템 
  업로드 라우트가 실수로 열리면 R2 presigned와 의미 충돌.                                                                  
  9. ConfigHostModule + ConfigModule이 여러 번 로그에 찍힘 (+4ms/+0ms/+0ms/+0ms): isGlobal: true인데 feature 모듈에서
  ConfigModule.forFeature(...) 혹은 중복 import가 있는 신호. 기능적 문제는 없으나 로그 노이즈와 소폭의 초기화 비용.        
  10. TSC(api)가 Nest 기동과 병렬: ⠋ TSC Initializing...가 Nest 시작 로그와 뒤섞여 출력됨. type 체크가 백그라운드에서
  돌아가는 구조라 런타임은 type 오류를 차단하지 못함 (SWC만으로 컴파일). 로컬 안전장치는 >  TSC  Found 0 issues의 개발자   
  확인 + CI 뿐.                                
                                                                                                                           
  11. 종합 평가                                                                                                             
                                               
  기동 로그 자체에 이상 징후는 없음. 두 WARN은 모두 의도된 개발 모드 fallback이고, 프로덕션 hard-fail과 dual-mode 감지     
  패턴으로 안전하게 구현됨. 구조적 개선 여지는 (1) 라우트 소유권 정리 (users/me/reservations가 ReservationController에 있는
   것), (2) OpenAPI 문서화, (3) ConfigModule 다중 로드 정리, (4) LocalUpload의 환경별 가드 확인 정도.  
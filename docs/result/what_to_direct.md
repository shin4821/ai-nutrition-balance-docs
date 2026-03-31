NBAI (Nutrition Balance AI) 기획 문서


1. 프로젝트 개요

프로젝트명: NBAI (Nutrition Balance AI)
목적: AI 기반 식사 분석, 영양 추적, 수분 관리, 약물-식품 상호작용 안전 체크를 제공하는 건강 식단 관리 웹 앱
대상 사용자: 건강 관리에 관심 있는 일반인, 식이 제한이 있는 환자, 피트니스 관심자
슬로건: "당신의 식탁 위 안전 가드"


2. 기술 스택

[프론트엔드]
- 프레임워크: React 18 + Vite
- 언어: JavaScript (JSX)
- 인증: Supabase (Google OAuth)
- API 통신: Axios (JWT 자동 첨부)
- 스타일링: 인라인 CSS + CSS 모듈
- 개발 서버: localhost:5173

[백엔드]
- 프레임워크: FastAPI (Python)
- 서버: Uvicorn
- 언어: Python 3.11+
- 포트: 8000
- 인증: JWT (Supabase)

[AI/LLM]
- 모델: GPT-4o-2024-08-06
- 비전: 이미지 분석 지원 (식사 사진, 약물 사진)
- 구조화 출력: response_format (structured output)

[데이터베이스 및 스토리지]
- DB: Supabase (PostgreSQL)
- 인증: Supabase Auth
- 파일 저장: Supabase Storage (meal-images 버킷)


3. 시스템 아키텍처

사용자 (React 프론트엔드)
  -> Supabase Auth (Google OAuth 로그인)
  -> Axios API Client (JWT 인터셉터)
  -> FastAPI 백엔드 (포트 8000)
       -> Supabase Client (DB + Storage)
       -> OpenAI API Client (GPT-4o)
       -> 분석 모듈: analyzer.py (식사), analyzer_picture.py (이미지), advisor.py (리포트), safety_guard.py (안전 체크), analyzer_water.py (수분), analyzer_medicine.py (약물)
  -> Supabase DB
       -> profiles (사용자 프로필)
       -> meal_logs (식사 기록)
       -> advice_reports (일일 리포트, 캐시)
       -> water_logs (수분 기록 + 피드백)
       -> meal-images (스토리지 버킷)


4. 화면 구성 및 사용자 플로우

4.1 로그인 화면 (LoginPage)

- Google OAuth 로그인 버튼
- 브랜드명 "NuBAI" 표시
- 태그라인: "당신의 식탁 위 안전 가드"
- 로그인 성공 시 JWT 토큰 저장, 메인 대시보드로 이동

4.2 메인 대시보드 (App)

[헤더 영역]
- 날짜 표시 (예: "2026년 3월 13일 목요일")
- 제목: "오늘의 건강 리포트"
- 우측 상단 프로필 아바타 (클릭 시 프로필/로그아웃 메뉴)

[주간 캘린더 (Calendar)]
- 일~토 7열 구성
- 주 이동 버튼 (이전/다음)
- 현재 주가 아닌 경우 "오늘" 버튼 표시
- 날짜별 건강 점수 점(dot) 표시:
  - 초록: 70점 이상
  - 주황: 40~69점
  - 빨강: 40점 미만
  - 회색: 데이터 없음
  - "-": 식사 기록 없음
- 미래 날짜 선택 불가
- 날짜 클릭 시 해당 날짜 데이터 조회

[탭 바]
- "영양 현황" 탭
- "식사 기록" 탭

[영양 현황 탭 내용]
- 건강 점수 카드: 반원 게이지 (0~100점)
- 신호등 경고: 일일 종합 신호등이 초록이 아닌 경우 경고 표시
- 맞춤 조언: AI가 생성한 개인 맞춤 영양 조언
- 영양소 분석: 목표 대비 110% 초과 또는 90% 미만 영양소를 바 차트로 표시
- 다음 식사 추천: 부족 영양소를 보충할 식사 추천
- 칼로리 도넛 차트: 탄/단/지 비율 원형 차트
- 3대 영양소 바: 탄수화물, 단백질, 지방의 TDEE 대비 섭취율
- 기타 영양소: 8가지 미량 영양소의 상태 그리드

[식사 기록 탭 - 하위 탭]
- "식사 기록" 하위 탭:
  - 아코디언 카드 형태로 각 식사 표시
  - 썸네일 이미지 (있는 경우)
  - 음식명 (3개까지 + "외 N개")
  - 칼로리 + 건강 점수
  - 펼치면: 탄/단/지 수치, 신호등, 영양사 코멘트, 삭제 버튼

- "수분 기록" 하위 탭:
  - 8컵 그리드 (클릭 가능한 컵 아이콘)
  - 현재 섭취량: 컵 수, ml, 목표 ml
  - 달성률: 색상으로 구분 (파랑 100% 이상, 초록 50% 이상, 주황 50% 미만)
  - 수분 분석: AI 피드백 텍스트

4.3 식사 입력 모달 (MealInput)

- 하단 플로팅 버튼: "+ 식사 기록"
- 모달 열림 시 두 개 탭:

[텍스트 탭]
- 텍스트 입력란 (예시: "현미밥 한 공기, 된장찌개, 고등어 구이")

[사진 탭]
- 식사 전 사진 (필수)
- 식사 후 사진 (선택, 남은 음식 확인용)
- 인원 수 설정 (1~10명, 기본값 1)

[버튼]
- "식단 체크": Safety Guard 사전 안전성 체크 (DB 저장 안 함)
- "식단 기록": 분석 후 DB에 저장

[식단 체크 결과 팝업 (SafetyPopup)]
- 안전 여부 표시
- 경고 메시지 (위험 시 빨간색)
- 식사 가이드
- 영양제 팁
- 확인 버튼

[식단 기록 결과 (MealResult)]
- 음식 항목 목록
- 영양소 정보
- 영양사 코멘트
- 건강 점수
- 신호등 표시

4.4 수분 입력 모달 (WaterInput)

- 하단 플로팅 버튼: "물 기록"
- 8컵 클릭 가능 그리드
- 현재 섭취량 텍스트: "{컵수} / 8 컵 / {ml} / 1,600 ml"
- 컵 클릭 시 즉시 DB 저장 (500ms 디바운스)
- LLM 분석은 수분 기록 하위 탭 진입 시에만 호출

4.5 프로필 페이지 (ProfilePage)

- 뒤로 가기 버튼 + "프로필 수정" 헤더
- 사용자 정보 (아바타, 이름, 이메일)
- 3개 탭 + 하단 저장 버튼

[기본체격 탭 (BodyInfoForm)]
- 성별: 남성/여성 라디오 버튼
- 나이: 숫자 입력
- 키(cm): 숫자 입력
- 몸무게(kg): 숫자 입력
- 체지방률(%): 숫자 입력 (선택)
- 골격근량(kg): 숫자 입력 (선택)
- 활동 수준: 5단계 라디오 버튼
  - 1.2: 비활동적 (거의 운동 안 함)
  - 1.375: 가벼운 활동 (주 1~3일)
  - 1.55: 보통 활동 (주 3~5일)
  - 1.725: 활발한 활동 (주 6~7일)
  - 1.9: 매우 활발 (매일 고강도)

[질환/약물 탭 (MedicationForm)]
- 질환: 태그 입력 (쉼표/엔터 구분)
- 알레르기: 태그 입력
- 복용 약물:
  - 텍스트로 추가 또는 사진으로 추가
  - 항목별 필드: 이름, 용량, 복용 시간, 메모
  - 수정/삭제 가능
- 영양제:
  - 텍스트로 추가 또는 사진으로 추가
  - 항목별 필드: 이름, 용량, 복용 시간, 1회 영양소 함량, 메모
  - 사진 분석 시 성분표 자동 추출
  - 수정/삭제 가능

[식습관/목표 탭 (DietGoalForm)]
- 건강 목표:
  - 주요 목표 (최대 2개 선택): 체중 감량, 근육량 증가, 체중 유지, 식단 교정, 바디 프로필
  - 부가 목표 (다중 선택): 체지방 감소, 기초대사량 증진, 나트륨 줄이기, 단백질 위주, 규칙적 식사, 수분 섭취
  - 집중 관리 영역 (다중 선택): 장 건강, 피부 개선, 면역력, 혈당 관리, 피로 회복, 집중력, 콜레스테롤

- 식습관:
  - 식단 유형 (드롭다운): 일반, 채식, 비건, 페스코, 키토, 저탄고지
  - 간헐적 단식 여부 (토글)
  - 단식 시간 (예: "16:8", 단식 여부 활성화 시)
  - 하루 평균 식사 횟수 (기본값 3)
  - 문제 식습관 (다중 선택): 야식, 과식/폭식, 음주, 맵고 짠 음식, 단 음식, 불규칙 식사, 빠른 식사
  - 하루 수분 목표(ml) (기본값 2000)

- 생활 패턴:
  - 취침 시간
  - 기상 시간
  - 주간 음주 제한 (잔)


5. 핵심 기능 상세

5.1 식사 분석

입력 방식:
- 텍스트: 사용자가 먹은 음식을 한국어로 입력 (예: "현미밥 한 공기, 소고기 미역국")
- 사진: 식사 전 사진 1장 (필수), 식사 후 사진 (선택), 인원 수 지정

분석 과정:
1. 입력 데이터를 GPT-4o에 전달
2. 구조화된 MealAnalysis 결과 반환
3. DB에 저장 (식단 기록 시에만, 식단 체크 시에는 저장 안 함)
4. 이미지가 있는 경우 Supabase Storage에 업로드, 서명된 URL 반환

분석 결과 항목:
- food_items: 음식별 이름, 중량(g), 칼로리(kcal), 탄수화물(g), 단백질(g), 지방(g)
- nutrients: 전체 영양소 합계 (탄수화물, 단백질, 지방, 식이섬유, 비타민D, 비타민C, 칼슘, 마그네슘, 철분, 나트륨, 칼륨)
- reasoning: AI 영양사 코멘트 (프로필 기반 개인화)
- health_score: 건강 점수 0~100 (균형, 질환 적합도, 목표 정합성, 미량영양소 기준)
- traffic_lights: 음식별 신호등 (초록/노랑/빨강 + 사유)

개인화:
- 프로필이 있으면 질환, 알레르기, 신체 정보, 활동 수준, 건강 목표, 식단 유형을 모두 반영
- 시스템 프롬프트에 프로필 요약을 주입하여 분석

5.2 Safety Guard (식단 체크)

실행 시점: 사용자가 "식단 체크" 버튼 클릭 시

분석 항목:
- 질환-식품 경고:
  - 고혈압 + 나트륨 600mg 초과 -> 혈압 경고
  - 당뇨 + 고탄수화물 -> 혈당 급상승 경고
  - 통풍 + 고퓨린 식품 -> 요산 경고

- 약물-식품 상호작용 (시간 기반):
  - 현재 시간 vs 약물 복용 시간 비교
  - 와파린 + 짙은 녹색 채소 -> 혈액 응고 영향
  - 항고혈압제 + 고칼륨 식품(바나나) -> 고칼륨혈증 위험
  - 지용성 약물 + 저지방 식사 -> 흡수율 저하

- 영양제-식품 타이밍:
  - 다음 영양제 복용 시간 vs 현재 식사 시간
  - 지용성 영양소(오메가3, 비타민D) + 지방 함량 분석
  - 칼슘-철분 간섭 감지
  - 최적 복용 타이밍 추천

결과 항목:
- is_safe: 안전 여부 (경고가 있으면 false)
- warning_message: 구체적 위험 내용 (안전하면 빈 문자열)
- action_guide: 식사 조정 방법 또는 주의사항
- supplement_tip: 영양제 최적 복용 타이밍

5.3 일일 영양 리포트

생성 시점: 사용자가 "영양 현황" 탭을 볼 때

생성 과정:
1. 해당 날짜의 모든 meal_logs 조회
2. 캐시 확인 (advice_reports 테이블에서 같은 meal_count인지)
3. 캐시 유효 -> 캐시된 리포트 반환 (빠름)
4. 새 식사 추가됨 or meal_count 변경 -> 새 리포트 생성:
   - 모든 식사의 영양소 합산
   - 프로필의 영양제 영양소 합산
   - 최종 합계 (식사 + 영양제) 계산
   - GPT-4o에 전달: 식사 요약, TDEE, 목표 권장량, 영양제 목록
   - AdviceReport 생성 후 DB에 캐시

리포트 항목:
- daily_total: 식사만의 영양소 합계
- supplements_total: 영양제만의 영양소 합계
- final_total: 식사 + 영양제 합계
- target_comparison: 일일 목표 대비 각 영양소 비율(%)
- personalized_advice: AI 맞춤 조언 (질환/목표 반영, 3~4줄)
- next_meal_suggestion: 부족 영양소를 채울 구체적 식사 추천
- daily_health_score: 일일 종합 건강 점수 (0~100)
- daily_traffic: 종합 신호등 (초록/노랑/빨강 + 요약)

목표량 계산:
- TDEE = BMR x 활동계수
- BMR: 체지방률이 있으면 Katch-McArdle, 없으면 Mifflin-St Jeor 공식
- 탄수화물 50%, 단백질 20%, 지방 30% 비율
- 미량 영양소: 고정 권장량 (예: 비타민D 15ug, 나트륨 2000mg, 고혈압 시 1500mg)

5.4 수분 추적

기능:
- 플로팅 버튼으로 빠른 기록: 8컵 그리드에서 클릭
- 컵당 200ml, 일일 목표 8컵 (1,600ml)
- 클릭 즉시 DB 저장 (500ms 디바운스, LLM 호출 안 함)
- 수분 기록 탭 진입 시 AI 피드백 호출

AI 피드백:
- 사용자 프로필 (질환 정보) 반영
- 오늘 나트륨 섭취량 반영
- LLM 실패 시 규칙 기반 피드백:
  - 8컵 이상: 충분한 수분 섭취
  - 8컵 미만: 목표까지 N컵 더 필요

저장:
- water_logs 테이블 (user_id, log_date, cups, feedback)
- 사용자/날짜별 고유 제약

5.5 약물/영양제 사진 분석

실행 시점: 프로필 질환/약물 탭에서 카메라 아이콘 클릭

분석 과정:
1. 약물 또는 영양제 사진 촬영/업로드
2. GPT-4o 비전으로 분석
3. 인식된 항목 자동 추출:
   - 이름
   - 용량
   - 분류 (약물 vs 영양제)
   - 복용 시간
   - 메모
   - 1회 영양소 함량 (영양제인 경우)
4. 폼에 자동 입력


6. 데이터 모델

6.1 Nutrients (영양소)
- carbohydrates_g: 탄수화물 (g)
- protein_g: 단백질 (g)
- fat_g: 지방 (g)
- dietary_fiber_g: 식이섬유 (g)
- vitamin_d_ug: 비타민D (ug)
- vitamin_c_mg: 비타민C (mg)
- calcium_mg: 칼슘 (mg)
- magnesium_mg: 마그네슘 (mg)
- iron_mg: 철분 (mg)
- sodium_mg: 나트륨 (mg)
- potassium_mg: 칼륨 (mg)

6.2 FoodItem (음식 항목)
- name: 음식명
- weight_g: 중량 (g)
- calories_kcal: 칼로리 (kcal)
- carbohydrates_g: 탄수화물 (g)
- protein_g: 단백질 (g)
- fat_g: 지방 (g)

6.3 FoodTrafficLight (음식별 신호등)
- food_name: 음식명
- color: 초록/노랑/빨강
- reason: 사유

6.4 MealAnalysis (식사 분석 결과)
- food_items: FoodItem 목록
- nutrients: Nutrients
- reasoning: 영양사 코멘트
- health_score: 건강 점수 (0~100)
- traffic_lights: FoodTrafficLight 목록

6.5 TargetComparison (목표 대비 비율)
- calories_pct, carbohydrates_pct, protein_pct, fat_pct
- dietary_fiber_pct, vitamin_d_pct, vitamin_c_pct
- calcium_pct, magnesium_pct, iron_pct, sodium_pct, potassium_pct

6.6 DailyTrafficSummary (일일 종합 신호등)
- overall_color: 초록/노랑/빨강
- summary: 요약 텍스트

6.7 AdviceReport (일일 리포트)
- daily_total: Nutrients (식사)
- supplements_total: Nutrients (영양제)
- final_total: Nutrients (합계)
- target_comparison: TargetComparison
- personalized_advice: 맞춤 조언
- next_meal_suggestion: 다음 식사 추천
- daily_health_score: 건강 점수 (0~100)
- daily_traffic: DailyTrafficSummary

6.8 SafetyResult (안전 체크 결과)
- is_safe: 안전 여부
- warning_message: 경고 메시지
- action_guide: 식사 가이드
- supplement_tip: 영양제 팁

6.9 WaterAnalysis (수분 분석)
- amount_ml: 섭취량 (ml)
- feedback: 피드백 문자열


7. API 엔드포인트

인증: 모든 엔드포인트 (healthz 제외)에 Authorization: Bearer {jwt_token} 필요

[헬스 체크]
GET /healthz -> { status: "ok" }

[프로필]
GET /profile -> { success, profile }
PUT /profile -> { success } (바디: 전체 프로필 JSON)

[식사 분석]
POST /analyze/text
  - 바디: { meal_text, record_date? (YYYY-MM-DD) }
  - 응답: { success, meal_analysis, message }

POST /analyze/image
  - 폼 데이터: before_image (파일), after_image? (파일), num_people (정수), record_date? (문자열)
  - 응답: { success, meal_analysis, image_url (서명된 URL), message }

[사전 안전 체크]
POST /analyze/pre-meal/text
  - 바디: { meal_text }
  - 응답: { success, meal_analysis, safety (SafetyResult), message }

POST /analyze/pre-meal/image
  - 폼 데이터: before_image (파일), num_people (정수)
  - 응답: { success, meal_analysis, safety, message }

[약물 분석]
POST /analyze/medicine
  - 폼 데이터: image (파일)
  - 응답: { success, items: [{ name, dosage, category, time_of_day, note, nutrients_per_dose }] }

[식사 기록 조회/삭제]
GET /meals/{date} (date: YYYY-MM-DD)
  - 응답: { success, meals: [MealAnalysis + id + image_url], count }

DELETE /meals/{meal_id}
  - 응답: { success }

[리포트]
GET /report/{date} (date: YYYY-MM-DD)
  - 응답: { success, report (AdviceReport), message }

GET /calendar?start_date=YYYY-MM-DD&end_date=YYYY-MM-DD
  - 응답: { success, summary: { "YYYY-MM-DD": { total_calories, meal_count, avg_health_score } } }

[수분]
GET /water/{date} (date: YYYY-MM-DD)
  - 응답: { cups, feedback }

POST /water/{date}
  - 바디: { cups }
  - 응답: { success, cups }

POST /water/{date}/analyze
  - 응답: { cups, feedback (LLM 생성) }


8. 데이터베이스 스키마 (Supabase)

[profiles 테이블]
- id (UUID, PK)
- data (JSONB, 전체 프로필)
- created_at (timestamp)
- updated_at (timestamp)

[meal_logs 테이블]
- id (UUID, PK)
- user_id (UUID, FK -> auth.users)
- food_items (JSONB 배열)
- nutrients (JSONB)
- reasoning (text)
- health_score (int)
- traffic_lights (JSONB 배열)
- image_url (text, 스토리지 경로)
- created_at (timestamp, KST)
- 인덱스: (user_id, created_at)

[advice_reports 테이블]
- id (UUID, PK)
- user_id (UUID, FK)
- report_date (date, YYYY-MM-DD)
- daily_total (JSONB)
- supplements_total (JSONB)
- final_total (JSONB)
- target_comparison (JSONB)
- personalized_advice (text)
- next_meal_suggestion (text)
- daily_health_score (int)
- daily_traffic (JSONB)
- meal_count (int, 캐시 무효화 기준)
- created_at (timestamp)
- 인덱스: (user_id, report_date)

[water_logs 테이블]
- id (UUID, PK)
- user_id (UUID, FK)
- log_date (date, YYYY-MM-DD)
- cups (int)
- feedback (text, LLM 생성)
- created_at (timestamp)
- 고유 제약: (user_id, log_date)

[Supabase Storage]
- 버킷: meal-images
- 경로: meals/{uuid.확장자}
- 접근: 비공개 (요청 시 서명된 URL 생성)


9. 컴포넌트 구조

App.jsx
  LoginPage.jsx (미인증 시)
  ProfilePage.jsx (프로필 탭 선택 시)
    BodyInfoForm.jsx
    MedicationForm.jsx
    DietGoalForm.jsx
  메인 대시보드
    헤더 (날짜, 아바타 드롭다운)
    Calendar.jsx
    탭 바 (영양 현황 / 식사 기록)
    [영양 현황 탭]
      HealthScoreCard.jsx (반원 게이지)
      NutrientAnalysis.jsx (영양소 바 차트)
      NextMealSuggestion.jsx (다음 식사 추천)
    [식사 기록 탭]
      [식사 하위 탭]
        MealList.jsx (아코디언 카드)
      [수분 하위 탭]
        WaterLog.jsx
          WaterTracker.jsx (8컵 그리드)
    플로팅 버튼
      MealInput.jsx (식사 입력 모달)
        MealResult.jsx (결과 표시)
        SafetyPopup (안전 체크 결과)
      WaterInput.jsx (수분 입력 모달)
        WaterTracker.jsx


10. 주요 기준값

[건강 점수 범위]
- 70점 이상: 양호 (초록)
- 40~69점: 보통 (주황)
- 40점 미만: 위험 (빨강)

[영양소 목표 평가]
- 90~110%: 적정 (초록)
- 90% 미만: 부족 (파랑)
- 110% 초과: 과다 (빨강)

[활동 계수]
- 1.2: 비활동적
- 1.375: 가벼운 활동 (주 1~3일)
- 1.55: 보통 활동 (주 3~5일)
- 1.725: 활발한 활동 (주 6~7일)
- 1.9: 매우 활발 (매일 고강도)

[나트륨 목표]
- 일반: 2,000mg/일
- 고혈압: 1,500mg/일

[수분 목표]
- 기본: 8컵/일 = 1,600ml


11. 오류 처리 및 대체 로직

- LLM API 실패:
  - 수분 분석: 규칙 기반 피드백으로 대체
  - 식사 분석: 빈 결과 + 오류 메시지 반환
  - 리포트: 오류 토스트, 식사 추가 기록 유도

- 이미지 업로드 실패:
  - 서명된 URL 미생성
  - 식사 자체는 저장됨 (영양소 분석 완료)

- 프로필 미등록:
  - 기본 TDEE 2,000kcal 사용
  - 개인화 조언 생략
  - 일반 목표량 사용

- 데이터베이스 오류:
  - 일부 필드로 재시도
  - 오류 로깅 + 사용자 알림

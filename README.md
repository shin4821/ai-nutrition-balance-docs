# Nutrition Balance AI (NuBAI)

AI 기반 개인 맞춤형 영양 관리 서비스. 텍스트 또는 사진으로 식단을 기록하면 GPT-4o가 영양소를 분석하고, 사용자 프로필(체형, 질환, 알레르기, 복용 약물/영양제)에 따라 신호등 평가, 안전성 점검, 맞춤 식단 조언을 제공한다.


[![시연 영상](https://img.youtube.com/vi/PWRU-jT84SY/0.jpg)](https://www.youtube.com/watch?v=PWRU-jT84SY)


## 주요 기능

- **텍스트/이미지 식단 분석** -- 자연어 또는 식전/식후 사진으로 음식 인식, 중량 추정, 영양소 산출
- **신호등 평가 (Traffic Light)** -- 음식별 green/yellow/red 등급과 사유 제공
- **Safety Guard** -- 식사 전 안전성 사전 점검 (약물-음식 상호작용, 알레르기 경고)
- **영양 리포트** -- 일일 총 섭취량 vs 권장량 비교, 건강 점수(0-100), 맞춤 조언
- **영양제/약물 인식** -- 사진으로 영양제/약물 분석, 보충 영양소 반영
- **수분 섭취 추적** -- 컵 단위 기록 + 나트륨 연계 피드백
- **주간 캘린더** -- 날짜별 건강 점수 요약, 주 단위 탐색


## 기술 스택

| 영역 | 기술 |
|------|------|
| Backend | FastAPI, Python 3.9+, Pydantic v2 |
| Frontend | React 19, Vite |
| AI | OpenAI GPT-4o (structured output) |
| Database | Supabase (PostgreSQL + Storage + Auth) |
| 인증 | Supabase Auth, JWT (ES256), Google OAuth |


## 프로젝트 구조

```
nutrition-balance-ai/
├── app/
│   ├── main.py               # FastAPI 서버, 라우트 정의
│   ├── analyzer.py           # 식단 분석 (텍스트/이미지)
│   ├── analyzer_picture.py   # 음식 이미지 분석
│   ├── analyzer_water.py     # 수분 섭취 분석
│   ├── analyzer_medicine.py  # 약물/영양제 사진 분석
│   ├── advisor.py            # 일일 영양 리포트 생성
│   ├── safety_guard.py       # 식전 안전성 점검
│   └── db_client.py          # Supabase DB 매니저
├── frontend/
│   ├── src/
│   │   ├── App.jsx           # 루트 컴포넌트
│   │   ├── api.js            # Axios API 클라이언트
│   │   ├── AuthProvider.jsx  # 인증 컨텍스트
│   │   └── components/       # Calendar, MealInput, DailyReport 등
│   ├── package.json
│   └── vite.config.js
├── requirements.txt
└── supabase_rls.sql          # RLS 정책
```


## 설치 및 실행

### Backend

```bash
pip install -r requirements.txt
uvicorn app.main:app --reload    # http://localhost:8000
```

### Frontend

```bash
cd frontend
npm install
npm run dev                      # http://localhost:5173
```


## 환경 변수

### Backend (.env)

```
OPENAI_API_KEY=
SUPABASE_URL=
SUPABASE_KEY=
SUPABASE_SERVICE_KEY=
SUPABASE_JWT_SECRET=
CORS_ORIGINS=http://localhost:5173
```

### Frontend (frontend/.env)

```
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```


## API 엔드포인트

모든 엔드포인트는 `Authorization: Bearer <token>` 헤더가 필요하다.

### 식단 분석

| Method | Endpoint | 설명 |
|--------|----------|------|
| POST | `/analyze/text` | 텍스트 기반 식단 분석 |
| POST | `/analyze/image` | 이미지 기반 식단 분석 (before/after) |
| POST | `/analyze/pre-meal/text` | 식전 안전성 점검 (텍스트) |
| POST | `/analyze/pre-meal/image` | 식전 안전성 점검 (이미지) |
| POST | `/analyze/medicine` | 약물/영양제 사진 분석 |

### 조회

| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | `/meals/{date}` | 특정 날짜 식사 목록 |
| GET | `/report/{date}` | 특정 날짜 영양 리포트 |
| GET | `/report` | 오늘의 영양 리포트 |
| GET | `/calendar?start_date=&end_date=` | 주간 요약 |

### 수분 추적

| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | `/water/{date}` | 수분 섭취량 + 피드백 조회 |
| POST | `/water/{date}` | 수분 섭취량 저장 |
| POST | `/water/{date}/analyze` | 수분 섭취 LLM 분석 |

### 프로필

| Method | Endpoint | 설명 |
|--------|----------|------|
| GET | `/profile` | 사용자 프로필 조회 |
| PUT | `/profile` | 사용자 프로필 저장 |

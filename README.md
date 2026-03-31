<div align="center">

# Nutrition Balance AI (NuBAI)

**AI 기반 개인 맞춤형 영양 관리 서비스**

*"단순 칼로리 기록을 넘어 — 약물-음식 상호작용까지 실시간 점검하는 당신의 식탁 위 안전 가드"*

<br>

[![시연 영상](https://img.youtube.com/vi/PWRU-jT84SY/0.jpg)](https://www.youtube.com/watch?v=PWRU-jT84SY)

▶ **시연 영상 보기**

</div>

---

## 프로젝트 배경

<table>
  <tr>
    <th>항목</th>
    <th>내용</th>
  </tr>
  <tr>
    <td><b>목표</b></td>
    <td>식단 분석을 넘어 사용자의 질환·약물·알레르기를 교차 분석하여 실시간 안전성 점검과 맞춤 영양 조언을 제공하는 서비스</td>
  </tr>
  <tr>
    <td><b>역할</b></td>
    <td>풀스택 설계·구현 (1인)</td>
  </tr>
  <tr>
    <td><b>기간</b></td>
    <td>2026년 3월 3일 ~ 3월 13일 (2주)</td>
  </tr>
  <tr>
    <td><b>환경</b></td>
    <td>FastAPI + React 19 · Supabase · OpenAI GPT-4o · Docker</td>
  </tr>
</table>

<br>

기존 식단 관리 앱은 칼로리 기록과 체중 감량에만 집중하며, **"이 음식이 나에게 안전한가?"** 라는 질문에는 답하지 못합니다. NuBAI는 사용자의 건강 프로필(질환, 알레르기, 복용 약물)을 기반으로 식사 전 안전성을 사전 점검하고, 식사 후에는 영양 균형 리포트와 다음 식사 추천까지 제공하는 서비스입니다.

---

## 핵심 기능 개발 히스토리

### Phase 1 — 텍스트 기반 식단 분석

```
"현미밥, 된장찌개, 고등어구이" → GPT-4o Structured Output → 음식별 영양소 산출 + 신호등 평가
```

자연어로 식단을 입력하면 GPT-4o가 음식별 영양소(칼로리, 탄·단·지, 나트륨 등)를 산출하고, green/yellow/red 신호등 등급을 부여합니다.

---

### Phase 2 — 이미지 기반 식단 분석

GPT-4o Vision API를 활용하여 식전/식후 사진으로 음식을 인식합니다. 식전 사진으로 음식 종류와 중량을 추정하고, 식후 사진이 있으면 잔반량을 반영하여 실제 섭취량을 계산합니다. Pillow로 EXIF 회전 보정, 리사이징, JPEG 압축을 처리합니다.

---

### Phase 3 — 개인 맞춤형 영양 리포트 (Advisor)

일일 총 섭취량 vs 권장량 비교, 건강 점수(0-100), 맞춤 조언을 생성합니다. 사용자 체형 데이터로 TDEE(총 에너지 소비량)를 계산하고, 고혈압 환자에게는 나트륨 목표를 1,500mg으로 자동 조정합니다. **다음 식사 추천**으로 부족 영양소를 보충할 메뉴를 제안합니다.

---

### Phase 4 — Safety Guard: 식전 안전성 점검

**가장 도전적이었던 핵심 기능.** 식사 전 텍스트/이미지로 음식을 입력하면, 사용자의 질환·약물·알레르기 프로필과 교차 분석하여 위험 요소를 사전 경고합니다.

- 약물-음식 상호작용 감지 (예: 와파린 복용 중 녹색 채소 과다 섭취 경고)
- 알레르기 성분 경고
- 영양소 흡수 최적 타이밍 안내 (예: 지용성 비타민은 지방과 함께)

**기술적 해결**: 사용자 건강 데이터를 서버 사이드에서 프롬프트에 주입(Context Injection)하고, Structured Output으로 `safety_warnings`, `action_guide`, `supplement_tip` 필드를 강제하여 일관된 응답을 보장합니다.

---

### Phase 5 — 수분 섭취 추적 & 나트륨 연계

8컵 그리드 UI로 수분 섭취를 기록하고(목표 1,600ml/일), 당일 나트륨 섭취량과 연계하여 AI 피드백을 제공합니다. 비동기 분석으로 즉각적인 피드백을 반환합니다.

---

### Phase 6 — 사용자 프로필 & Google OAuth

Supabase Auth + Google OAuth로 소셜 로그인을 구현하고, 종합 건강 프로필(체형, 질환, 알레르기, 약물, 식이 목표, 생활 패턴)을 관리합니다. JWT(ES256) 토큰 검증으로 모든 API를 보호합니다.

---

### Phase 7 — 약물/영양제 사진 인식

약물/영양제 사진을 촬영하면 GPT-4o Vision이 성분을 분석하고, 보충 영양소를 일일 섭취량에 반영합니다. Supabase Private Storage + Signed URL로 민감한 의약품 이미지를 보호합니다.

---

## AI 활용 개발 방법론

- **프롬프트 히스토리 기반 개발**: `docs/prompt/` 폴더에 날짜별 개발 기록을 문서화하여 AI 세션 간 맥락을 유지
- **Role-Based System Prompt 설계**: AI에 "임상 영양사 & 약사 어드바이저" 페르소나를 부여하여, 일반적인 건강 조언이 아닌 금기 사항 중심의 분석을 유도
- **Structured Output 강제**: 모든 AI 응답에 JSON 스키마를 강제하여 UI 렌더링 안정성과 데이터 일관성을 확보
- **Context Injection 패턴**: 사용자 건강 프로필을 서버에서 프롬프트에 주입하여, 프론트엔드에 민감 데이터를 노출하지 않으면서 개인화된 분석을 수행

---

## 시스템 아키텍처

```
React 19 (Vite)
     │
     ▼
Supabase Auth (Google OAuth + JWT ES256)
     │
     ▼
FastAPI Backend (Python 3.11)
├─ analyzer.py          텍스트 식단 분석
├─ analyzer_picture.py  이미지 식단 분석
├─ safety_guard.py      식전 안전성 점검
├─ advisor.py           영양 리포트 생성
├─ analyzer_water.py    수분 섭취 분석
├─ analyzer_medicine.py 약물/영양제 인식
└─ db_client.py         Supabase DB 매니저
     │
     ▼
OpenAI GPT-4o (Structured Output + Vision)
     │
     ▼
Supabase
├─ PostgreSQL (profiles, meal_logs, advice_reports, water_logs)
└─ Storage (meal-images, Private Bucket + Signed URL)
```

---

## 기술 스택

**Backend**: Python 3.11, FastAPI, Pydantic v2, httpx, Pillow, PyJWT (ES256/HS256)

**Frontend**: React 19, Vite, Axios, @supabase/supabase-js

**AI**: OpenAI GPT-4o (Structured Output, Vision API)

**Database**: Supabase (PostgreSQL + Storage + Auth), Row Level Security (RLS)

**Infrastructure**: Docker

---

## 실행 방법

```bash
# Backend
pip install -r requirements.txt
uvicorn app.main:app --reload    # http://localhost:8000

# Frontend
cd frontend
npm install
npm run dev                      # http://localhost:5173
```

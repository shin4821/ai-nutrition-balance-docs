종합 영양 어드바이저 모듈(advisor.py) 제작해줘.

1. 역할: app/analyzer.py와 app/analyzer_picture.py에서 생성된 MealAnalysis 결과들을 취합하여, 사용자 맞춤형 영양 상태 리포트를 생성하는 app/advisor.py를 제작해줘. 
2. 사용자 프로필 기반 기준 설정:
- data/user_profile.json을 로드하여 Mifflin-St Jeor 공식으로 기초대사량(BMR)을 구하고, 활동계수를 곱해 **하루 권장 칼로리(TDEE)**를 계산해줘. 
- 권장 영양 성분 비율(예: 탄 50%, 단 20%, 지 30%)과 나트륨 권장량(2000mg 미만)을 설정해줘. 
3. 주요 기능 (Generate Report):
- 입력: 오늘 하루 섭취한 MealAnalysis 객체들의 리스트. 
- 계산: 총 영양소 합계 vs 하루 권장량의 비율(%) 계산. 
- 분석: 사용자의 지병(예: 고혈압)이나 건강 목표를 고려하여 GPT-4o가 종합적인 피드백을 생성하도록 해줘. 
4. 결과 포맷 (AdviceReport Pydantic 모델):
- daily_total: 오늘 먹은 총 영양소. 
- target_comparison: 권장량 대비 섭취량 비율 (예: "단백질 80% 달성"). 
- personalized_advice: 사용자의 질환과 목표를 반영한 3~4줄의 핵심 조언. 
- next_meal_suggestion: 부족한 영양소를 채우기 위해 다음 식사 때 추천하는 식재료. 
5. 기술적 요구사항:
- 기존 analyzer.py에 정의된 MealAnalysis, Nutrients 클래스를 재사용할 것. 
- OpenAI의 beta.chat.completions.parse를 사용하여 구조화된 리포트를 반환할 것.
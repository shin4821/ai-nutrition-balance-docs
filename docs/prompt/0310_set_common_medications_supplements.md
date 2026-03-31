"우리 서비스의 복약/영양제 가이드 로직을 정교화하기 위해 user_profile.json 구조와 관련 처리 로직들을 리팩토링해줘. medications와 supplements가 담는 정보는 다르지만, '시간 기반 조언'을 위해 공통 분모를 맞추는 작업이야."

1. 데이터 구조 설계 규칙:

   공통 필드(필수): 두 항목 모두 name, dosage, time_of_day 필드를 반드시 가지도록 해줘. (medications에도 필요하다면 note를, supplements에도 note 필드를 추가해줘.)

   개별 필드(유지): supplements에만 있는 brand, frequency, nutrients_per_dose 정보는 절대 삭제하지 말고 그대로 유지해줘.

2. app/analyzer.py 수정:

   _build_profile_prompt 함수를 수정해서, 약물과 영양제 리스트를 순회할 때 공통 필드(name, dosage, time_of_day)를 먼저 텍스트로 만들고, 개별 필드(note나 nutrients)가 있을 때만 뒤에 덧붙여서 요약하도록 로직을 개선해줘.

3. app/safety_guard.py 고도화:

   _build_medications_detail와 _build_supplements_detail 함수가 위에서 정의한 통합 구조를 에러 없이 읽도록 수정해줘.

   특히 영양제의 nutrients_per_dose에 적힌 지방(fat)이나 칼슘 함량을 LLM이 인지하게 해서, 현재 식단과 대조하여 "지금 지방이 적절하니 오메가3 흡수에 최적입니다" 같은 초정밀 조언을 하도록 system_prompt를 다듬어줘.

4. 최종 확인:

   user_profile.json의 예시 데이터도 이 규칙에 맞게 한 번 더 정리해서 보여줘."
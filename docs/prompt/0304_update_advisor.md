확장된 프로필과 실제 데이터 흐름을 반영한 advisor.py 수정

1. 프로필 경로 수정: calculate_tdee 함수에서 활동 계수를 가져올 때, 새로운 user_profile.json 구조인 profile['activity']['coefficient']를 참조하도록 수정해줘. 
2. TDEE 공식 보강: 기초대사량 계산 시 body_info에 body_fat_percentage(체지방률)가 있다면 이를 활용하는 Katch-McArdle 공식을 선택적으로 사용하거나, 지금의 Mifflin-St Jeor 공식을 유지하되 더 정확한 신체 데이터를 반영해줘. 
3. 프롬프트 강화: _build_profile_prompt 함수가 dietary_habits(야식, 단식 등)와 lifestyle_preferences 정보를 포함하도록 업데이트해서 AI가 더 정교한 생활 습관 조언을 하게 해줘. 
4. 테스트 데이터 정렬: 하단 if __name__ == "__main__":의 sample_meals 데이터를 실제 analyzer.py의 출력 양식과 100% 일치하도록(Pydantic 모델 기반) 깔끔하게 다듬어줘. 
5. 나트륨 기준 동적화: 고혈압 환자의 경우 나트륨 권장량을 일반인(2000mg)보다 낮은 1500mg 등으로 AI가 유연하게 판단하도록 가이드를 줘.

// 4번 참고용 nalyzer.py의 출력 양식 예시
{
"food_items": [
    {
    "name": "계란",
    "weight_g": 100.0
    },
    {
    "name": "사과",
    "weight_g": 100.0
    },
    {
    "name": "잡채",
    "weight_g": 150.0
    },
    {
    "name": "계란말이",
    "weight_g": 120.0
    }
],
"nutrients": {
"carbohydrates_g": 45.0,
"protein_g": 30.0,
"fat_g": 20.0,
"vitamin_d_ug": 4.0,
"vitamin_c_mg": 9.0,
"calcium_mg": 60.0,
"magnesium_mg": 25.0,
"iron_mg": 3.0,
"sodium_mg": 900.0,
"potassium_mg": 500.0
},
"reasoning": "사진 속 음식은 두 명이 나누어 먹었습니다. 사용자의 고혈압을 고려하여 나트륨 섭취를 줄이기 위해 섭취량을 계산했습니다. 주 요리는 잡채와 계란말이로, 여기에 포함된 나트륨이 많음을 유의하세요."
}
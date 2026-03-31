'C:\Workspace\nutrition-balance-ai\app\analyzer.py' 에 영양제(supplements) 합산 로직 추가해줘.

1. 영양제 합산 함수 구현: user_profile.json의 supplements 항목에 있는 nutrients_per_dose를 모두 더해주는 sum_supplements_nutrients(profile) 함수를 만들어줘. 
2. 종합 합산 로직: generate_report 함수 내에서 식단 합계(daily_total)와 영양제 합계를 합쳐서 **final_total**을 계산하도록 수정해줘. 
3. TDEE 비교 대상 업데이트: target_comparison 비율을 계산할 때, 식단만 점검하는 게 아니라 **(식단 + 영양제)**의 총합을 기준으로 권장량 대비 몇 %인지 계산하도록 해줘. 
4. 시스템 프롬프트 보강: AI가 영양제 섭취 사실을 인지하고, "식단은 부족하지만 영양제로 보충 중이다" 혹은 "영양제를 먹어도 특정 영양소가 여전히 부족하다" 같은 입체적인 조언을 하도록 지시를 추가해줘.
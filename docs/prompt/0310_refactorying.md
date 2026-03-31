"현재 safety_guard.py에서 app.analyzer의 _build_profile_prompt를 가져와 사용하고 있는데, 이 함수가 예전의 단순 리스트형 약물 구조로 짜여 있어. 이를 새로운 객체 리스트 구조(name, time_of_day 등)에 맞게 리팩토링해줘.

요청 사항:
1. app/analyzer.py 수정: _build_profile_prompt 함수가 새로운 user_profile.json 구조(객체 형태의 medications 및 supplements)를 에러 없이 읽을 수 있도록 업데이트해줘. 
2. 중복 방지: _build_profile_prompt는 질환, 알레르기 같은 기본 건강 프로필만 요약하도록 하고, 상세한 약물/영양제 스케줄은 safety_guard.py에서 만든 상세 함수들이 담당하게 역할을 나눠줘. 
3. 출력 확인: safety_guard.py의 check_safety 함수 시작 부분에 print(f"DEBUG PROFILE: {profile_text}")를 추가해서, LLM에게 전달되는 최종 프로필 텍스트가 중복 없이 깔끔한지 확인할 수 있게 코드를 다듬어줘."
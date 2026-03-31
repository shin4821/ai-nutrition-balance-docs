analyzer.py에 사용자 프로필 연동 및 로직 고도화

현재 텍스트 기반 식단 분석 모듈인 app/analyzer.py를 app/analyzer_picture.py와 동일하게 사용자 프로필(data/user_profile.json)을 기반으로 분석하도록 수정해줘. 다음 요구사항을 반영해줘:

1. 프로필 로드 및 빌드 로직 추가: >    - data/user_profile.json 파일을 읽어오는 load_user_profile() 함수를 추가해줘. 
- 프로필 정보(성별, 나이, 체격, 지병, 알러지, 건강 목표)를 텍스트로 변환하여 시스템 프롬프트에 삽입하는 _build_profile_prompt() 함수를 구현해줘. 
2. 시스템 프롬프트 업데이트:
- 사용자의 개인 신체 정보를 바탕으로 '한 공기', '한 그릇' 같은 추상적인 표현을 해당 사용자에게 적합한 **개인화된 중량(g)**으로 추론하도록 지시해줘. 
- reasoning 필드에 사용자의 지병(예: 고혈압)이나 건강 목표를 고려한 맞춤형 영양 조언을 반드시 포함하도록 규칙을 세워줘.
3. 분석 함수(analyze_meal) 수정:
- 함수 내부에서 자동으로 프로필을 로드하고, 이를 반영한 시스템 프롬프트를 생성하여 OpenAI API(gpt-4o-2024-08-06)에 전달하도록 수정해줘. 
- 기존에 정의된 MealAnalysis Pydantic 모델과 response_format 구조는 그대로 유지해줘. 
4. 경로 설정: 프로필 파일 경로는 os.path.dirname을 활용해 프로젝트 루트의 data/user_profile.json을 정확히 가리키도록 설정해줘.
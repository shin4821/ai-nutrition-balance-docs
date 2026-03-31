명령어: 식단 이미지 정밀 분석 모듈(analyzer_picture.py) 제작

기존 app/analyzer.py의 텍스트 분석 기능을 확장하여, **이미지 기반의 식단 분석 모듈인 app/analyzer_picture.py**를 제작해줘. 이 모듈은 다음의 요구사항을 반드시 충족해야 해.

1. 기술 스택 및 모델

    모델: OpenAI gpt-4o (Vision 기능을 활용하여 이미지 분석)

    데이터 구조: 기존 analyzer.py에서 정의한 **Pydantic 모델(MealAnalysis, FoodItem, Nutrients)**을 100% 동일하게 사용하여 결과의 일관성을 유지할 것.

2. 입력 파라미터 및 옵션 (핵심 로직)

    필수 입력: before_image_path (식사 전 사진)

    선택 옵션: >1. after_image_path (식사 후 사진): 첨부될 경우, AI는 두 사진을 비교하여 **실제 섭취량(Intake)**을 계산해야 함 (예: 음식을 남겼다면 남긴 만큼 영양소 제외).
    2. num_people (인원수, 기본값 1): 사진 속 음식을 몇 명이 나누어 먹는지 지정.
    3. gender_ratio (성별 구성): 남녀 비율에 따른 섭취량 차이를 분석에 반영.

3. 이미지 분석 가이드 (System Prompt 포함 내용)

    사진 속 숟가락, 젓가락, 컵, 혹은 표준 접시 크기를 참조점으로 삼아 음식의 **중량(g)**을 최대한 정밀하게 추론할 것.

    after_image가 제공되면 잔반의 양을 분석하여 영양소 결과값에서 차감할 것.

    인원수(num_people)가 1명 이상일 경우, 전체 음식 양을 인원수에 맞게 배분하고 성별 특성을 고려하여 **'1인당 평균 섭취량'**을 MealAnalysis 객체로 반환할 것.

4. 코드 구현 상세

    로컬 이미지 파일을 Base64로 인코딩하여 OpenAI API로 전송하는 유틸리티 함수 포함.

    OpenAI의 Structured Outputs (beta.chat.completions.parse) 기능을 사용하여 Pydantic 객체로 즉시 반환할 것.

    analyzer.py와 마찬가지로 .env의 OPENAI_API_KEY를 사용하며, 필요시 프록시 이슈 방지를 위한 httpx.Client() 설정을 반영할 것.
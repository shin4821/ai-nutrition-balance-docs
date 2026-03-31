"이제 식단 분석의 핵심인 app/analyzer.py를 작성할 거야.

1. OpenAI 또는 Anthropic API를 사용하여 사용자가 입력한 식단(자연어)을 분석하는 기능을 만들어줘.

2. 결과값은 반드시 JSON 형태여야 하며, 다음 항목을 포함해야 해:

    - food_items: 각 식재료 명칭 및 추정 중량

    - nutrients: 탄단지 + 비타민D, C + 칼슘, 마그네슘, 철분 + 나트륨, 칼륨

3. Pydantic 라이브러리를 사용해서 이 데이터 구조를 엄격하게 정의(Schema)해줘."
"파이썬 기반의 영양 분석 서비스인 'NBAI' 개발을 시작하려고 해. FastAPI로 백엔드를 구축하고 Streamlit으로 대시보드를 만들 거야.

    1. 프로젝트에 필요한 requirements.txt 파일을 만들어줘.

    2. 사용자의 이름, 신체 정보(키, 몸무게, 나이), 활동 계수, 지병 정보를 담을 data/user_profile.json 샘플을 생성해줘."


그리고 프로젝트 구조는 아래처럼 구상할꺼야.
앞으로는 이 구조를 기반으로 코딩해주면 돼.

nutrition-balance-api/
├── app/
│   ├── main.py          # FastAPI 서버 (진입점)
│   ├── analyzer.py      # 식단 분석 AI 로직
│   ├── advisor.py       # 영양 상호작용 추론 로직
│   └── utils.py         # 공통 유틸리티
├── data/
│   └── user_profile.json # 사용자 프로필
├── dashboard.py         # Streamlit 웹 화면 (프론트)
├── requirements.txt     # 설치 라이브러리 목록
└── README.md
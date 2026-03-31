기술 스택 및 선정 이유

[Backend: FastAPI & Python 3.9+]
선정 이유: 비동기 처리를 통한 고성능 API 서버 구축에 최적화되어 있습니다. 특히 Pydantic v2를 사용하여 AI가 분석한 복잡한 영양 데이터를 엄격한 타입 가드로 검증하고, 프론트엔드에 정확한 구조로 전달하기 위해 선택했습니다.

[Frontend: React 19 & Vite]
선정 이유: 최신 React 19의 효율적인 렌더링 성능과 Vite의 빠른 빌드 속도를 결합하여, 이미지 업로드 및 분석 결과 확인 과정에서 지연 없는 사용자 경험을 제공하고자 했습니다.

[AI: OpenAI GPT-4o (Structured Output)]
선정 이유: 이미지 내 다중 음식 인식 및 복잡한 영양 성분 추출에 있어 최고의 성능을 발휘합니다. 특히 Structured Output 기능을 통해 비정형 식단 사진을 데이터베이스화 가능한 정형 JSON 데이터로 100% 일관되게 변환합니다.

[Database & Auth: Supabase (PostgreSQL, Storage, Auth)]
    선정 이유:

        PostgreSQL: 사용자 프로필과 식단 간의 복잡한 관계형 데이터를 안정적으로 관리합니다.

        Storage: Private Bucket과 Signed URL 기능을 통해 사용자의 민감한 약 봉투 및 식단 사진 보안을 강화했습니다.

        Auth (Google OAuth & JWT): 신뢰할 수 있는 소셜 로그인 체계와 ES256 알고리즘 기반의 JWT 인증으로 보안성과 연동 편의성을 모두 확보했습니다.
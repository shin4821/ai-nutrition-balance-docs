'C:\Workspace\nutrition-balance-ai\app' 하위 파일에서 'load_user_profile()'이 로컬 JSON 파일을 읽었지만, 이제는 헤더로 넘어온 JWT 토큰을 검증하거나 UID를 받아 Supabase에서 데이터를 가져와야 해.
로직을 Supabase DB 연동 방식으로 전환하려고 해.

1. 인증 방식: 프론트엔드(React)에서 구글 로그인을 완료하면 Supabase가 주는 JWT 혹은 User ID를 FastAPI 백엔드로 보낼 거야. 
2. 백엔드(FastAPI) 수정: analyzer.py의 load_user_profile() 함수가 로컬 파일을 읽지 않고, Supabase Python SDK를 사용하여 해당 UID의 profiles 테이블에서 JSON 데이터를 가져오도록 수정해줘. 
3. 프로필 저장 로직: 회원가입 직후 혹은 프로필 수정 시, React에서 입력받은 복잡한 JSON 데이터를 Supabase의 data (JSONB 컬럼)에 그대로 upsert 하는 코드를 작성해줘. 
4. 보안: FastAPI에서 supabase-py 라이브러리를 사용해 토큰 유효성을 검사하는 미들웨어나 의존성 주입 코드를 간단하게 예시로 보여줘.
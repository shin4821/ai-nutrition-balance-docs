현재 파이썬으로 인공지능 영양 분석 서비스의 백엔드 로직을 완성했어. 이제 이 로직들을 외부에서 호출할 수 있도록 FastAPI를 이용해 app/main.py를 작성해줘.

요구사항:
1. 두 가지 분석 모드 제공:
   - 사진 분석 (POST /analyze/image): 유저 ID와 사진 파일을 받아서 'C:\Workspace\nutrition-balance-ai\app\analyzer_picture.py'의 analyze_meal_from_image를 실행. (사진은 Supabase Storage에 먼저 업로드하고 그 URL을 DB에 함께 저장)
   - 텍스트 분석 (POST /analyze/text): 유저 ID와 텍스트(예: "점심으로 돈까스 먹었어")를 받아서 'C:\Workspace\nutrition-balance-ai\app\analyzer.py'의 analyze_meal을 실행. (사진이 없으므로 image_url은 null로 저장)
2. 종합 리포트 생성 (GET /report/{user_id}): 특정 유저의 오늘 식단 로그들을 DB에서 모두 가져와서 advisor.py의 generate_report를 실행하고 결과를 advice_reports 테이블에 저장한 후 반환.
3. 참고 파일:
   - app/analyzer_picture.py: analyze_meal_from_image(path) (async 함수)
   - app/analyzer.py: analyze_meal(text) (async 함수)
   - app/db_client.py: db_manager (저장 및 업로드 메서드 포함)
4. 기타: CORS 설정(모든 허용)과 적절한 에러 처리를 포함해줘.
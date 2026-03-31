지금 막 구글 OAuth를 supabase와 연결해서 기능을 추가했거든.
근데 아직 제대로 작동하지 못하는 것 같아.
아래 두 가지 에러 수정해줘.

1. 현재 로컬 웹사이트 들어가면 "Google로 로그인" 버튼이 생겼고, 누르면 작동을 하거든.
근데 첫 화면 들어가자마자 아래 에러 로그가 떠.

// 첫 화면 로그
[DB ERROR] 로그 조회 실패: {'message': 'invalid input syntax for type uuid: "null"', 'code': '22P02', 'hint': None, 'details': None}
INFO:     127.0.0.1:7962 - "GET /water/null/2026-03-11 HTTP/1.1" 200 OK
INFO:     127.0.0.1:7961 - "GET /meals/null/2026-03-11 HTTP/1.1" 200 OK
[DB ERROR] 로그 조회 실패: {'message': 'invalid input syntax for type uuid: "null"', 'code': '22P02', 'hint': None, 'details': None}
[DB ERROR] 주간 요약 조회 실패: {'message': 'invalid input syntax for type uuid: "null"', 'code': '22P02', 'hint': None, 'details': None}
INFO:     127.0.0.1:7959 - "GET /calendar/null?start_date=2026-03-08&end_date=2026-03-14 HTTP/1.1" 200 OK
INFO:     127.0.0.1:7960 - "GET /report/null/2026-03-11 HTTP/1.1" 200 OK
[DB ERROR] 로그 조회 실패: {'message': 'invalid input syntax for type uuid: "null"', 'code': '22P02', 'hint': None, 'details': None}
INFO:     127.0.0.1:10348 - "GET /meals/null/2026-03-11 HTTP/1.1" 200 OK
[DB ERROR] 로그 조회 실패: {'message': 'invalid input syntax for type uuid: "null"', 'code': '22P02', 'hint': None, 'details': None}
INFO:     127.0.0.1:7967 - "GET /water/null/2026-03-11 HTTP/1.1" 200 OK
[DB ERROR] 주간 요약 조회 실패: {'message': 'invalid input syntax for type uuid: "null"', 'code': '22P02', 'hint': None, 'details': None}

2. frontend 첫 화면의 'Google로 로그인'을 누르면 아래 에러가 떠.

// 에러
액세스 차단됨: 이 앱의 요청이 잘못되었습니다.
400 오류: redirect_uri_mismatch
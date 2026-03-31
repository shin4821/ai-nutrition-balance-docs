"구글 로그인을 완료한 유저가 자신의 건강 프로필을 입력할 수 있는 마이페이지 컴포넌트를 React와 Tailwind CSS로 만들어줘.

1. 구성: 상단에 유저 기본 정보(이름, 이메일)를 보여주고, 그 아래에 3개의 탭(기본체격, 질환/약물, 식습관/목표)을 만들어줘. 
2. 데이터 처리: user_profile JSON 구조와 일치하게 데이터를 수집해야 해. 특히 약물(medications)과 영양제(supplements)는 사용자가 '추가하기' 버튼을 눌러 여러 개를 등록할 수 있는 리스트 형태여야 해.
3. 저장: '저장하기' 버튼을 누르면 Supabase의 profiles 테이블에 있는 data (JSONB) 컬럼에 전체 JSON이 upsert 되도록 handleSave 함수를 짜줘. 
4. UX: 입력이 완료되지 않은 항목이 있어도 저장이 가능해야 해.

추가로 약물과 영양제를 사진 분석으로 등록하는 기능을 포함해야 해.

1. 핵심 기능: '약물 및 영양제' 탭에서 [📷 사진으로 간편 등록] 버튼을 만들어줘. 사진을 업로드하면 백엔드의 /analyze/medicine 엔드포인트 (analyzer_picture.py 로직을 살짝 변형해서 analyzer_medicine.py를 만들어줘) 로 전송되어 결과값을 받아와야 해.
2. user_profile JSON 구조를 유지하되, 사진 분석 결과가 들어오면 medications 또는 supplements 리스트에 객체 형태로 자동 추가되게 해줘.
3. 분석 결과가 완벽하지 않을 수 있으니 사용자가 수동으로 수정하거나 삭제할 수 있는 '카드형 리스트' UI를 제공해줘.

analyzer_medicine.py 로직

1. 사진을 받으면 Vision AI(GPT-4o)를 사용하여 약 봉투/라벨에서 name, dosage, time_of_day, note를 추출하는 함수를 analyzer_medicine.py에 별도로 제안해줘. (영양제의 경우 nutrients_per_dose까지 추출 시도)
2. 약 봉투 분석용 GPT 프롬프트에는 반드시 '글자가 흐릿해도 문맥상 유추하여 가장 가능성 높은 성분명'을 적어줘라는 지시를 포함해줘.
3. 또한 프롬프트에 사용자가 올린 사진이 '처방약'인지 '영양제'인지 AI가 스스로 판단해서 적절한 리스트(medications vs supplements)에 넣어주도록 지시해줘.

컴포넌트 분리: 탭이 많으므로 ProfileTabs.jsx, MedicineCard.jsx, BodyInfoForm.jsx 등으로 파일을 나누어 관리하는게 좋아보인다.

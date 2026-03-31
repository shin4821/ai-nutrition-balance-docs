명령어: analyzer_picture.py 내 이미지 최적화 로직 추가

app/analyzer_picture.py 파일의 encode_image_to_base64 함수를 다음 요구사항에 맞춰 수정해줘:

1. Pillow(PIL) 라이브러리 활용: 이미지를 처리하기 위해 Pillow 라이브러리를 사용하고, 필요한 import 문을 상단에 추가해줘. 
2. 리사이징(Resizing): 원본 이미지가 클 경우 가로 또는 세로 최대 길이를 1024px로 제한하여 비율을 유지하며 줄여줘. 
3. 회전 보정: 스마트폰 촬영 시 발생하는 EXIF 회전 정보를 반영하여 이미지가 정방향으로 전송되도록 처리해줘 (ImageOps.exif_transpose 활용). 
4. 최적화 전송: 이미지를 다시 파일로 저장하지 않고, io.BytesIO를 사용하여 메모리 상에서 JPEG 포맷(품질 85%)으로 압축하여 Base64로 인코딩하도록 짜줘. 
5. RGBA 처리: 만약 투명도가 있는 이미지(PNG 등)일 경우, JPEG 변환 시 오류가 나지 않도록 RGB 모드로 자동 변환하는 로직을 포함해줘.
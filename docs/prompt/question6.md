현재 'C:\Workspace\nutrition-balance-ai\app\analyzer.py', 'C:\Workspace\nutrition-balance-ai\app\analyzer_picture.py' 실행 후 결과값은 아래와 같거든.
근데 아래 결과값에는 각 ingredients에 대한 무게만 나와있는데,
각 ingredients에 대한 칼로리, 탄수화물, 단백질, 지방 역시 전달받도록 수정해줘.



// 결과값
{
"food_items":[
0:{
"name":"김밥"
"weight_g":200
}
1:{
"name":"돈까스"
"weight_g":150
}
2:{
"name":"샐러드"
"weight_g":100
}
3:{
"name":"콘 샐러드"
"weight_g":50
}
4:{
"name":"고구마 스틱"
"weight_g":30
}
]
"nutrients":{
"carbohydrates_g":130
"protein_g":30
"fat_g":40
"dietary_fiber_g":10
"vitamin_d_ug":0
"vitamin_c_mg":15
"calcium_mg":150
"magnesium_mg":80
"iron_mg":4
"sodium_mg":1800
"potassium_mg":600
}
"reasoning":"식사는 다양한 음식으로 구성되어 있으며 나트륨이 비교적 높은 편입니다. 고혈압을 관리 중이므로 소스를 적게 드시거나 나트륨 섭취를 주의하세요. 비타민과 미네랄 섭취는 양호하며, 충분한 식이섬유를 포함하고 있습니다. 체중 감량 및 나트륨 조절에 유의하시고, 샐러드를 통한 비타민 C 섭취가 도움이 됩니다."
}
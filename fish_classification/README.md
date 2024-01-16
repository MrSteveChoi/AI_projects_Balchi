# [2023 제3회 K-water AI 경진대회] 어종(魚種) 식별 및 분류 알고리즘 개발
- 대회기간 : 2023-10-30 ~ 2023-11-30
- Team : 김다현, 최희영, 지경호
- Topic : Object Detection
<br/>
<img src="https://github.com/MrSteveChoi/AI_projects/assets/132117793/4c3dee4d-aae9-41fb-8ac0-3a45a16ff8a6" width=60% height=60%>
<br/>

## 대회주제 <br/>
낙동강 하굿둑 물고기 영상에서 수조 내에 찍힌 물고기의 어종을 식별하고 분류하는 AI 모델을 개발합니다.

## 대회 진행 과정 <br/>
### 데이터 EDA 및 준비
 주어진 Dataset은 아래와 같습니다.
Data set | 개수  
:---: | :---: | 
train | 104,875
test | 44,946
Label 정보 | 5,561
<br/>
train 이미지 내에 총 존재하는 물고기의 수는 총 6,301마리이며, 분포는 아래 그래프와 같습니다.
<img src="https://prod-files-secure.s3.us-west-2.amazonaws.com/b0006364-3a15-40ba-9667-031beba24d29/60c49ae6-ea42-40e0-aaca-ba454124baab/Untitled.png" width=60% height=60%>
<br/>
<img src="!https://prod-files-secure.s3.us-west-2.amazonaws.com/b0006364-3a15-40ba-9667-031beba24d29/41420fde-60ab-4ec6-b905-6dcccc60a4ba/Untitled.png" width=60% height=60%>
EDA 결과를 토대로 label이 존재하는 이미지를 학습 이미지로 사용하되, Class imbalance 문제와 특정 Class에 대해 높은 mAP값이 나오는 문제를 해결하기 위한 Augmentation 방법을 적용하였습니다.

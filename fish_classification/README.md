# [2023 제3회 K-water AI 경진대회] 어종(魚種) 식별 및 분류 알고리즘 개발
- 대회기간 : 2023-10-30 ~ 2023-11-30
- Team : 김다현, 최희영, 지경호
- Topic : Object Detection
<br/>
<img src="https://github.com/MrSteveChoi/AI_projects/assets/132117793/4c3dee4d-aae9-41fb-8ac0-3a45a16ff8a6" width=60% height=60%>
<br/>

## 대회주제 <br/>
낙동강 하굿둑 물고기 영상에서 수조 내에 찍힌 물고기의 어종을 식별하고 분류하는 AI 모델을 개발합니다.
---
## 대회 진행 과정 <br/>
### 1. DATA 준비
Data set | 개수  
:---: | :---: | 
train | 104,875
test | 44,946
Label이 존재하는 data | 5,561
<br/>
<br/>
EDA 결과를 토대로 label이 존재하는 이미지를 학습 이미지로 사용하되, Class imbalance 문제와 특정 Class에 대해 높은 mAP값이 나오는 문제를 해결하기 위한 Augmentation 방법을 적용하였습니다.<br/>
<img src="https://github.com/MrSteveChoi/AI_projects_Balchi/assets/132117793/a0866158-5868-45ce-9100-791377583640" width=70% > <br/>
### 2. 훈련 데이터 명세
<br/>
Num total|TrainSet|ValidSet
:---:|:---:|:---:|
18,752|15,001(80%)|3,751(20%)
<br/>
### 3. 훈련 모델
yolo-v8
Detectron2(fast-RCNN)
<br/>
### 4. 모델 학습
<br/>
### 5. 결과
Metric: SMAPE
Public Score : th(%) / 6.11069
Private Score: th(%) / 5.589
---
### 회고
---
### Refernce

# [2023 제3회 K-water AI 경진대회] 어종(魚種) 식별 및 분류 알고리즘 개발
- 대회기간 : 2023-10-30 ~ 2023-11-30
- Team : 김다현, 최희영, 지경호
- Topic : Object Detection
<br/>
<p align="center">
<img src="https://github.com/MrSteveChoi/AI_projects/assets/132117793/4c3dee4d-aae9-41fb-8ac0-3a45a16ff8a6" width=60% height=60%>
</p>
<br/>

## 대회주제 <br/>
낙동강 하굿둑 물고기 영상에서 수조 내에 찍힌 물고기의 어종을 식별하고 분류하는 AI 모델을 개발합니다. <br/>

---
## 대회 진행 과정 <br/>
### 1. DATA 준비
Data set | 개수  
:---: | :---: | 
train | 104,875
test | 44,946
Label이 존재하는 data | 5,561
<br/>
EDA 결과를 토대로 label이 존재하는 이미지를 학습 이미지로 사용하되, Class imbalance 문제와 특정 Class에 대해 높은 mAP값이 나오는 문제를 해결하기 위한 Augmentation 방법을 적용하였습니다.<br/>
<img src="https://github.com/MrSteveChoi/AI_projects_Balchi/assets/132117793/a0866158-5868-45ce-9100-791377583640" width=70% > <br/>

### 2. 훈련 데이터 명세
Num total | TrainSet | ValidSet
:---: | :---: | :---: |
18,752 | 15,001(80%) | 3,751(20%)
<br/>

### 3. 모델 학습 과정
<img src="https://github.com/MrSteveChoi/AI_projects_Balchi/assets/132117793/ef01b326-9957-449f-95e3-c6b812a2f216" width=70% > <br/>
<br/>

### 4. 결과
Metric: Macro F1 Score <br/>
Public Score : 40th(%) / 0.5030873813 <br/>
Private Score: 44th(%) / 0.5214107843 <br/>
https://aifactory.space/task/2600/leaderboard <br/>

---

### 회고
<br/>

---

### 기술스택
<img src="https://img.shields.io/badge/python-3776AB?style=for-the-badge&logo=python&logoColor=white">
---

### Refernce
- 어종분류를 위한 CNN적용 <br/>
    - https://scienceon.kisti.re.kr/srch/selectPORSrchArticle.do?cn=JAKO201912261948910 <br/>
- 딥러닝을 이용한 어종 판별 시스템 <br/>
    - https://scienceon.kisti.re.kr/srch/selectPORSrchArticle.do?cn=DIKO0015305668  <br/>

### 주관 / 주최
- 주최 : 한국수자원공사(K-water)
- 주관 : 인공지능팩토리


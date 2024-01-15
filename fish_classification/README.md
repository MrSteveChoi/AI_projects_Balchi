# [2023 제3회 K-water AI 경진대회] 어종(魚種) 식별 및 분류 알고리즘 개발
- 대회기간 : 2023-10-30 ~ 2023-11-30
- 주최 : 한국수자원공사(K-water)
- 주관 : 인공지능팩토리
- Team : 김다현, 최희영, 지경호
<br/>
<img src="https://github.com/MrSteveChoi/AI_projects/assets/132117793/4c3dee4d-aae9-41fb-8ac0-3a45a16ff8a6" width=60% height=60%>
<br/>

# 프로젝트의 배경 및 목적

- 낙동강 하굿둑 물고기 영상에서 8가지 어종을 분류하는 AI 모델을 개발한다.
- 수조 내에서 찍힌 물고기의 위치와 종류를 구별해 낸다.

<br>

## 기술 스택

- python
  pytorch
  YOLOv8
  Detectron2
  albumentation

<br>

## 프로젝트 진행 과정

### 데이터 정의 및 준비

- 주어진 Dataset
  - train images : 104,875장 
    test images : 44,946장
  - Label의 정보가 담겨있는 json파일
- Label의 정의
  - category_id : 분류된 어종의 종류
    - 0 : 농어
    - 1 : 배스
    - 2 : 숭어
    - 3 : 강준치
    - 4 : 블루길
    - 5 : 잉어
    - 6 : 붕어
    - 7 : 누치
  - id : json 파일 내의 annotation id
  - image_id : 이미지 파일의 id
- Dataset 준비
  - train data에서 실제 Label 정보가 존재하는 이미지 파일만 분리하여 사용하였습니다.
    - 실제 Label 정보가 존재하는 이미지 파일의 수 : 5,561
    - 총 존재하는 물고기의 수 : 6,301
- Dataset augmentation 및 분리
  - Class imbalance 문제를 최소화하고 특정 Class에 대해 높은 mAP를 보여주는 것에 대해 Augmentation을 적용하였습니다.
    - 한 이미지 안에 여러 Class가 존재하는 multi-label data가 존재하므로 특정 Class만 증강하는것에 어려움이 있었습니다. 따라서, 잘라낸 물고기와 빈 수조 이미지를 alpha(0.5)값으로 blending하여 부족한 특정 Class만 추가하는 Augmentation 기법도 적용하였습니다.
  - 총 18,752장 데이터를 학습용 데이터(Training Set), 검증용 데이터(Validation Set)로 나누었고, 그 비율은 **8:2**로 설정했습니다.
  - 데이터 세트 분리 시, 특정 데이터 세트에 데이터가 편향되는 것을 막기 위해 Stratified Split을 사용했습니다.
  - 한 이미지 안에 여러 label이 존재하는 multi-label dataset이기 때문에 특정 label만 추가하는것에 어려움이 있어.
  - 이미지에서 물고기만 Crop 후 빈 배경 이미지와 합성하여 부족한 특정 label만 추가하여 Stratified Split.
  - **mAP** 
    농어 : 0.38 
    배스 : 0.69 
    숭어 : 0.78 
    강준치 : 0.77 
    블루길 : 0.84 
    잉어 : 0.81 
    붕어 : 0.80 
    누치 : 0.75
  - 전체 2,000개 데이터를 학습용 데이터(Training Set), 검증 및 모델 선택용 데이터(Validation Set)로 나누었고, 그 비율은 8:2로 설정했습니다.
- 전체적인 Train Pipeline Code 구축  

<br>

## 모델 선택의 이유

- YOLOv8
  - 1-stage model의 대표주자이자 YOLO 최신모델.
- fast-RCNN
  - 1-stage 방식에 비해 2-stage 방식이 일반적으로 정확도가 높기 때문에 시도
- 1-stage model과 2-stage model을 앙상블  

<br>

## 모델 학습(Training) 과정

### Image normalization

- 모델의 과적합 방지를 위한 정규화

<br>

### Image Augmentation

- HorizontalFlip 
  VerticalFlip 
  RandomRotate 
  RandomFlip 
  Blur 
  MotionBlur 
  MediamBlur 
  GaussianBlur 
  ToGray 
  RandomBrightnessContrast 
  RandomGamma 
  ImageCompression

<br>

### Loss

- Focal Loss : one-stage detector의 정확도 성능을 개선하기 위하여 고안된 loss로,  one-stage detector가 two-stage detector에 비하여 가지고 있는 문제점인 Class imbalance를 해결하기 위해 사용했습니다.

<br>

## 성능 평가

- 자체 성능평가 : mAP
- 본 대회의 평가에는 Macro F1 Score를 사용.

>  **[평가지표]**
>
> - 본 대회의 평가에는 Macro F1 Score가 사용됩니다.
>
>   - mAP가 아니라 F1이므로 **참가자분들은 불필요한 중복 예측이 없도록 Non-Max Suppression 혹은 그에 준하는 정리 과정을 적용한 결과물을 제출하셔야 합니다.**
>
>     ![img](https://cdn.aifactory.space/images/20231027123529_YWQd.png)
>
>     ![img](https://cdn.aifactory.space/images/20231027123529_oBTw.png)
>
> - F1 Score 계산 방식
>
>   - 위치와 클래스가 정확히 맞으면 TP
>   - 둘 중 하나라도 틀리면 FP
>   - 예측이 만들어지지 않은 레이블은 FN
>
> - Score 산출 과정 설명
>
>   1. 먼저 한 이미지 내의 모든 정답과 모든 예측 사이에 IoU를 계산합니다. <br/>
>   2. 각 예측은 자신과 IoU가 0.1 이상인 정답 가운데 IoU가 가장 큰 정답에 사용합니다.
>     - 하나의 예측이 복수 정답에 할당될 수 없습니다.
>   3. 각 정답은 위 할당 조건을 통과한 하나, 여럿, 또는 0개의 예측이 사용합니다. <br/>
>     - 정답에 하나의 예측이 할당된 경우, 클래스도 맞으면 정답 클래스에 TP, 틀리면 FP로 간주합니다. <br/>
>     - 정답에 여러 예측이 할당될 경우 IoU가 가장 큰 예측 하나를 진짜로 간주, 클래스도 맞으면 정답 클래스에 TP, 틀리면 FP 로 간주합니다. <br/>
>          - 나머지 할당되었으나 IoU가 가장 크지 않은 예측들은 각각의 예측 클래스에 FP로 간주합니다. <br/>
>     - 정답이 있으나 아무 예측도 할당되지 않은 경우 해당 정답 클래스에 FN로 간주합니다. <br/>
>     - 아무 정답도 없는 구역에 생성된 예측은 해당 예측 클래스에 FP로 간주합니다. <br/>
>   4. 위 단계를 통해 최종적으로 Macro F1 score 점수를 계산합니다.

<br>

## 프로젝트 결과

- YOLO와 Detectron2(Fast-RCNN)를 활용해 이미지 내에서 물고기의 위치와 어종을 분류해 낼 수 있었음.
- 40 / 13위  

<br>

## 잘한 점 및 부족했던 점

- YOLO와 Detectron2로 OD가 가능한 Pipeline 구축에 성공한 점.
- train data에서 부족한 label만 추출 후 합성을 통해 추가적인 학습용 이미지를 만들었다는 점.
- 그럼에도 불구하고 예상보다 결과가 좋지 않았음.
- 물고기를 bbox로 crop하는것 보다 SAM등을 활용해서 깔끔하게 segmentation 후에 합성했다면 결과가 더 좋았을 것.
- 접근방법 자체는 참신했으나 좀 더 여러가지 방법을 시도해 봤으면 좋았을 것.

<br>

## 데이터 구조
```
fish
├── YOLO
├── data
│   ├── YOLO
│   │   ├── test
│   │   ├── train
│   │   ├── train_
│   │   └── valid_
│   ├── labels_YOLO
│   │   ├── test
│   │   └── train
│   ├── original
│   │   ├── labels
│   │   ├── test
│   │   └── train
│   └── yolo_train
├── data-preprocess
├── data_convert_tools
│   ├── COCO_train_test
│   └── convert2Yolo
│       ├── __pycache__
│       ├── example
│       │   ├── kitti
│       │   │   ├── images
│       │   │   └── labels
│       │   └── voc
│       │       ├── JPEG
│       │       └── label
│       └── images
├── emsemble
├── models
│   ├── Detectron
│   │   └── 
│   └── YOLO
├── outputs
│   ├── Detectron
│   ├── YOLO
│   │   └── test_01
│   │       └── weights
│   └── test_01
│       └── weights
├── submission
├── wandb
└── yolo_train
    ├── USER
    │   └── mnc
    │       └── fish
    │           └── data
    ├── yolo
    │   ├── results
    │   ├── test
    │   ├── train
    │   └── valid
    └── yolo_config
```




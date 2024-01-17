# [Track 2] 2023 바이오헬스 데이터 경진대회 - 치의학 분야 (일반 부문)
- 대회기간 : 2023-11-20 ~ 2023-11-27
- Team : 김다현, 최희영, 지경호, 양재하
<br/>
<p align="center">
<img src="https://github.com/MrSteveChoi/AI_projects/assets/132117793/8795d664-fc2d-43d0-ac44-8c0863a97bbc" width=50% height=50%>
</p>
<br/>

## 대회 목표 및 과제 <br/>
 - 목표 및 기대효과 : 바이오헬스 데이터와 AI 기술을 활용하여 치의학 분야에서의 위험성 평가와 예측 모델 개발
 - 과제 : 사랑니 이미지와 환자 데이터를 바탕으로 사랑니 발치 시간을 예측하는 모델 개발
---

## 대회 진행 과정 <br/>
### 1. Data set
Data set | 개수  
:---: | :---: | 
train images | 432
test images | 286
meta data | -
<br>
** image 파일 시각화 불가능 ** <br>
 - image 파일은 예상도 <br> 
  - Panorama X-ray에서 하악 사랑니를 crop한 image <br>
  - 사랑니 앞치아와의 관계 및 하치조신경이 포함된 사진일 것으로 예상 <br>
  <img src="https://github.com/MrSteveChoi/AI_projects_Balchi/assets/132117793/727e1870-d8f1-432d-9a0c-c5c7f9305f4a" width=70% > <br>
 - meta data 예시 <br>
  <img src="https://github.com/MrSteveChoi/AI_projects_Balchi/assets/132117793/344550b8-a33a-477c-ada1-aad65826d951" width=70% > <br>

'''
  - filename : 이미지 파일명
  - operator : 수술 집도의 (익명화 처리됨)
  - date : 수술 집행 날짜
  - ID : 환자 고유 정보 (익명화 처리됨)
  - sex : 환자의 성별 (M: 남자, F: 여자)
  - age : 환자의 연령
  - ext_tooth : 발치 대상의 치식 번호 (38: 하악 좌측 사랑니, 48: 하악 우측 사랑니)
  - mmo : 환자가 최대로 입을 벌릴 수 있는 범위를 측정한 값 (mm)
  - height : 환자의 신장 (cm)
  - weight : 환자의 몸무게 (kg)
  - bmi : 환자의 bmi
  - time_min : 수술 소요시간 (타겟 정보, train 데이터만 제공)
'''  ''''''

<br>

### 2. 훈련 데이터 명세
 Image Augmentation을 통해 증강한 총 2,500개 데이터를 학습용 데이터(Training Set), 검증 및 모델 선택용 데이터(Validation Set)로 나누었고, 그 비율은 8:2로 설정했습니다.
Num total | TrainSet | ValidSet
:---: | :---: | :---: |
2,500 | 2,000(80%) | 500(20%)
<br/>
### 3. 모델 학습 과정
<img src="https://github.com/MrSteveChoi/AI_projects_Balchi/assets/132117793/ef01b326-9957-449f-95e3-c6b812a2f216" width=70% > <br/>
<br/>

### 4. 결과
Metric: MAPE <br/>
Public Score : 9th(%) / 0.4546 <br/>
Private Score: 9th(%) / (수정) <br/>
https://aiconnect.kr/competition/detail/233/task/307/leaderboard <br/>

---

### 회고
- 폐쇄 환경에서 Vim을 이용해서 .py파일만으로 학습을 시켜볼 수 있었던 독특한 경험이였음.
- 부족한 train data를 물리적으로 증강하여 학습에 사용.
- 그럼에도 불구하고 예상보다 결과가 좋지 않았음.
- 제출 횟수가 부족했기 때문에 여러가지 해보고싶던 실험을 시도해 보지 못했음.
- 제출 전에 자체적은 평가지표를 만든 후 활용했으면 좀 더 많은 실험을 해볼 수 있지 않았을까 함
- 다른 사람들의 solution에서는 회귀문제를 구간화하여 분류문제로 해결한 팀도 있었는데, 좋은 접근법이였던것 같다.
<br/>

---

## 기술 스택

- python
  pytorch
  albumentation

<br>

## 프로젝트 진행 과정

### 데이터 정의 및 준비

- 주어진 Dataset
  
  - 사랑니 부분이 crop된 구강 파노라마 이미지 (3, 300, 300)
  
    - train images : 432장
      test images : 286장
    - 이미지의 정보가 담겨있는 CSV파일
  
    
  
- 환자 정보 컬럼 명세 (CSV)
  - **filename** : 이미지 파일명
  - **operator** : 수술 집도의 (익명화 처리됨)
  - **date** : 수술 집행 날짜
  - **ID** : 환자 고유 정보 (익명화 처리됨)
  - **sex** : 환자의 성별 (M: 남자, F: 여자)
  - **age** : 환자의 연령
  - **ext_tooth** : 발치 대상의 치식 번호 (38: 하악 좌측 사랑니, 48: 하악 우측 사랑니)
  - **mmo** : 환자가 최대로 입을 벌릴 수 있는 범위를 측정한 값 (mm)
  - **height** : 환자의 신장 (cm)
  - **weight** : 환자의 몸무게 (kg)
  - **bmi** : 환자의 bmi
  - **time_min** : 수술 소요시간 (타겟 정보, train 데이터만 제공)

  

- Dataset 준비
  - train data의 시각화가 불가능하기 때문에 일반적인 구강 파노라마 이미지를 사용해 테스트.
  
- Dataset augmentation 및 분리
  
  - 모든 사랑니는 하악 좌측, 우측 사랑니이므로 vertical flip은 적용하지 않음.
  - meta data를 확인한 결과 좌, 우측 사랑니의 발치시간이 유의미하게 차이나지 않기 때문에 우측 사랑니일 경우 horizontal flip을 적용
  
  - 총 432장 데이터를 학습용 데이터(Training Set), 검증용 데이터(Validation Set)로 나누었고, 그 비율은 **8:2**로 설정했습니다.
  - 부족한 이미지를 물리적으로 증강하여 총 2,500개 데이터를 학습용 데이터(Training Set), 검증 및 모델 선택용 데이터(Validation Set)로 나누었고, 그 비율은 8:2로 설정했습니다.

<br>

## 모델 선택의 이유

- Resnet50
  - 
- EfficientNet_b0-7
  - 
- Vit_base / small
- Swinv2_tiny / small

<br>

## 모델 학습(Training) 과정

### Image Augmentation

- RandomSizedCrop
- OneOf
  - HueSaturationValue
  - RandomBrightnessContrast
- JpegCompression
- OneOf
  - Blur
  - MedianBlur

- HorizontalFlip
- RandomRotate90
- Transpose
- Resize
- Cutout
- Normalize

<br>

### Loss

- L1Loss

<br>

## 성능 평

>  **[평가지표]**
> - Mean Absolute Percentage Error (MAPE) <br/>
>  <img src="https://github.com/MrSteveChoi/AI_projects/assets/132117793/a8e4b510-04f9-421c-8264-7224e6551cc6" width=20% height=20%> <br/>
>  - n : 샘플 수
>  - y : 실제 타겟 값
>  - y_hat : 예측 값 
<br>

## 프로젝트 결과

- EfficientNetB4와 Swin을 사용하여 발치시간을 예측 (회귀)
- 9위  


<br>

## 데이터 구조
```
${PROJECT}
├── config/
│   ├── train_config.yaml
│   ├── predict_config.yaml
├── models/
│   ├── effnet.py
│   └── utils.py
├── modules/
│   ├── datasets.py
│   ├── earlystoppers.py
│   ├── losses.py
│   ├── metrics.py
│   ├── optimizers.py
│   ├── recorders.py
│   ├── trainer.py
│   └── utils.py
├── README.md
├── train.py
└── predict.py
```



### Vim 환경설정

- vim plugin 설치 할 때 참고한 사이트
  - [Vim plugin 관리를 위한 번들(Vundle)](https://github.com/VundleVim/Vundle.vim)
  
  - [Jedi for vim autocompletion](https://vimawesome.com/plugin/jedi-vim)
  
  - [Flake8 for vim syntax & style checker](https://vimawesome.com/plugin/vim-flake8)
  <br>

1. Vundle 설치아래 명령어 실행

```
git clonehttps://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
<br>

2. `~/.vimrc`에 아래 탬플릿 작성(큰따옴표로 주석처리)

```
set nocompatible              " be iMproved, required 
filetype off                  " required 

set rtp+=~/.vim/bundle/Vundle.vim 
call vundle#begin() 

" 여기에 플러그인 설치 사용법에 따라 추가작성 

call vundle#end()            " required 
filetype plugin indent on    " required
```
<br>

3. ```~/.vimrc``` 파일에 다음 명령어 추가

```
Plugin 'davidhalter/jedi-vim'
Plugin 'nvie/vim-flake8'
```
<br>
4. vim에서 다음 명령어 실행

```
:source %
:PluginInstall
```

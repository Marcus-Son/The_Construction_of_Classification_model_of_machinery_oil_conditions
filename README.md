# Classification of construction machinery oil conditions by using unbalanced data sampling techniques and knowledge distillation 

## 배경

모든 기계장치에는 윤활유가 필요하다. 공장에서 사용하는 생산설비는 물론이고, 자동차, 기차, 선박, 항공기 같은 교통수단 역시 수많은 부품과 기계장치로 이뤄져 있기 때문에 곳곳에 윤활유가 사용된다. 기계장치에 윤활유를 이용하면 기계가 타버리거나 마모되는 것을 방지할 수 있다. 그래서 품질이 좋은 윤활유를 쓸수록 기계가 마모되는 것을 방지하여 기계의 수명을 더 늘릴 수 있고 반대로 품질이 안 좋은 윤활유를 쓰면 기계의 수명이 단축될 수 있다. 따라서 품질이 안 좋은 윤활유는 적절히 교체를 해주어야된다. 
앞서 언급한 바와 같이 품질이 안 좋은 윤활유는 교체를  해주어야하기 때문에 윤활유의 품질이 불량인 것을 잘 감지할 수 있는 윤활유 품질 분류 모델을 만들어야 한다.

## 주제

건설장비에서 윤활유의 상태를 실시간으로 모니터링하기 위한 윤활유 품질 판단 모델 개발

## 데이터 설명 

모델 학습시에는 주어진 모든 ID 변수를 제외한 모든 feature 53개를 다 이용할 수 있지만 진단 테스트시에는  ID 변수를 제외한 일부 18개의 feature만 사용이 가능하다. 
본 데이콘(Dacon) 대회에서는 test data의 Y_LABEL 변수를 제공할 수 없으므로 train 데이터를 train data, validation data, test data로 6.4:1.6:2로 쪼개서 사용하였다.
데이터 클래스에서는 윤활유의 품질이 불량인 경우와 윤활유의 품질이 좋은 경우의 비가 8.54:91.46으로 Class Imbalance(클래스 불균형)이 심하다. 

## 실험방법

데이터의 클래스의 불균형이 심해 Class weight와 4가지 Sampling 기법( Smote, Smote-Tomek, ADASYN, Randomundersampling )을 적용하였다. 이 대회에서는 특이한 점이 Train data와 
Test data의 변수의 수가 다르므로 이를 해소하기 위해 Knowledge Distillation을 적용하였다.


## 과정

### Step1 Data Load 및 간단한 전처리
  + train.csv 데이터만을 사용해서 진행
  + 문자열 데이터인 ID 및 Component_ARBITRARY 변수 제거
  + Year 변수 제거( Year 변수는 '진단년도' 뜻하는 변수이고 진단년도가 윤활유의 품질에 영향을 미치지 않기 때문에 제거하기로 정했다. )
  + 결측치가 15%가 넘는 열 제거
  + Train data를 6.4 : 1.6 : 2로 train data, validation data, test data로 나누기
  + train data에는 test data 변수 16개( ID, Component_ARBITRARY, Year 변수 제외 )와  윤활유의 품질을 잘 분류할 수 있는  AL(알루미늄 함유량)변수를 제외하고 나머지 변수들은 삭제 
    하기 
  + 데이터들이 정규분포 모양이 아니기 때문에 StandardScaler 보다는 MinMaxScaler를 이용하기
   
### Step2 다음 표와 같이 Class weight나 Sampling 기법을 이용하여 Class Imbalance 문제를 해소하기
 
  ![image](https://user-images.githubusercontent.com/65749318/210593094-6578d579-9b20-40b4-9109-7b7aa70cc717.png)

### Step3  Teacher Model인 LightGBMClassifier 모형을 만들기
 
  + 각 Case들을 비교하기 위해 LightGBMClassifier 모델의 파라미터를 같게 하기
 
### Step4  Student Model인 LightGBMRegressor 모형을 만들기

  + 각 Case들을 비교하기 위해 LightGBMRegressor 모델의 파라미터를 같게 하기

### 각 Case 별 모델들의 Macro F-1 Score 값을 비교하기


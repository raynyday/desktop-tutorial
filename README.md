# 스마트 웨어러블 심전도 기기를 활용한 딥러닝 기반의 수면 단계 분류 기술 개발 

---
## 데이터

1 epoch = 30 sec x sampling rate
ECG sampling rate = 256Hz
ACC, GYRO sampling rate = 50Hz
American Academy of Sleep Medicine(AASM) 기준을 적용하여 Wake-Light Sleep(N1+N2)-Deep Sleep(N3)-REM의 4-class 분류로 진행했다.

### Step 1 : Data preprocessing

1) 장비 착용시간과 수면 시작 시간 간의 차이가 존재하여 이를 맞추는 TimeSync와 불필요한 data를 제거했다. 
2) multi-channel의 신호를 사용하기에, ECG와 ACC, GYRO의 sampling rate를 맞추기 위한 resampling 과정을 거쳤다. → 본 연구에서는 512Hz로 Up-sampling을 진행했다.
3) 수면 구조 특성상 수면 단계의 수가 불평등한 class imbalance problem(CIP)가 존재해 Synthetic Minority Oversampling Technique(SMOTE)의 strategy 중 minority를 적용했다.

### Step 2 : Training CNN
[DatasetA]
train data shape : (15139, 7, 15360)


train label shape : (15139,)


test data shape : (6489, 7, 15360)


test label shape : (6489,)



train 및 test는 hold-out approach로 진행했다. 


![image](https://github.com/raynyday/desktop-tutorial/assets/133008226/b9df08b9-26d5-4a46-8ab6-428b9fad7ac8)


그림 1. input data


![image](https://github.com/raynyday/desktop-tutorial/assets/133008226/6ddd422b-aa3e-4edd-8a28-26509b591c25)
그림 2. Overview of the overall process of proposed approach (single ECG)

### kernel size 설정
ECG 신호 특성 상 하나의 peak가 0.05sec라는 짧은 시간동안 발생한다. 이를 감지하여 수면단계를 분류하기 위해서 kernel size를 (3,20)으로 진행했을때, 0.04sec window가 생성되어 peak를 탐지할 수 있다. 


### Step 3 : Testing CNN
[DatasetB-A]
data shape : (7, 15360, 884)


data label : (884,)


---
## Requirments


---


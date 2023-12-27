# 스마트 웨어러블 심전도 기기를 활용한 딥러닝 기반의 수면 단계 분류 기술 개발 

----

수면은 현대 사회에서 수면은 건강과 삶의 질에 중대한 영향을 미친다. 학업, 가사일, 인간관계, 과도한 업무 등으로 인해 많은 현대인들은 스트레스를 받으며 살아가고 있으며, 이는 현대인들의 수면 부족으로 이어지고 있다. 수면 부족은 교감 뇌 네트워크도 덜 활동적으로 만들뿐더러 더 반사회적으로 만든다는 연구결과가 나왔다. 


수면다원검사를 통해 수면 단계를 분석하는 것은 일반적이지만, 비용과 시간 등의 제약으로 어려움이 있어 웨어러블 디바이스와 같은 생체신호를 활용한 수면 단계 분류 연구가 활발히 진행되고 있다. 2023년 상반기에 수면에 대한 관심이 전년 대비 15% 증가하며, 이는 COVID-19 이후 자기 관리와 수면 케어에 대한 수요 상승을 반영한다. 

이 연구에서는 single Electrocardiogram(ECG)와 ECG, Accelerometer(ACC), Gyroscope(GYRO)의 세 신호를 사용해 자동으로 수면 단계 분류를 수행할 수 있는 딥러닝 모델을 개발했다.
single ECG를 사용했을 때 기존 연구들에서 성능 저하의 문제점이 발생하여 ACC와 GYRO를 함께 사용하였을 때, 성능 향상의 효과가 있는지 확인하고자 했다.

---
## 데이터

1 epoch = 30 sec x sampling rate
ECG sampling rate = 256Hz
ACC, GYRO sampling rate = 50Hz
American Academy of Sleep Medicine(AASM) 기준을 적용하여 Wake-Light Sleep(N1+N2)-Deep Sleep(N3)-REM의 4-class 분류로 진행했다.

### 전처리

1. 장비 착용시간과 수면 시작 시간 간의 차이가 존재하여 이를 맞추는 TimeSync와 불필요한 data를 제거했다.
2. multi-channel의 신호를 사용하기에, ECG와 ACC, GYRO의 sampling rate를 맞추기 위한 resampling 과정을 거쳤다. → 본 연구에서는 512Hz로 Up-sampling을 진행했다.
3. 수면 구조 특성상 수면 단계의 수가 불평등한 class imbalance problem(CIP)가 존재해 Synthetic Minority Oversampling Technique(SMOTE)의 strategy 중 minority를 적용했다.

![image](https://github.com/raynyday/desktop-tutorial/assets/133008226/b9df08b9-26d5-4a46-8ab6-428b9fad7ac8)


그림 1. input data

---
## 모델 설명

![image](https://github.com/raynyday/desktop-tutorial/assets/133008226/6ddd422b-aa3e-4edd-8a28-26509b591c25)
그림 2. Overview of the overall process of proposed approach (single ECG)

### kernel size 설정
ECG 신호 특성 상 하나의 peak가 0.05sec라는 짧은 시간동안 발생한다. 이를 감지하여 수면단계를 분류하기 위해서 kernel size를 (3,20)으로 진행했을때, 0.04sec window가 생성되어 peak를 탐지할 수 있다. 

---
## 훈련 과정

Train:Test = 7:3으로 나눠 hold-out approach로 진행했다.

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
## Version
absl-py                      1.4.0
apptools                     5.2.1
astropy                      5.2.2
astunparse                   1.6.3
audioread                    3.0.1
bidict                       0.22.1
biosppy                      1.0.0
cachetools                   5.3.1
certifi                      2023.7.22
cffi                         1.16.0
charset-normalizer           3.2.0
configobj                    5.0.8
contourpy                    1.1.0
cycler                       0.11.0
DateTime                     5.2
decorator                    5.1.1
dtw                          1.4.0
envisage                     7.0.3
et-xmlfile                   1.1.0
filelock                     3.12.3
flatbuffers                  23.5.26
fonttools                    4.42.1
future                       0.18.3
gast                         0.4.0
google-auth                  2.22.0
google-auth-oauthlib         1.0.0
google-pasta                 0.2.0
grpcio                       1.58.0
h5py                         3.9.0
hrv-analysis                 1.0.4
idna                         3.4
imbalanced-learn             0.11.0
imblearn                     0.0
importlib-metadata           6.8.0
importlib-resources          6.0.1
Jinja2                       3.1.2
joblib                       1.3.2
keras                        2.13.1
kiwisolver                   1.4.5
lazy_loader                  0.3
libclang                     16.0.6
librosa                      0.10.1
llvmlite                     0.41.1
lxml                         4.9.3
Markdown                     3.4.4
MarkupSafe                   2.1.3
matplotlib                   3.7.2
mayavi                       4.8.1
mlxtend                      0.23.0
mpmath                       1.3.0
msgpack                      1.0.7
networkx                     3.1
neurokit2                    0.1.4.1
nolds                        0.5.2
numba                        0.58.1
numpy                        1.24.3
oauthlib                     3.2.2
opencv-python                4.7.0.72
openpyxl                     3.1.2
opt-einsum                   3.3.0
packaging                    23.1
pandas                       2.0.3
pandas-datareader            0.10.0
Pillow                       10.0.0
pip                          23.2.1
platformdirs                 4.0.0
pooch                        1.8.0
protobuf                     4.24.3
pyasn1                       0.5.0
pyasn1-modules               0.3.0
pycparser                    2.21
pyerfa                       2.0.0.3
pyface                       8.0.0
Pygments                     2.16.1
pyparsing                    3.0.9
PyQt5                        5.15.10
PyQt5-Qt5                    5.15.2
PyQt5-sip                    12.13.0
python-dateutil              2.8.2
pytils                       0.4.1
pytz                         2023.3.post1
PyYAML                       6.0.1
requests                     2.31.0
requests-oauthlib            1.3.1
resampy                      0.4.2
rsa                          4.9
scikit-learn                 1.3.2
scipy                        1.10.1
seaborn                      0.12.2
setuptools                   68.0.0
shortuuid                    1.0.11
six                          1.16.0
smote                        0.1
soundfile                    0.12.1
soxr                         0.3.7
spicy                        0.16.0
sympy                        1.12
tensorboard                  2.13.0
tensorboard-data-server      0.7.1
tensorflow                   2.13.0
tensorflow-estimator         2.13.0
tensorflow-intel             2.13.0
tensorflow-io-gcs-filesystem 0.31.0
termcolor                    2.3.0
threadpoolctl                3.2.0
tools                        0.1.9
torch                        2.0.1
torchvision                  0.15.2
traits                       6.4.3
traitsui                     8.0.0
typing_extensions            4.5.0
tzdata                       2023.3
urllib3                      1.26.16
vtk                          9.2.6
Werkzeug                     2.3.7
wfdb                         3.4.1
wheel                        0.38.4
wrapt                        1.15.0
zipp                         3.16.2
zope.interface               6.0
---


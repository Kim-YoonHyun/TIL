# 1. 개요

## 1.1. 모듈 설명

dicom 파일을 파이썬에서 다루기 위한 모듈.

## 1.2. dicom 상세설명

- dicom 파일은 pixel와 header 로 이루어져 있다.

  pixel: 이미지 구성 값.

  header: 이미지에 대한 각종 정보(환자 정보, 비트수, 모달리티 등)

### 1.2.1. pixel 정보

pixel image array 는 (channel, width, height) 를 이루고 있다.

- channel: 시간 축, CT 이미지의 한 프레임으로 볼 수 있음. channel 값이 없는 경우 이미지는 한 장이라는 의미.

### 1.2.2. 주요 header 정보

#### Bits Stored

dicom 이미지가 저장된 비트 수 = dicom 파일을 이루고 있는 값의 범위.

#### Rescale Slope, Rescale Intercept

dicom 파일에 저장된 값을 전처리하기 위해 필요한 정보.
이미지를 저장하는 과정에서 실제 값과 저장값 같의 차이가 존재하게 되며 저장값에서 실제 값을 계산하기 위한 보정값으로 사용됨.

Rescale slope: 기울기와 비슷한 개념

Rescale Intercept : offset(절편)과 비슷한 개념

보정 계산식: `Rescale slope * pixel + Rescale Intercept`

#### Window Center, Window Width

특정 조직을 더 잘 보이게 강조하기 위한 값. HU 범위값 계산.
보고자 하는 조직의 HU 범위값에 맞춰서 그 조직을 강조하는 방식.

WC: 보고자 하는 조직의 HU 값

WW: HU 값 범위

범위 계산식: `WC - WW/2 ~ WC + WW/2`

#### Photometric Interpretation

dicom 이미지의 visualization 관련 정보.

'MONOCHROME1': 최소값 부분이 하얀색인 흑백 이미지

'MONOCHROME2': 최솟값 부분이 검은색인 흑백 이미지

'RGB': 컬러 이미지

# 2. 명령어

## 2.1. 기본 import

```python
import pydicom
```

## 2.2. dicom 파일 불러오기

```python
dcm = pydicom.read_file(<dicom_path>)
```



### 2.1.1. 상세 분류(설명)


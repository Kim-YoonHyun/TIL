# 1. 개요

- 딥러닝에 활용되는 Network 구조들 모음.

- 기본적으로 tensorflw.keras로 활용가능한 네트워크
  - ResNet50



# 2. CNN

## 2.1. ResNet

### 2.1.1. 참고자료

참고 자료 1. [논문 원본](ResNet.pdf)

참고 자료 2. [상세 설명 블로그](https://bskyvision.com/644)

### 2.1.2. 상세 설명

- 입력값을 출력값에 더해줄 수 있는 Shortcut connections 가 추가된 Residual block 이 구현된 네트워크.

  ![residual block](C:\Users\10-210917\Desktop\public git\TIL\Deep Learning Networks\residual block.png)

- VGG-19 네트워크를 기본 뼈대로 하여 CNN layer 를 더해 깊게 만든 후 shortcut connection 을 추가한 Net.

  ![34layer_residual](C:\Users\10-210917\Desktop\public git\TIL\Deep Learning Networks\34layer_residual.png)위 이미지는 34층 레이어에 대한 예시 이미지임.

- plain 과는 다르게 ResNet은 통상적으로 깊어질수록 에러가 낮아지는 특성이 있으며 
  34 -> 50 -> 101 -> 152층 순으로 성능이 좋아지는 모습을 보임.

- tensorflow.keras 에서 ResNet50 활용가능.

## 2.2. Xception

### 2.2.1. 참고자료

- 참고자료 1: [원본 논문](Xception.pdf)
- 참고자료 2:

### 2.2.2. 상세 설명

- Inception 모듈의 cross-channel corelations 와 spatial correlations 를 더욱 완벽히 계산하고 mapping하기 위한 모듈.

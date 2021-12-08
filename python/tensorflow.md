# tensorflow

## 1. keras

### 1.1. 기본 imprt 

```python
from tensorflow.keras.<name> import <name>
```

### 1.2. ImageDataGenerator

실시간 데이터 증강을 사용하여 텐서 이미지 데이터 batch를 생성. 데이터에 대해 batch 단위로 루프가 순환

데이터 전처리용 keras 클래스.

#### 1.2.1 옵션

```python
ImageDataGenerator(<option>)
```

| 이름                            | 입력값                                                       | 기본값            | 의미                                                         |
| ------------------------------- | ------------------------------------------------------------ | ----------------- | ------------------------------------------------------------ |
| `featurewise_center`            | `bool`                                                       |                   | 특성별로 input의 평균이 0이 되도록 함.                       |
| `samplewise_center`             | `bool`                                                       |                   | 각 샘플의 평균이 0이 되도록 함.                              |
| `featurewise_std_normalization` | `bool`                                                       |                   | input을 각 특성 내에서 데이터셋의 표준편차로 나눔.           |
| `samplewise_std_normalization`  | `bool`                                                       |                   | 각 input을 표준편차로 나눔                                   |
| `zca_epsilon`                   | `float`                                                      | 1e-6              | 영위상 성분분석 백색화의 epsilon 값.                         |
| `zca_whitening`                 | `bool`                                                       |                   | 영위상 성분분석 백색화 적용 여부                             |
| `rotation_range`                | `int`                                                        |                   | 무작위 회전각도 범위                                         |
| `width_shift_range`             | 부동소수점                                                   |                   | 1D 형태의 유사배열 혹은 정수<br />< 1: 전체 가로넓이에서의 비율<br />>=1: 픽셀의 갯수<br />정수: `(-width_shift_range, +width_shift_range)` 사이 구간의 픽셀 갯수. |
| `height_shift_range`            | 부동소수점                                                   |                   | 1D 형태의 유사배열 혹은 정수<br />< 1: 전체 세로넓이에서의 비율<br />>=1: 픽셀의 갯수<br />정수: `(-height_shift_range, +height_shift_range)` 사이 구간의 픽셀 갯수. |
| `brightenss_range`              | `list` or `tuple`                                            |                   | 두 부동소수점 값으로 이루어진 리스트 혹은 튜플.<br />밝기 정도를 조절할 값의 범위. |
| `shear_range`                   | 부동소수점                                                   |                   | 층밀리기 강도                                                |
| `zoom_range`                    | 부동소수점 or<br />[하한, 상한]                              |                   | 무작위 zoom의 범위. <br />부동소수점의 경우 `[하한, 상한] = [1-zoom_range, 1+zoom_range]` |
| `channel_shift_range`           | 부동소수점                                                   |                   | 무작위 채널 이동 범위                                        |
| `fill_mode`                     | 'constant',<br />'neareat',<br />'reflect',<br />'wrap' <br />중 하나 | 'nearest'         | 모드에 따라 input 경계의 바깥 공간을 채움.<br />'constant': `kkkkkkkk|abcd|kkkkkkkk`(cval=k)<br />'neareat': `aaaaaaaa|abcd|dddddddd`<br />'reflect': `abcddcba|abcd|dcbaabcd`<br />'wrap': `abcdabcd|abcd|abcdabcd` |
| `cval`                          | 부동소수점 or<br />정수                                      |                   | `fill_mode = 'constant'` 일 경우 경계 밖 공간에 사용하는 값. |
| `horizontal_flip`               | `bool`                                                       |                   | input을 무작위로 가로로 뒤집음.                              |
| `vertical_flip`                 | `bool`                                                       |                   | input을 무작위로 세로로 뒤집음.                              |
| `rescale`                       | `int, float`                                                 | `None`            | None 혹은 0 인경우 재조절이 적용되지 않음.<br />그외의 경우 (다른변형을 전부 적용 후) 데이터를 주어진 값으로 곱함. |
| `preprocessing_function`        | `function`                                                   |                   | 이미지 크기조절 및 증강이 지난 후 작용하는 함수.<br />계수가 3인 numpy 텐서 단일 이미지를 하나의 인수로 가짐.<br />동일한 형태의 numpy 텐서 출력 필요. |
| `data_format`                   | 이미지 데이터                                                | `'channels_last'` | `'channels_first' = (샘플, 채널, 넓이, 높이)`<br />`'channels_last' = (샘플, 높이, 넓이, 채널)`<br />모드에 따라 들어오는 이미지 데이터의 차원 구성이 바뀌어야함.<br />`~/keras/keras.json`에 위치한 value found in your 케라스 구성 파일의<br /> `image_data_format`값으로 설정할 수 있음. |
| `validation_split`              | 부동소수점(0~1)                                              |                   | validation용도로 남겨둘 이미지 비율.                         |
| `dtype`                         | 자료형                                                       |                   | 생성된 배열에 사용할 자료형.                                 |

#### 1.2.2. method

##### 1.2.1.1. flow_from_directory

디렉토리에의 경로를 전달받아 증강된 데이터의 batch를 생성.

```python
.flow_from_directory(directory, options)

directory:
    표적 디렉토리의 경로값.
```

| 옵션 이름       | 입력값                                                       | 초기값        | 의미                                                         |
| --------------- | ------------------------------------------------------------ | ------------- | ------------------------------------------------------------ |
| `target_size`   | `(int, int) = (높이, 넓이)`                                  | `(256, 256)`  | 모든 이미지의 크기를 재조정할 치수.                          |
| `color_mode`    | 'grayscale',<br />'rbg', 'rbga' 중 하나.                     | 'rbg'         | 변환될 이미지가 1, 3, 4개의 채널 중 하나를 가질지 여부       |
| `classes`       | 하위 디렉토리의 선택적 list                                  | `None`        | 값이 지정되있지 않으면 각 하위 디렉토리를 각기 다른 클래스로 대하는 방식으로.<br />`class_indices` 를 통해서 클래스 이름과 색인 간 매핑을 담은 dict 얻음. |
| `class_mode`    | 'categorical', 'binary',<br />'sparse', 'input',<br />None 중 하나. | 'categorical' | 반환될 label 배열 종류를 결정.<br />categorical: 2D형태의 원-핫 인코딩<br />binary: 1D형태의 이진 라벨.<br />sparse: 1D형태의 정수 라벨.<br />input: 인풋 이미지와 동일한 이미지(주로 자동 인코더와 함께 사용)<br />None: 바렝르 반환하지 않음. |
| `batch_size`    | `int`                                                        | 32            | 데이터 batch 의 크기                                         |
| `shuffle`       | `bool`                                                       | `True`        | 데이터를 뒤섞을지 여부                                       |
| `seed`          | `int, float`                                                 |               | 데이터 shuffle에 사용할 선택적 난수 seed                     |
| `save_to_dir`   | `None` or string                                             | `None`        | 디렉토리를 선택적 지정해서 생성된 증강 사진을 저장할 수 있도록 함.<br />현재작업 시각화에 유용. |
| `save_prefix`   | string                                                       |               | `save_to_dir`이 설정된 경우만 사용.<br />저장된 사진의 파일 이름에 사용할 접두 부호. |
| `save_format`   | png or jpeg                                                  | png           | `save_to_dir` 이 설정된 경우만 사용.                         |
| `follow_links`  | `bool`                                                       | `False`       | 클래스 하위 디렉토리 내 심볼릭 링크를 따라갈지 여부          |
| `subset`        | 'training' or 'validation'                                   |               | `ImageDataGenerator` 의 `validation_split`이 설정된 경우 데이터의 부분집합. |
| `interpolation` | 'nearest', 'bilinear', 'bicubic',<br />(PIL 1.1.3 이상): 'lanczos',<br />(PIL 3.4.0 이상): 'box', 'hamming' | 'nearest'     | 로드된 이미지의 크기와 표적 크기가 다른 경우 이미지를 다시 샘플링하는 보간법 메소드. |

- 반환값

  ```python
  (x, y)
  x: (batch_size, 표적크기, 채널), numpy array.
  y: x에 대응하는 라벨 numpy array.
  ```


### 1.3. Sequential

#### 1.3.1. 모델 생성

각 레이어에 **정확히 하나의 입력 텐서와 하나의 출력 텐서**가 있는 **일반 레이어 스택**에 적합.

<적합하지 않은 경우>
모델 또는 레이어에 다중입력 또는 다중 출력이 있는경우.
레이어 공유가 필요한 경우.
비선형 토폴로지(잔류연결, 다중 분기 모델)이 필요한 경우.

```python
model = keras.Sequential(
    [
        layers.Dense(2, activation='relu', name='layer1')
        layers.Dense(3, activation='relu', name='layer2')
        layers.Dense(4, name='layer3')
    ]
)

x = tf.ones((3, 3))
y = model(x)
```

```python
layer1 = layers.Dense(2, activation='relu', name='layer1')
layer2 = layers.Dense(3, activation='relu', name='layer2')
layer3 = layers.Dense(4, name='layer3')

x = tf.ones((3, 3))
y = model(x)
```

위 두 코드는 동일한 코드이다.

##### 1.3.1.2. layer 확인

```python
model.layers
```

##### 1.3.1.3. layer 추가(점진적 작성)

```python
model.add(layer.Dense(5, activation='relu', name='layer5'))
model.add(layer.Dense(6), name='layer6')
```

##### 1.3.1.4. layer 제거

```python
model.pop()	# 리스트와 유사하게 작동
```

##### 1.3.1.5. weight 확인

```python
model.weights
```

##### 1.3.1.6. 모델 내용 요약

```python
model.summary
```

#### 1.3.2. 모델 구성

```python
# 기본
model.compile(
    optimizer='adam'
    loss='binary_crosstropy'
    metrics=['accuracy']
)
```

- loss(손실): 학습을 통해 직접적으로 줄이고자 하는 값.

  square_loss, entropy_loss, binary_crossentropy, categorical_crossentropy 등

- metric(척도): 학습을 통해 목표를 얼마나 잘(못) 달성했는지 평가할 항목.

  accuracy 등

| 인수                 | 설명                                                         | 값 예시                 |
| -------------------- | ------------------------------------------------------------ | ----------------------- |
| `optimizer`          | 최적화 프로그램                                              | `'adam'`                |
| `loss`               | 손실 함수                                                    | `'binary_crossentropy'` |
| `metrics`            | 척도 값                                                      | `'accuracy'`            |
| `loss_weights`       | 각기 다른 모델 아웃풋의 손실 기여도에 가중치를 부여하는 스칼라 계수를 특정하는 선택적 리스트 또는 딕셔너리. |                         |
| `sample_weight_mode` | `temporal` 설정시 시간 단계별 샘플 가중치 부여               | `None(default)`         |
| `weighted_metrics`   | `sample_weight`이나 `class_weight`으로 가중치를 적용하여 평가할 측정항목의 리스트 |                         |
| `target_tensors`     |                                                              |                         |

#### 1.3.3 모델 학습

##### 1.3.3.1 단일 input

```python
fit(
    x=None, y=None, batch_size=None, 
    epochs=1, 
    initial_epoch=0, # 학습을 시작할 epoch
    steps_per_epoch=None
    validation_split=0.0, validation_data=None, , validation_steps=None, 
    
    shuffle=True, 
    verbose=1, callbacks=None, 
    class_weight=None, sample_weight=None, 
     
)
```

| 이름               | 값              | default | 설명                                                         |
| ------------------ | --------------- | ------- | ------------------------------------------------------------ |
| `initial_epoch`    | `int`           |         | 학습을 시작할 epoch. 이전의 학습 과정 재개에 유용.           |
| `step_per_epoch`   | `int`           | `None`  | 다음 epoch를 시작하기까지의 sample batch 의 총 갯수.<br />None일 경우 sample batch의 총 갯수는<br />샘플수/batch 또는 1(확정 불가일 경우)이 된다. |
| `validation_split` | `0 ~ 1`         |         | 검증용 데이터 비율.                                          |
| `validation_data`  | `tuple`         |         | 검증용 데이터 (`validation_split`보다 우선순위 높음).        |
| `validation_step`  | `int`           |         | 정지 전 검증할 sample batch의 총 갯수.<br />`step_per_epoch` 가 지정된 경우만 유의미. |
| `shuffle`          | `boolean`       |         | 학습데이터를 뒤섞을지 여부.<br />`step_per_epoch`가 지정되지 않은 경우만 유의미. |
| `verbose`          | `0, 1, 2`       |         | 0: 자동.<br />1: 진행 표시줄.<br />2: epoch 당 한 라인.      |
| `callbacks`        | `instance list` |         | 학습과 검증 과정에서 적용할 콜백 리스트.                     |



##### 1.3.3.2 배치 단위로 생산한 data input

```python
fit_generator(
    generator, 
    epochs=1, initial_epoch=0
    steps_per_epoch=None,
    validation_data=None, validation_steps=None,
    
    shuffle=True,
    verbose=1, callbacks=None, 
    class_weight=None, 
    max_queue_size=10, workers=1, use_multiprocessing=False, 
    
)
```



#### 1.3.4 모델 평가

```python
predict(
    x, 
    batch_size=None, 
    verbose=0, 
    steps=None, 
    callbacks=None
)
```

| 이름   | 값            | default | 설명                                                         |
| ------ | ------------- | ------- | ------------------------------------------------------------ |
| `x`    | `numpy array` |         | 평가할 인풋 데이터                                           |
| `step` | `int`         | `None`  | 예측이 한번 완료될 때 까지의 단계.<br />None일 경우 고려되지 않음. |




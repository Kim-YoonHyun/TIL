# tensorflow

## keras

### 기본 imprt 

```python
from tensorflow.keras.<name> import <name>
```

### ImageDataGenerator

실시간 데이터 증강을 사용하여 텐서 이미지 데이터 batch를 생성. 데이터에 대해 batch 단위로 루프가 순환

데이터 전처리용 keras 클래스.

#### 옵션

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

#### method

##### flow_from_directory

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

  


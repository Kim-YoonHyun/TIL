# 1. 개요

- 이미지를 처리하는 모듈
- 보편적으로 cv2 를 활용함

# 2. 명령어

## 2.1. 설치 & import

```bash
$ pip install opencv-python
```

```python
import cv2
```



## 2.2. 이미지 제어

### 2.2.1. 불러오기

- 이미지는 numpy 배열 형식

```python
img = cv2.imread('<path>/<img_name>')
```

### 2.2.2. RGB => GRAY

```python
img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
```

### 2.2.3. RGB => HSV

```python
img_hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
```

### 2.2.4. RGB => LAB

```python
img_lab = cv2.cvtColor(img, cv2.COLOR_BGR2LAB)
```

### 2.2.5.  resize

```python
resize_img = cv2.resize(img, dsize=(50, 50), interpolation=cv2.INTER_AREA)
```

### 2.2.6 show

```python
cv2.imshow('title', img)
cv2.waitKey()			# imshow 이후 꼭 적어야함
cv2.destroyAllWindows()	# imshow 이후 꼭 적어야함
```

### 2.2.7. 저장

```python
cv2.imwrite('<path>/<image_name>', img)
```

## 2.3. 검출

### 2.3.1. threshold

```python
cv2.threshold(
    src=image,	# 입력 이미지(흑백)
    thresh=128	# 임계값
    maxval=255,	# 임계값을 넘었을 때 적용할 값
    type=cv2.THRESH_BINARY
    type=cv2.THRESH_BINARY_INV
    type=cv2.THRESH_TRUNC
    type=cv2.THRESH_TOZERO
    type=cv2.THRESH_TOZERO_INV
)	# 방식
```

```python
cv2.adaptiveThreshold(
    src,		# 입력이미지(흑백)
    maxValue,	# 임계값
    adaptiveMethod,		# thresholding value 를 결정하는 계산방법
    thresholdType,		# threshold type
    blockSize,			# thresholding을 적용할 영역 사이즈
    C		# 평균이나 가중평균에서 차감할 값
                     )
```

### 2.3.2 원 검출

```python
circles = cv2.HoughCircles(gray, 	# 입력 이미지
                           cv2.HOUGH_GRADIENT, # 검출 방법
                           1, 	# 해상도 비율
                           100,	# 최소 거리
                           param1=250,	#캐니 엣지 임계값
                           param2=10,	# 중심 임계값
                           minRadis=80,	# 최소 반지름
                           maxRadius=120)	# 최대 반지름
for i in circles[0]:
    cv2.circle(img_raw.copy(), (i[0], i[1]), i[2], (255, 255, 255), 5)
```

검출 방법: 고정

해상도 비율: 원의 중심을 검출하는데 사용되는 누산 평면의 해상도
1 -> 입력한 이미지와 동일한 해상도
2 -> 해상도가 절반으로 줄어 입력 이미지 크기와 반비례

최소거리: 검출된 원과 원 사이의 최소 거리. 원이 여러개 검출되는것을 방지

캐니 엣지 임계값: 허프 변환에서 자체적으로 캐니엣지를 적용할때 사용되는 상위 임계값
하위 임계값은 지정한 상위 임계값의 절반값으로 자동 지정

중심 임계값: 그레디언트 방법에 적용된 중심 히스토그램에 대한 임계값. 값이 작을수록 더 많은 원이 검출.

최소&최대 반지름: 검출될 원의 반지름 범위. 0으로 지정하면 반지름에 제한을 두지 않음.
최대 반지름에 음수를 입력하면 검출된 원의 중심만 반환

circles 는 (1, N, 3) 차원 형태. 반복문을 통해 중심점과 반지름을 반환.

### 2.3.3. 특정 범위 안에 있는 행렬 원소 검출

```python
mask = cv2.inRange(src, lowerb, upperb, dst=None)
```

## 2.4. track bar

### 2.4.1. 기본 구조

```python
def onChange(x):
    pass


# 윈도우 이름 설정
cv2.nameWindow('<window name>')

# 최솟값 ~ 최댓값 범위를 지닌 트랙바 생성
cv2.createTrackbar('<var name>', <min num>, <max num>, onChange)

# 트랙바에 표시할 최초 값 설정
var = <num>
cv2.setTrackbarPos('upper_h', 'Trackbar Windows', var)

# 'q' 입력을 통해 윈도우 종료시 트랙바 값 추출
while cv2.waitKey(1) != ord('q'):
    var1 = cv2.getTrackberPos('<var name>', '<window name>')
print(var1)
```

### 2.4.2. 예시

```python
def onChange(pos):
    pass

cv2.namedWindow('Trackbar Windows')
cv2.createTrackbar('var', 'Trackbar Windows', 0, 255, onChange)

var = 125
cv2.setTrackbarPos('var', 'Trackbar Windows', var)

while cv2.waitKey(1) != ord('q'):
    var = cv2.getTrackbarPos('var', 'Trackbar Windows')

cv2.destroyAllWindows()
print(var)
```





## 기타 

### numpy 입력할때

```python
numpy 의 type 을 uint8 로 해야함
a = np.array(a, dtype=np.uint8)
```


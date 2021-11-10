# OpenCV

## CV2

### 이미지 불러오기

이미지는 numpy 배열 형식과 동일

```python
img = cv2.imread('<path>/<image_name>.ext')
```

### 이미지 변환

```python
img_gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
img_hsv = cv2.cvtColor(img, cv2.COLOR_BGR2HSV)
img_lab = cv2.cvtColor(img, cv2.COLOR_BGR2LAB)
```

### 이미지 show

```python
cv2.imshow('title', img)
cv2.waitKey()			# imshow 이후 꼭 적어야함
cv2.destroyAllWindows()	# imshow 이후 꼭 적어야함
```

### 이미지 저장

```python
cv2.imwrite('<path>/<image_name>.ext', img)
```

### threshold

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

### 원 검출

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

### 특정 범위 안에 있는 행렬 원소 검출

```python
mask = cv2.inRange(src, lowerb, upperb, dst=None)
```


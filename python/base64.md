# 1. 개요

# 2. 명령어

## 2.1. encoding

### 2.1.1. numpy to base64

1.  numpy 의 dtype 이 int64 여야한다. (uint 는 안됨)
2. base64.b64encode(bytes)
   byte 로 변환 한 값을 입력해야 하며 numpy arry 를 그대로 입력하면 안된다.

```python
arr = np.array([[1, 2, 3], [4, 5, 6],])
byte_data = arr.tobytes()
base64_data = base64.b64encode(byte_data)
```

### 2.1.2. opencv

```python
img = cv2.imread('<path>/image.jpg')
jpg_img = cv2.imencode('.jpg', img)
b64_string = base64.b64encode(jpg_img[1]).decode('utf-8')
```



## 2.2. decoding

### 2.2.1. base64 to numpy

```python
byte_data = base64.b64decode(base64_data)
arr = np.frombuffer(byte_data, dtype=np.int32)
```

```python
imgdata = base64.b64decode(b64_string)
dataBytesIO = io.BytesIO(imgdata)
image = Image.open(dataBytesIO)
image = cv2.cvtColor(np.array(image), cv2.COLOR_BGR2RGB)
print(np.shape(image))

cv2.imshow('tt', image)
```


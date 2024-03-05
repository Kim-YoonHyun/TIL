# 1. 개요

# 2. 명령어

## 2.1. 기본 import

```python
import matplotlib as mpl
import matplotlib.pyplot as plt
```

- spyder 에서 창을 따로 띄우고 싶을때
  Tools -> Preferences -> IPython console -> Graphics -> Graphics backend 로 가서 Backend 를 Automatic 으로 변경

### 2.1.1. 한글 폰트 사용

```python
import matplotlib.font_manager as fm
path = 'C:/NanumBarunGothic.ttf'		# 다운로드 필요
fontprop = fm.FontProperties(fname=path, size=10) 

# 이후 한글 폰트를 적용하고자 하는 곳에 fontproperties=fontprop 입력
```

## 2.2. plot

### 2.2.1. figure 크기 조정

```python
plt.figure(figsize=(10, 5))
```

### 2.2.2. 제목 설정

```python
plt.title('title')
```

### 2.2.3. 기본 plot

```python
x = [1, 2, 3, 4, 5]
y = [1, 2, 3, 4, 5]
plt.plot(x, y)
```

#### 2.2.3.1. 추가 plot

```python
x = [1, 2, 3, 4, 5]
y = [1, 2, 3, 4, 5]
plt.plot(x, y)

y2 = [1, 3, 5, 7, 9]
plt.plot(x, y2)
```

#### 2.1.3.2. 옵션

```python
plt.plot(x, y, 
         ls='--',	# 선 스타일
         lw=5,  	# 선 굵기
         c='b', 	# 선 색
         marker='o',# 마커 형태
         ms=15,		# 마커 사이즈
         mfc='r',	# 마커 색
         mec='g',	# 마커 테두리 색
         mew=5,		# 마커 테두리 굵기
         label='a'  # 라벨
        )

plt.plot(x, y, 'bo--') # 축약형태로 가능
```

| 색 명령어 | 의미    | 마커명령어 | 의미           | 스타일명령어 | 의미     |
| --------- | ------- | ---------- | -------------- | ------------ | -------- |
| `b`       | blue    | `.`        | point marker   | `-`          | soild    |
| `g`       | green   | `,`        | pixel          | `--`         | dashed   |
| `r`       | red     | `o`        | circle         | `-.`         | dash-dot |
| `c`       | cyan    | `v`        | triangle_down  | `:`          | dotted   |
| `m`       | magenta | `^`        | triangle_up    |              |          |
| `y`       | yellow  | `<`        | triangle_left  |              |          |
| `k`       | black   | `>`        | triangle_right |              |          |
| `w`       | white   | `1`        | tri_down       |              |          |
|           |         | `2`        | tri_up         |              |          |
|           |         | `3`        | tri_left       |              |          |
|           |         | `4`        | tri_right      |              |          |
|           |         | `s`        | square         |              |          |
|           |         | `p`        | pentagon       |              |          |
|           |         | `*`        | star           |              |          |
|           |         | `h`        | hexagon1       |              |          |
|           |         | `H`        | hexagon2       |              |          |
|           |         | `+`        | plus           |              |          |
|           |         | `x`        | x              |              |          |
|           |         | `D`        | diamond        |              |          |
|           |         | `d`        | thin_diamond   |              |          |

### 2.2.4. 축 설정

#### 2.2.4.1. 축 길이 설정

```python
plt.xlim(0, 100)	# (최솟값, 최댓값)
plt.ylim(0, 100)
```

#### 2.2.4.2. 격자 설정

```python
plt.xticks([0, 10, 30, 40, 50])	# (최솟값, 중간, ...,  최댓값)
plt.yticks([0, 5, 10, 20])
```

#### 2.2.4.3. 축 범위 없애기

```python
# x범위 없애기
plt.gca().axes.xaxis.set_visible(False)#x범위 없애기

# y 범위 없애기
plt.gca().axes.yaxis.set_visible(False)#y범위 없애기

[ 출처: https://seong6496.tistory.com/ ]
```



### 2.2.5. 축 이름 설정

```python
plt.xlabel('xlabel')
plt.ylabel('ylabel')
```

### 2.2.6. legend 표시

```python
plt.legend(loc=1)
```

### 2.2.7. 격자 표시

```python
plt.grid()
```

### 2.2.8. color 바 표시

```python
plt.colorbar()
```

### 2.2.9. image 저장

```python
plt.savefig('<path>/image_name.png', 	# 저장 경로 및 파일이름, 확장자 설정
            dpi=100, 					# dpi(해상도) 설정
            facecolor='#eeeeee', 		# 배경색 설정 (기본 흰색)
            edgecolor='blue')			# 테두리색 설정 (기본 흰색)
```

### 2.2.10. show

```python
plt.show()
```

### 2.2.11. subplot

```python
x = [1, 2, 3, 4, 5]

plt.subplot(2, 1, 1)		# 2 X 1 에서 1 의 위치
y = [1, 2, 3, 4, 5]
plt.plot(x, y)
# <그외 구성 코드>

plt.subplot(2, 1, 2)		# 2 X 1 에서 2 의 위치
y2 = [1, 3, 5, 7, 9]
plt.plot(x, y2)
# <그외 구성 코드>

plt.tight_layout()		# 각 서브플롯의 레이아웃이 겹치지 않게 조정
plt.show()
```

### 2.2.12 모든 설정 초기화

```python
plt.clf()
```

### 2.2.13. 다양한 plot 모음

#### imshow()

이미지 플롯

#### bar, barh

```python
x = [0, 1, 2]
y = [200, 300, 100]
plt.bar(x, y)
plt.show()
```

![bar_chart](C:\Users\10-210917\Desktop\public git\TIL\python\bar_chart.png)

```python
plt.barh(x, y)
plt.show()
```

![barh_chart](C:\Users\10-210917\Desktop\public git\TIL\python\barh_chart.png)

#### pie chart

```python
sizes = [15, 30, 45, 10]
explode = (0, 0.1, 0, 0)  # 간격 여부
labels = ['prog', 'pig', 'dog', 'tree']
colors = ['yellowgreen', 'gold', 'lightskyblue', 'lightcoral']

plt.title("Pie Chart")
plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct='%1.1f%%', shadow=True, startangle=90)
plt.axis('equal')
plt.savefig('pie_chart.png')
plt.show(block=False)
plt.pause(2)
plt.close()
```

![pie_chart](C:\Users\10-210917\Desktop\public git\TIL\python\pie_chart.png)

#### histogram

```python
# histogram
np.random.seed(0)
x = np.random.randn(1000)
plt.title("Histogram")
arrays, bins, patches = plt.hist(x, bins=10)    # x 축의 값을 일정한 간격으로 나눔

plt.savefig('histogram.png')
plt.show(block=False)
plt.pause(2)
plt.close()
```

![histogram](C:\Users\10-210917\Desktop\public git\TIL\python\histogram.png)

#### scatter

```python
# scatter
np.random.seed(0)
x = np.random.normal(0, 1, 100)
y = np.random.normal(0, 1, 100)
plt.title('scatter plot')
plt.scatter(x, y)
plt.show()
```

![scatter_plot](C:\Users\10-210917\Desktop\public git\TIL\python\scatter_plot.png)

#### box

```python
plt.boxplot([x1, y1, x2, y2],
            labels=['x1_label', 'y1_label', 'x2_label', 'y2_label'])
```

## 2.3. imshow

이미지를 보이기 위한 용도.

### 2.3.1. 기본 imshow

```python
img = np.array([[20,2,2],[2,20,2],[2,2,20]])
plt.imshow(img)
```




# 1. 개요

- 학습에 활용 되는 모듈
- [pytorch 공식 사이트](https://pytorch.org/get-started/locally/)를 통해 install 가능

# 2. 명령어

## 2.1. tensor

### 2.1.1. 사이즈 확인

```python
x = torch.FloatTensor([
    [1, 2],
    [3, 4],
    [5, 6],
    [7, 8]
])

x.size()
```



## 모델

### 모델 저장

```python
torch.save(model.state_dict(), PATH)
```

### 모델 불러오기

```python
model.load_state_dict(torch.load(PATH))
model.eval()
```





### 2.1.1. 상세 분류(설명)


# 1. 개요

- 학습에 활용 되는 모듈
- pytorch 공식 사이트](https://pytorch.org/get-started/locally/)를 통해 install 가

11.7 버전

```python
conda install pytorch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 pytorch-cuda=11.7 -c pytorch -c nvidia
```



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

## 3.1. Dataloader

### 3.1.1. import 방법

```python
from torch.utils.data import DataLoader
```

### 3.1.2. options

[참고](https://subinium.github.io/pytorch-dataloader/)

| 옵션             | 타입                       | default | 설명                                                         |
| ---------------- | -------------------------- | ------- | ------------------------------------------------------------ |
| `dataset`        | `torch.utils.data.Dataset` | -       |                                                              |
| `batch_size`     | `int`                      | 1       | 배치의 크기                                                  |
| `shuffle`        | `bool`                     | `False` | 데이터 섞기 여부                                             |
| `sampler`        | `Sampler`                  | -       | 데이터의 index 를 원하는 방식대로 조정                       |
| `batch_sampler`  | `Sampler`                  | -       | 데이터의 batch 를 원하는 방식대로 조정                       |
| `num_workers`    | `int`                      | 0       | 데이터로딩에 사용하는 subprocess 개수                        |
| `collate_fn`     | -                          | -       | dataset 이 고정된 길이가 아닐 경우 batch size 를 생성하기 위한 사용자 지정 함수 |
| `pin_memory`     | `bool`                     | -       | `True` 인 경우 tensor 를 cuda 고정 메모리에 올림.            |
| `drop_last`      | `bool`                     | -       | 마지막 batch 를 사용하지 않음.                               |
| `time_out`       | `int` `float`              | 0       | Dataloader 가 data를 불러오는 제한시간.                      |
| `worker_init_fn` |                            | `None`  | 불러올 worker를 리스트로 전달.                               |

## 2.3. nn

### 2.3.1. nn.Embedding

```python
torch.nn.Embedding(num_embeddings, embedding_dim)
```

num_embeddings 값은 입력되는 텐서 내부의 값의 최댓값 보다 1 이상 커야한다.

```python
input = torch.LongTensor([0, 2, 3, 4, 5, 6, 7, 1])
embedding_layer = nn.Embedding(num_embeddings=8, embedding_dim=3)
'''
tensor([[-0.1778, -1.9974, -1.2478],  --> 0
        [ 0.0000,  0.0000,  0.0000],  --> 1
        [ 1.0921,  0.0416, -0.7896],  --> 2
        [ 0.0960, -0.6029,  0.3721],  --> 3
        [ 0.2780, -0.4300, -1.9770],  --> 4
        [ 0.0727,  0.5782, -3.2617],  --> 5
        [-0.0173, -0.7092,  0.9121],  --> 6
        [-0.4817, -1.1222,  2.2774]], --> 7
        requires_grad=True)
'''
```

위의 예시에서 input 텐서 내부의 값 중 최댓값은 9 이다. 이에, Embedding 을 생성시 num_embeddings 가 8 보다 작은 경우 input 텐서의 idx 에 매칭되는 임베딩 벡터가 존재하지 않아 에러가 발생한다.



## 2.4. F vs torch.nn

```python
import torch
import torch.nn as nn
import torch.nn.functional as F

class OurModel(nn.Module):
    def __init__(self, in_dim, hidden_dim, out_dim):
        super(OurModel, self).__init__()
        self.linear = nn.Linear(in_dim, hidden_dim)
        self.relu = nn.ReLU()  # nn.Module 로 선언가능

    def forward(self, x):
        x = self.linear(x)
        out = self.relu(x) 
        out = F.relu(x) # forward 에서 함수로 바로 호출 가능 
        return out
```






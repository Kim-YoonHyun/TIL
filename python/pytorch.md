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

## 3.1. Dataloader

### 3.1.1. import 방법

```python
from torch.utils.data import DataLoader
```

### 3.1.2. options

[참고](https://subinium.github.io/pytorch-dataloader/)

| 옵션             | 타입                       | default | 설명                                              |
| ---------------- | -------------------------- | ------- | ------------------------------------------------- |
| `dataset`        | `torch.utils.data.Dataset` | -       |                                                   |
| `batch_size`     | `int`                      | 1       | 배치의 크기                                       |
| `shuffle`        | `bool`                     | `False` | 데이터 섞기 여부                                  |
| `sampler`        | `Sampler`                  | -       | 데이터의 index 를 원하는 방식대로 조정            |
| `batch_sampler`  | `Sampler`                  | -       | 데이터의 batch 를 원하는 방식대로 조정            |
| `num_workers`    | `int`                      | 0       | 데이터로딩에 사용하는 subprocess 개수             |
| `collate_fn`     | -                          | -       | smaple_list를 batch 단위로 바꾸는 기능            |
| `pin_memory`     | `bool`                     | -       | `True` 인 경우 tensor 를 cuda 고정 메모리에 올림. |
| `drop_last`      | `bool`                     | -       | 마지막 batch 를 사용하지 않음.                    |
| `time_out`       | `int` `float`              | 0       | Dataloader 가 data를 불러오는 제한시간.           |
| `worker_init_fn` |                            | `None`  | 불러올 worker를 리스트로 전달.                    |


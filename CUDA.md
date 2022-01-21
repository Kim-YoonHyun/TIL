# 1. 개요

# 2. 

## 2.1. 오류

### 2.1.1. RuntimeError

- `all CUDA-capable devices are busy or unavailable`

메모리가 가득차거나 해서 할당량이 없는 경우일 때가 많음.

단순히 코드 실행을 종료하는것이 아니라 메모리에서 완전 제거를 해줘야함

```bash
$ pkill -ef -9 python
```




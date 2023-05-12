# 1. 개요

# 2. 명령어

## 2.1. 기본

```python
# map 과 lambda

# 일반 함수 이용
def func_mul(x):
    return x * 2

result1 = list(map(func_mul, [5, 4, 3, 2, 1]))
print(f"map(일반함수, 리스트) : {result1}")

# 람다 함수 이용
result2 = list(map(lambda x: x * 2, [5, 4, 3, 2, 1]))
print(f"map(람다함수, 리스트) : {result2}")
```


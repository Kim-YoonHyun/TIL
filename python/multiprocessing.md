# 1. 개요

# 2. 명령어

## 2.1. 기본 import

```python
import multiprocessing
```

### 2.2. Process

### 2.2.1. 예시

```python
import os
from multiprocessing import Process

def add_one(num):
    p_id = os.getpid()
    print('({} + 1) done by ---> process {}'.format(num, p_id))
    num += 1
	
if __name__ == '__main__':
    nums = range(1, 10)
    procs = []

    for idx, num in enumerate(nums):
        proc = Process(target=add_one, args=(num,))
        procs.append(proc)
        proc.start()

    for proc in procs:
        proc.join()
```

### 2.3. Manager()

### 2.3.1. 예시

```python
import multiprocessing



def worker(data, final_list):
    for item in data:
        final_list.append(item)

if __name__ == '__main__':
    manager = multiprocessing.Manager()
    final_list = manager.list()

    input_list_one = ['one', 'two', 'three', 'four', 'five']
    input_list_two = ['six', 'seven', 'eight', 'nine', 'ten']
    process1 = multiprocessing.Process(target=worker, args=[input_list_one, final_list])
    process2 = multiprocessing.Process(target=worker, args=[input_list_two, final_list])

    process1.start()
    process2.start()

    process1.join()
    process2.join()

    print(final_list)
```


# 1. 개요

압축

# 2. 명령어

## 2.1. tar

모든 파일을 하나의 파일로 응축하기만 한 형태.

용량 축소 효과는 없지만 속도가 빠름

### 2.1.1. 압축하기

```bash
tar -cvf <파일명>.tar <폴더명>
# tar -cvf aaa.tar abc
```

### 2.1.2. 압축풀기

```bash
tar -xvf <파일명>.tar
# tar -xvf aaa.tar
```

## 2.2. tar.gz

모든 파일을 하나의 파일로 압축하고 용량이 축소된 형태

속도가 느림

### 2.2.1. 압축하기

```bash
tar -zcvf <파일명>.tar.gz <폴더명>
```

- 특정 확장자 제외 압축

  ```bash
  tar --exclude='*.csv' -zcvf <파일명>.tar.gz <폴더명>
  ```

- 특정 디렉토리 제외 압축

  ```bash
  tar --exclude='my_folder/tmp/*' -zcvf <파일명>.tar.gz <폴더명>
  ```

  ```bash
  tar --exclude='project/module1/*' \
      --exclude='project/module2/*' \
      -zcvf <파일명>.tar.gz <폴더명>
  ```

  

### 압축해제

```bash
tar -zxvf <파일명>.tar.gz
tar -zxvf <파일명>.tar.gz -C <폴더명> # 덮어쓰기
tar -zxvf <파일명>.tar.gz --strip-components=1  # 상위 폴더 1개 제거
```








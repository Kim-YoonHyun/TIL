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

v 는 압축 내역을 보여주는 것으로 생략하면 안보임

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


### 2.2.2. 폴더 명 변경 압축

stage/temp.py  을 압축후 해제했을때 system/temp.py 이 되도록 하는 방법

```bash
tar -zcvf output.tar.gz --transform='s|^stage/|system/|' stage/
```

`transform` : 변경 옵션 적용

`s`: 치환(substitute)

`^stage/` : 경로 시작부분의 `stage/` 문자열을 찾음

`system/` : 찾은 문자열을 `system/` 으로 치환

`stage/` : 압축할 대상 폴더

`|` : 구분자를 의미하며 반드시 `|` 를 쓰는 것이 아니라 자유롭게 지정가능. 가독성&편의성 중시
예: 콤마 `,` 사용 -> `'s,^stage/,system/,'`

- 압축 확인
  ```bash
  tar -tvf output.tar.gz
  ```

`exclude` 와 같이 사용

```bash
tar -zcvf output.tar.gz --transform='s|^stage/|system/|' \
	--exclude='*/old' \
	--exclued='*/log' \
	stage/
```

### 압축해제

```bash
tar -zxvf <파일명>.tar.gz
tar -zxvf <파일명>.tar.gz -C <폴더명> # 덮어쓰기
tar -zxvf <파일명>.tar.gz --strip-components=1  # 상위 폴더 1개 제거
```








파이썬 버전에 맞춰서 설치

```bash
$python -m pip install <모듈>
```

# 2. 명령어

## 2.1. requirements

### 2.1.1. 생성

```bash
pip freeze > requirements.txt
```

### 2.1.2. 활성화

```bash
pip install -r requirements.txt
```

## 2.2. 폐쇄망

### 2.2.1. download

```bash
pip download -d <경로> <패키지명>==<버전>
# 예시
# -d : <경로> 에 디렉토리 설정
pip download -d ./ seaborn==0.11.2
# requirements
pip download -d ./ -r requirements.txt
```

pip download 옵션 관련 참고

https://pip.pypa.io/en/stable/cli/pip_download/

### 2.1.4. install

```bash
pip install --no-index --find-links=./ <패키지명>==<버전>
pip install --no-index --find-links=./ seaborn==0.11.2

# --no-index --find-links=./
# 패키지의 인덱스를 무시하고 ./ 경로에서 찾은 패키지만 다운
```


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

### 2.1.3. download

pip download 옵션 관련 참고

https://pip.pypa.io/en/stable/cli/pip_download/

#### 특정 디렉토리

```bash
pip download -d $PATH -r requirements.txt
```


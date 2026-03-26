# 기본

## 경로

### 생성

현재 파일 위치 경로 추출

```python
BASE_DIR = Path(__file__).resolve().parent
```

```python
from pathlib import Path

# 현재 작업 디렉토리 기준
base_path = Path.cwd() 

# 경로 합치기 (운영체제에 맞는 구분자를 자동으로 사용)
new_config = base_path / "settings" / "config.yaml"

# 디렉토리가 없으면 생성 (exist_ok=True는 이미 있어도 에러 안 냄)
new_config.parent.mkdir(parents=True, exist_ok=True)

print(new_config)
# 출력: /Users/name/project/settings/config.yaml (mac/linux)
# 출력: C:\Users\name\project\settings\config.yaml (windows)
```

## 파일

### 정보추출

```python
file_path = Path("data/results/report.pdf")

print(file_path.name)      # report.pdf (파일명 전체)
print(file_path.stem)      # report (확장자 제외)
print(file_path.suffix)    # .pdf (확장자만)
print(file_path.parent)    # data/results (부모 디렉토리)
file_path.relative_to(start) # 상대 경로

# 확장자만 바꿔야 할 때 (매우 유용!)
csv_path = file_path.with_suffix(".csv")
print(csv_path)            # data/results/report.csv
```

### 탐색

```python
# 현재 폴더에서 .txt 파일 모두 찾기
for txt_file in Path.cwd().glob("*.txt"):
    print(txt_file.name)

# 하위 폴더를 포함하여 모든 .py 파일 찾기 (재귀적 탐색)
py_files = list(Path.cwd().rglob("*.py"))
print(f"발견된 파이썬 파일 수: {len(py_files)}")
```

### 읽기 & 쓰기

```python
p = Path("hello.txt")

# 텍스트 쓰기
p.write_text("Hello, Pathlib!", encoding="utf-8")

# 텍스트 읽기
content = p.read_text(encoding="utf-8")
print(content)

# 파일 존재 여부 확인
if p.exists():
    print(f"{p.name} 파일이 존재합니다.")
```


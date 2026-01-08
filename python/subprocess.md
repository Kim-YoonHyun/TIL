# 1. 개요

# 2. 명령어

## 2.1. 분류

### 2.1.1. 예시

```python
import subprocess

cmd = f"""
cd /home/user/module && \
tar -zcvf output.tar.gz --transform='s|^stage/|system/|' stage/
"""
subprocess.run(cmd, shell=True, check=True)
```

| 옵션    | 기본값  | 설명                                                         |
| ------- | ------- | ------------------------------------------------------------ |
| `shell` | `False` | 명령어를 시스템 쉘을 통해 실행할지 여부 결정<br />`False` : 명령어 직접 실행. 실행 명령어를 띄어쓰기 단위로 나누어서 리스트에 넣어줘야함. 보안상 더 안전<br />예: `ls -al` --> `['ls', '-al']`<br />`True` : 명령어를 하나의 문자열(위의 예시)로 전달 가능. 편리하지만 shell Injection 공격에 취약함 |
| `check` | `False` | 명령어 실행 성공 여부를 python 이 감지할지 여부<br />`False`: 명령어가 실패해도 python 코드가 멈추지 않음<br />`True` : 명령어가 실패(에러발생)하면 python 에서 `CalledProcessError` 발생 |
| `cwd`   | 없음    | 명령어를 실행할 작업 디렉토리 설정                           |



`shell=True` : 명령어를 시스템 쉘을 통해 실행 여부 결정

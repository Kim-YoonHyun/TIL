# 1. 개요

- **t**erminal **mu**ltiple**x**er 의 약자이며, 터미널을 window와 session 단위로 관리하는 도구.
- 다양한 기능을 실행 할 수 있으며 이 기능을 실행시키는 trigger 는 
  <Ctrl + b> 이다.

# 2. 명령어

- 기존의 터미널 화면과 tmux 상에서의 명령어를 구분하기 위해
  tmux 내부 명령어는 `tmux$` 으로 표현.

## 2.1. session

### 2.1.1. 기본 생성

```bash
$ tmux
```

### 2.1.2. 이름과 같이 생성

```bash
$ tmux new -s <session name>
```

### 2.1.3. 세션, 윈도우 같이 생성

```bash
$ tmux new -s <session_name> -n <window_name>
```

### 2.1.4. 중단

- 프로세스는 계속 실행되고 있음

```python
tmux$ <Ctrl+b> d
```

### 2.1.5. 세션 다시 시작(접속)

```bash
$ tmux attach -t <session_name>
```

### 2.1.6. 종료

```bash
tmux$ exit
```

### 2.1.7. 특정 세션 강제 종료

```bash
$ tmux kill-session -t <session_name>
```

### 2.1.8. 세션 리스트 확인

```bash
$ tmux ls
```



## 2.2. window

### 2.2.1. 나눠진 창(pane) 추가

```bash
tmux$ <Ctrl+b> %
```

### 2.2.2. 새로운 창 추가

```bash
tmux$ <Ctrl+b> c
```

### 2.2.3. 창 이동

```bash
tmux$ <ctrl+b> 0 # 창 번호 입력
```

- 이동된 창 이름에 * 가 붙어있는것으로 확인 가능.

### 2.2.4. pane 개별 종료

```bash
tmux$ <Ctrl+b> x
```

### 2.2.5. 해당 창 종료

- pane 와 상관없이 전체 창을 종료시켜버림.

```bash
tmux$ <Ctrl+b> &
```

### 2.2.6. 창 스크롤

```bash
tmux$ <ctrl+b> [
tmux$ q # 스크롤 모드 종료
```


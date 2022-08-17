### 아나콘다 prompt 에서 spyder 가상환경 사용법

1. 우선 아나콘다 가상환경을 만든다.
2. `conda istall spyder` 입력하여 spyder 설치
3. `spyder` 입력하여 spyder를 가상환경 기반으로 시작

# 2. 명령어

## 2.1. 가상환경

### 2.1.1. 가상환경 생성

```bash
$ conda create -n <이름> python=<version>
```

### 2.1.2. 가상환경 리스트

```bash
$ conda info --envs
```

### 2.1.3. 가상환경 삭제

```bash
$ conda remove --name <가상환경이름> --all
```

## 3. Linux 서버 내 설치

```bash
wget https://repo.anaconda.com/archive/Anaconda3-2021.11-Linux-x86_64.sh
bash Anaconda3-2021.11-Linux-x86_64.sh
source ~/.bashrc
```








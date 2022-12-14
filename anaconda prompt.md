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

### 2.1.4. 가상환경 복제(인터넷 망)

```bash
$conda activate <가상환경이름>
$conda env export > <가상환경이름>.yaml
$conda env create -f <가상환경이름>.yaml
```

### 2.1.5. 가상환경 복제(폐쇄망)

#### Anaconda 가이드

1. conda-pack 설치
   ```bash
   $conda install -c conda-forge conda-pack
   or
   $pip install conda-pack
   ```

2. 가상환경 압축
   ```bash
   $conda pack -n my_env
   ```

3. 압축파일을 옮길 서버의 anaconda3/envs 아래로 이동

4. restore
   anaconda3/envs 경로로 이동

   ```bash
   $mkdir -p my_env # 폴더 생성 
   $tar -xzf my_env.tar.gz -C my_env # 해당 폴더 아래 압축 풀기
   ```

## 3. Linux 서버 내 설치

```bash
wget https://repo.anaconda.com/archive/Anaconda3-2021.11-Linux-x86_64.sh
bash Anaconda3-2021.11-Linux-x86_64.sh
source ~/.bashrc
```



```bash
wget https://repo.anaconda.com/archive/Anaconda3-2022.10-Linux-x86_64.sh
bash Anaconda3-2022.10-Linux-x86_64.sh
source ~/.bashrc
```






# 1. 개요

# 2. 설치

## 2.1. 진행

1. CUDA 및 Nvidia-driver 초기화
   ```bash
   sudo apt-get purge nvidia*
   sudo apt-get autoremove
   sudo apt-get autoclean
   sudo rm -rf /usr/local/cuda*
   ```

2. Nvidia-driver 재설치
   ```bash
   sudo apt-get update
   sudo apt-get upgrade
   sudo apt-get install ubuntu-drivers-common
   ubuntu-drivers devices
   ```

   ```bash
   sudo apt-get install nvidia-driver-550 # 위의 명령어 출력결과에서 추천 드라이버
   sudo reboot
   ```

3. cuda 설치
   https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04&target_type=runfile_local 에서 버전에 따른 명령어 확인 후 진행

   ```bash
   wget https://developer.download.nvidia.com/compute/cuda/12.4.0/local_installers/cuda_12.4.0_550.54.14_linux.run
   sh cuda_12.4.0_550.54.14_linux.run
   ```

   진행시 드라이버는 이미 설치햇기에 체크 해제하고 진행

4. bashrc 수정
   ```bash
   # 파일 열기
   sudo vim ~/.bashrc
   
   # 아래 텍스트를 bashrc 가장 아래에 추가
   export PATH=/usr/local/cuda/bin:$PATH
   export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
   
   # 활성화
   source ~/.bashrc
   ```

5. cudnn 설치
   https://developer.nvidia.com/rdp/cudnn-archive 에서 버전에 맞는 설치파일 다운로드

   ```bash
   # 압축 해제
   tar -xvf cudnn-linux-x86_64-8.9.7.29_cuda12-archive.tar.xz
   
   # 압축 푼 경로로 이동
   cd cudnn-linux-x86_64-8.9.7.29_cuda12-archive
   
   # 파일 복사
   sudo cp include/cudnn*.h /usr/local/cuda/include
   sudo cp lib/libcudnn* /usr/local/cuda/lib64
   ```

   ```bash
   # 설치된 cudnn 버전 확인
   cat /usr/local/cuda/include/cudnn_version.h | grep CUDNN_MAJOR -A 2
   ```

   

## 2.2. 오류 관련

- reboot 시 MOK 관련 설정이 나오는 경우
  https://davi06000.tistory.com/22 참고

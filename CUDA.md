# 1. 개요

# 2. 설치

 ## 2.1. cuda

설치 사이트: https://developer.nvidia.com/cuda-toolkit-archive



# 2. 

## 2.1. 오류

### 2.1.1. RuntimeError

- `all CUDA-capable devices are busy or unavailable`

메모리가 가득차거나 해서 할당량이 없는 경우일 때가 많음.

단순히 코드 실행을 종료하는것이 아니라 메모리에서 완전 제거를 해줘야함

```bash
$ pkill -ef -9 python
```

설치 참고

https://normal-engineer.tistory.com/356

https://vividian.net/2022/11/1111

https://webnautes.tistory.com/1844

(드라이버 삭제)
sudo apt-get purge nvidia*
sudo apt-get autoremove
sudo apt-get autoclean

wget https://developer.download.nvidia.com/compute/cuda/11.7.1/local_installers/cuda_11.8.0_520.61.05_linux.run

sudo sh cuda_11.7.0_520.61.05_linux.run





$ sudo sh -c "echo 'export PATH=$PATH:/usr/local/cuda-11.7/bin'>> /etc/profile" $ sudo sh -c "echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.7/lib64'>> /etc/profile" $ sudo sh -c "echo 'export CUDADIR=/usr/local/cuda-11.7'>> /etc/profile" $ source /etc/profile



tar -xvf cudnn-linux-x86_64-8.x.x.x_cudaX.Y-archive.tar.xz 

sudo cp cudnn-*-archive/include/cudnn*.h /usr/local/cuda/include 
sudo cp -P cudnn-*-archive/lib/libcudnn* /usr/local/cuda/lib64 
sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*

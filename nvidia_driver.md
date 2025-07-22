```bash
sudo rmmod nvidia_drm 
# rmmod: ERROR: Module nvidia_drm is not currently loaded 가 떠야함
sudo rmmod nvidia_modeset 
sudo rmmod nvidia_uvm 
sudo rmmod nvidia
```

lshw -numeric -C display

lspci | grep -i nvidia

### 삭제

```bash
sudo apt --purge remove *nvidia*
sudo apt autoremove
sudo apt autoclean
```

### 설치 가능한 드라이버 확인

```bash
ubuntu-drivers devices
```

### 권장 드라이버 자동 설치

```bash
sudo ubuntu-drivers autoinstall
```





- rmmod: ERROR: Module nvidia_drm is in use 에러 발생시

X 서버 종료

sudo service lightdm stop



### cuda 버전 호환 확인

https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html

###  cuda 설치 진행 참고

https://velog.io/@whattsup_kim/gpu%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0

https://webnautes.tistory.com/1844

```bash
nvcc -V
```


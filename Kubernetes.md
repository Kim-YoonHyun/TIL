# 순서

## [minikube]

### 1) kubectl 설치

쿠버네티스에 명령을 내일 CLI 도구 설치

- kuberctl 설치
  ```bash
  curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
  ```

- 다운로드한 파일을 실행 가능 경로로 이동
  ```bash
  sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
  ```

- 설치 확인
  ```bash
  kubectl version --client
  ```

### 2) minikube 설치

가벼운 쿠버네티스 엔진인 미니큐브 설치

- minikube 다운로드
  ```bash
  curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
  ```

- 실행 가능한 경로로 이동
  ```bash
  sudo install minikube-linux-amd64 /usr/local/bin/minikube
  ```

- 설치 확인
  ```bash
  minikube version
  ```

### 3) 클러스터 시작

- docker 드라이버 기반 minikube 시작
  Docker 안에 거대한 컨테이너를 하나 띄워서 그 내부를 통째로 쿠버네티스 전용 공간으로 만드는 과정
  첫 실행 시 필요한 이미지 다운로드에 시간이 소요됨

  ```bash
  minikube start --driver=docker
  ```

- 잘 시작됬는지 확인
  ```bash
  kubectl get nodes
  ```

  `minikube` 라는 이름의 노드가 `Ready` 상태로 보이는지 확인

### 4) 상태 확인

- 현재 실행중인 컨테이너 목록, 상태 확인
  ```bash
  kubectl get pods
  ```

- 파드(pods)의 로그 확인
  ```bash
  kubectl logs <파드의_전체이름>
  ```

### 5) 컨테이너 실행

- k8s-deploy.yaml 파일 생성

  `k8s-deploy` 는 그냥 임의의 이름일 뿐이다.

  docker-compose.yml 가 있을시 그 파일을 변경

  ```yaml
  # 1. analy-module 배포 정의
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: analy-module
  spec:
    replicas: 1 # 띄울 컨테이너(Pod)의 개수
    selector:
      matchLabels:
        app: analy-module
    template:
      metadata:
        labels:
          app: analy-module
      spec:
        hostNetwork: true # network_mode: "host" 와 동일한 역할
        containers:
        - name: analy-module
          image: bus_rt_monitoring:b3.0.28 
          imagePullPolicy: Never # 인터넷 다운로드 시도 방지
          command: ["python", "-m", "scripts.run", "--env", "release", "--run_mode", "normal", "--name", "analy_module"]
          # 컨테이너 내부에 마운트할 경로 설정
          volumeMounts:
          - name: ipconfig-vol
            mountPath: /ipconfig_cryp.ini
            readOnly: true
          - name: cryptogram-vol
            mountPath: /cryptogram
            readOnly: true
          - name: localtime-vol
            mountPath: /etc/localtime
            readOnly: true
          - name: timezone-vol
            mountPath: /etc/timezone
            readOnly: true
        # 실제 호스트(서버)의 파일 경로 정의 (volumes 역할)
        volumes:
        - name: ipconfig-vol
          hostPath:
            path: /home/gj_anly/ipconfig_cryp.ini
        - name: cryptogram-vol
          hostPath:
            path: /home/gj_anly/cryptogram
        - name: localtime-vol
          hostPath:
            path: /etc/localtime
        - name: timezone-vol
          hostPath:
            path: /etc/timezone
  
  ---
  # 2. data-module 배포 정의
  ...
  
  ---
  # 3. route-module 배포 정의 (공통 볼륨/네트워크 설정 없음)
  ...
  ```

- 기본 실행
  ※ docker-compose 의 `up -d` 와 동일

  ```bash
  kubectl apply -f k8s-deploy.yaml
  ```

- 확인
  ```bash
  kubectl get pods
  ```

- 로그 확인
  ```bash
  kubectl logs <파드의_전체이름>
  ```

#### 5-1) 호스트 리소스 활용

쿠버네티스는 호스트 리소르 활용을 위한 볼륨 마운트 방식이 docker 와는 근본적으로 다르다.

| docker                                         | Kubernetes                                                   |
| ---------------------------------------------- | ------------------------------------------------------------ |
| 리눅스 서버의 실제 경로를 컨테이너에 직접 연결 | 파일의 내용(텍스트, 데이터)만 긁어와서 내부에 DB 에 저장해버림 |
| 실제 파일을 그대로 봄                          | 메모리상에 가짜 파일을 만들어서 컨테이너에 삽입함            |
| 호스트의 리소스를 그대로 사용                  | 컨테이너 내부의 DB 자원 사용                                 |

즉, 볼륨 마운트를 했다고 해서 그 파일을 "파일" 로써 사용하는 것은 불가능하며 이를 위해선 추가적인 세팅을 해줘야한다.
```bash
kubectl create configmap ipconfig-cmap --from-file=/home/gj_anly/ipconfig_cryp.ini
kubectl create configmap cryptogram-cmap --from-file=/home/gj_anly/cryptogram
```

```bash
# 삭제 할 경우
kubectl delete configmap ipconfig-cmap
# 삭제 없이 새로 적용할 경우
kubectl create configmap ipconfig-cmap --from-file=/home/gj_anly/ipconfig_cryp.ini -o yaml --dry-run=client | kubectl apply -f -
```



이후 .yaml 에서

```yaml
        volumeMounts:
        - name: ipconfig-vol
          mountPath: /ipconfig_cryp.ini
          subPath: ipconfig_cryp.ini # 폴더가 아닌 파일 자체를 마운트할 때 필요
          readOnly: true
        - name: cryptogram-vol
          mountPath: /cryptogram
...(중략)
      volumes:
      - name: ipconfig-vol
        configMap: # hostPath 대신 configMap 사용
          name: ipconfig-cmap
      - name: cryptogram-vol
        configMap: # hostPath 대신 configMap 사용
          name: cryptogram-cmap
```

단, 이 `--from-file=` 방식은 해당 폴더 내부의 바로 아래(=depth 1)의 파일만 가져오기에 만약에 내부에 폴더가 또 있다면 무의미해진다.

이로인해 만약 진짜로 host 리소스를 그대로 사용하기 위해서는 연결 그 자체를 **실시간으로 계속 켜둘 필요**가 있다.
```bash
minikube mount /home/gj_anly/cryptogram:/home/gj_anly/cryptogram
```

```yaml
        volumeMounts:
        - name: cryptogram-vol
          mountPath: /cryptogram
...(중략)
      volumes:
        - name: cryptogram-vol
        hostPath: # cryptogram 폴더는 리눅스 실제 하드디스크 연결
          path: /home/gj_anly/cryptogram
```

하지만 이때 보통은 
`permission denied while trying to connect to the docker API at unix:///var/run/docker.sock`
와 같은 권한문제가 발생하게된다. 이는 sudo 로 실행해버리면 내 정체가 `root` 로 변해버리는데 **미니큐브는 `gj_naly` 으로 켜두었으니** `root` 이름으로 켜둔 미니큐브가 없어서 실행이 필요하다는 메시지가 나옴.
즉 `sudo` 를 쓰면 안되며 `docker` 권한을 줘야함

```bash
sudo chmod 666 /var/run/docker.sock
```

이 후 재실행
```bash
minikube mount /home/gj_anly/cryptogram:/home/gj_anly/cryptogram
```

이는 tmux 등으로 계속 켜둬야하는 특성상 백그라운드에 돌릴 필요가 있다.
이에, 가능하면 `systemd` 를 통한 서비스 등록을 권장한다.

- 서비스 설정 파일 생성
  ```bash
  sudo nano /etc/systemd/system/minikube-mount.service
  ```

- 내용 입력
  ```bash
  [Unit]
  Description=Minikube Mount for Cryptogram
  # 순서: docker 가 켜지고 그다음 minikube 본체가 켜짐
  After=docker.service minikube.service
  # minikube 서비스가 켜져 있는 경우 (조건)
  Requires=minikube.service 
  
  [Service]
  Type=simple
  User=gj_anly
  # minikube의 절대 경로 (보통 /usr/local/bin/minikube)
  ExecStart=/usr/local/bin/minikube mount /home/gj_anly/cryptogram:/home/gj_anly/cryptogram
  Restart=always
  RestartSec=3
  
  [Install]
  WantedBy=multi-user.target
  ```

- 백그라운드 실행
  ```bash
  sudo systemctl daemon-reload
  sudo systemctl enable minikube-mount.service
  sudo systemctl start minikube-mount.service
  ```

  이렇게 해 두면 리눅스 서버가 켜져있는 한 마운트가 절대 끊기지 않음

#### 5-2) 도커 이미지 인식(중요)

현재 사용 중인 리눅스 서버(호스트)의 도커 공간과 **미니큐브 내부의 도커 공간은 완전히 격리** 되어있음

리눅스 서버 상에서 `docker images` 를 통해 확인가능한 이미지들은 **미니큐브는 볼 수 없음**

- 미니큐브에 이미지 인식 시키기
  ```bash
  minikube image load <image_name>
  ```

#### 5-3) cronjob 등록 방법

쿠버네티스에는 crontab 를 대체하는 `cronjob` 리소스가 자체 내장되어있음
```yaml
apiVersion: batch/v1
kind: CronJob
metadata:
  name: analy-module-cron
spec:
  # 1. 실행 주기 설정 (리눅스 crontab 문법과 100% 동일)
  schedule: "*/5 * * * *"
  
  # 2. 핵심 요구사항: "이전 작업이 안 끝났으면 건너뛴다"
  concurrencyPolicy: Forbid 
  
  jobTemplate:
    spec:
      template:
        spec:
          hostNetwork: true
          # 3. 작업이 끝나면 재시작하지 말고(Never/OnFailure) 종료 상태를 유지할 것
          restartPolicy: OnFailure 
          
          # --- 아래부터는 기존과 100% 동일 ---
          containers:
          - name: analy-module
          ...
```



### 6) 삭제 & 재실행

- 컨테이너 삭제
  ```bash
  kubectl delete -f k8s-deploy.yaml
  ```

- 재배포
  ```bash
  kubectl apply -f k8s-deploy.yaml
  ```

- 상태 확인
  ```bash
  kubectl get pods
  ```

- 로그 확인
  ```bash
  kubectl logs <파드의_전체이름>
  ```

## [시스템 서비스 등록]

배포시 cronjob 은 한번 켜두기만 하면 완벽하게 스케줄링이 되지만 쿠버네티스 본체가 아예 꺼져버리면 의미가 없다.

이에, 리눅스 시스템이 아예 껏다 켜졌을때를 대비한 `systemd` 기반 시스템 서비스 등록을 병행하는 것이 좋다.

\<systemd 간단설명>

| **명령어**      | **역할**                                                     | **언제 쓰는가?**                                             |
| --------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| `daemon-reload` | 리눅스에게 "설정 파일 내용이 바뀌었으니 다시 읽어라"라고 알려줌 | `.service` 파일 내부의 글자를 수정했을 때 **무조건 실행**    |
| `enable`        | "서버 재부팅 시 이 서비스를 자동으로 켜라"라고 예약함        | 처음 서비스를 만들었을 때 **딱 한 번**만 수행 (한 번 해두면 파일 수정 후 다시 할 필요 없음) |
| `start`         | "지금 당장 이 서비스를 켜라"                                 | 서비스가 꺼져 있을 때 켬 (이미 켜져 있다면 안 해도 됨)       |
| `restart`       | "껐다가 다시 켜라"                                           | 설정 변경 내용을 **즉시 적용**하고 싶을 때 (예: 마운트 경로를 바꾼 직후) |



### 1) 쿠버네티스 본체 실행

- 서비스 설정 파일 생성

  ```bash
  sudo nano /etc/systemd/system/minikube.service
  ```

- 내용 입력

  ```bash
  [Unit]
  Description=Minikube Cluster
  After=docker.service
  Requires=docker.service
  
  [Service]
  Type=oneshot
  RemainAfterExit=yes
  User=USER
  # 미니큐브 시작 명령어 (기존에 수동으로 치던 그 명령어)
  ExecStart=/usr/local/bin/minikube start --driver=docker
  ExecStop=/usr/local/bin/minikube stop
  
  [Install]
  WantedBy=multi-user.target
  ```

- 서비스 등록 및 활성화

  ```bash
  sudo systemctl daemon-reload
  sudo systemctl enable minikube.service
  sudo systemctl start minikube.service
  ```

### 2) 마운트 실행

※ 5-1) 호스트 리소스 활용에도 설명되어있음

- 서비스 설정 파일 생성

  ```bash
  sudo nano /etc/systemd/system/minikube-mount.service
  ```

- 내용 입력

  ```bash
  [Unit]
  Description=Minikube Mount for Cryptogram
  # 순서: docker 가 켜지고 그다음 minikube 본체가 켜짐
  After=docker.service minikube.service
  # minikube 서비스가 켜져 있는 경우 (조건)
  Requires=minikube.service 
  
  [Service]
  Type=simple
  User=gj_anly
  # minikube의 절대 경로 (보통 /usr/local/bin/minikube)
  ExecStart=/usr/local/bin/minikube mount /home/gj_anly/cryptogram:/home/gj_anly/cryptogram
  Restart=always
  RestartSec=3
  
  [Install]
  WantedBy=multi-user.target
  ```

- 백그라운드 실행

  ```bash
  sudo systemctl daemon-reload
  sudo systemctl enable minikube-mount.service
  sudo systemctl start minikube-mount.service
  ```

  이렇게 해 두면 리눅스 서버가 켜져있는 한 마운트가 절대 끊기지 않음

## [모니터링]

쿠버네티스에는 파드의 자원 사용량을 수집하는 메트릭 서버 (metrics Server) 내장기능이 있음

### 1) 켜기

- 메트릭 서버 켜기
  ※ 켜진 직후에는 데이터를 수집하기에 1~2분 정도 소요

  ```bash
  minikube addons enable metrics-server
  ```

### 2) 확인

- 자원 사용량 확인
  ```bash
  kubectl top pods
  ```

- 서버 전체 자원량 확인
  ```bash
  kubectl top nodes
  ```

## [K3s - 폐쇄망]

### 1) 파일 다운로드 & 생성

폐쇄망의 특성상 일단 기본 설치용 바이너리 파일 등을 전부 인터넷이 되는 PC 에서 다운로드 받아야 한다.

- K3s 바이너리 파일 다운로드
  ```bash
  curl -L -O https://github.com/k3s-io/k3s/releases/download/v1.28.7%2Bk3s1/k3s
  ```

- K3s 시스템 이미지 (Airgap Images) 다운로드
  ```bash
  curl -L -O https://github.com/k3s-io/k3s/releases/download/v1.28.7%2Bk3s1/k3s-airgap-images-amd64.tar
  ```

- K3s 설치 스크립트 다운로드
  ```bash
  curl -sfL https://get.k3s.io > install.sh
  ```

  - 파일 권한 부여
    ※ 혹시모를 권한 문제 방지

    ```bash
    chmod +x k3s install.sh
    ```

- 파일 용량 확인
  각 다운로드 파일이 제대로 다운로드됬는지 대략적 크기를 확인

  ```bash
  ls -lh
  # k3s --> 약 60MB
  # k3s-airgap-images-amd64.tar --> 약 150~200MB
  # install.sh : 약 30KB
  ```

- Redis 도커 이미지 아카이빙

  ```bash
  docker save -o redis.tar redis:7-alpine
  ```

- 가져갈 파일 목록 체크

| **분류**      | **파일명**                        | **용도**                  |
| ------------- | --------------------------------- | ------------------------- |
| **내 이미지** | `my_image.tar`                    | 파이썬 모듈 도커 컨테이너 |
| **K3s 엔진**  | `install.sh`                      | 설치 자동화 스크립트      |
| **K3s 엔진**  | `k3s`                             | K8s 실행 바이너리         |
| **K3s 엔진**  | `k3s-airgap-images-amd64.tar`     | K8s 시스템 이미지 묶음    |
| **설정/코드** | `k3s-mq-architecture-airgap.yaml` | 전체 설계도 (YAML)        |
| **인프라**    | `redis.tar`                       | 컨베이어 벨트(Redis)      |
| **설정/코드** | `run_k3s.sh`                      | 배포 자동화 스크립트      |
| **설정/코드** | `run_monitoring.sh`               | 실행 로그 실시간 파악용   |

### 2) 파일 준비 & 설치

- K3s 파일 배치 및 사전 준비

  ```bash
  # 1. K3s 실행 파일을 시스템 경로로 이동 및 권한 부여
  sudo cp k3s /usr/local/bin/
  sudo chmod +x /usr/local/bin/k3s
  
  # 2. 시스템 이미지를 K3s 전용 경로에 배치 (설치 시 자동으로 읽어감)
  sudo mkdir -p /var/lib/rancher/k3s/agent/images/
  sudo cp k3s-airgap-images-amd64.tar /var/lib/rancher/k3s/agent/images/
  ```

- K3s 오프라인 설치 진행
  ※ docker 연동 모드

  ```bash
  # INSTALL_K3S_SKIP_DOWNLOAD: 인터넷 다운로드 방지
  # --docker: 내장 런타임 대신 서버의 Docker 엔진 사용
  chmod +x install.sh
  sudo INSTALL_K3S_SKIP_DOWNLOAD=true INSTALL_K3S_EXEC="--docker" ./install.sh
  ```

- 설치 확인

  ```bash
  sudo kubectl get nodes
  ```


### 3) 배포

- 도커 이미지 로드 및 YAML 배포

  ```bash
  # 1. 이미지 로드 (이 작업은 K3s가 아니라 Docker 엔진에 등록하는 과정입니다)
  sudo docker load -i <이미지 tar 파일>
  sudo docker load -i redis.tar
  
  # 2. 이미지 등록 확인
  sudo docker images
  # 여기서 YAML에 적힌 'image: 이름:태그'와 정확히 일치하는지 확인하세요.
  ```

- 권한 문제 해결

  - 숨김 폴더 만들기

    ```bash
    mkdir -p ~/.kube
    ```

  - root 권한 으로 k3s.yaml 파일을 내 폴더로 복사(이름을 config 로 변경)

    ```bash
    sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
    ```

  - 복사해온 파일의 소유권을 내 계정으로 변경

    ```bash
    sudo chown $(id -u):$(id -g) ~/.kube/config
    ```

  - 환경 변수로 복사해온 파일을 읽도록 수정

    ```bash
    # 1. 현재 터미널에 마스터 키 경로 지정
    export KUBECONFIG=~/.kube/config
    
    # 2. 앞으로 서버를 껐다 켜도 자동으로 적용되도록 bashrc에 기록
    echo "export KUBECONFIG=~/.kube/config" >> ~/.bashrc
    
    # 3. 변경된 설정 즉시 적용
    source ~/.bashrc
    ```

  - 최종 확인

    ```bash
    kubectl get nodes
    ```

- 설계도(YAML) 에 Redis 추가하기
  맨 위 또는 맨 아래에 아래 코드를 추가

  ```yaml
  ---
  # 1. Redis 작업 반장 (Deployment)
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: redis-master
  spec:
    replicas: 1
    selector:
      matchLabels:
        app: redis
    template:
      metadata:
        labels:
          app: redis
      spec:
        containers:
        - name: redis
          image: redis:6.2-alpine  # 구워오신 Redis 이미지 태그와 똑같이 맞춰주세요 (예: redis:latest)
          imagePullPolicy: Never   # 💡 [핵심] 인터넷에서 받지 말고 서버에 있는 거 써라!
          ports:
          - containerPort: 6379
  
  ---
  # 2. Redis 연결 통로 (Service - 파이썬이 접속할 이름)
  apiVersion: v1
  kind: Service
  metadata:
    name: redis-svc
  spec:
    selector:
      app: redis
    ports:
    - port: 6379
      targetPort: 6379
  ```


### 4) 이미지 인식이 안되는 경우

- K3s가 어떤 창고를 쓰는지 확인
  ※ 설치할때 `--docker` 옵션을 주었지만 서버에 특정 연결 프로그램(`cri-dockerd`)이 깔려있지 않았다면 K3s 는 자신의 내장엔진(`containerd`)로 서버를 켜고 있을 가능성이 있음

  ```bash
  kubectl get nodes -o wide
  ```

- 파드의 상세 기록 보기

  ```bash
  kubectl describe pod <파드이름>
  ```

- 도커 창고에 `pause` 이미지 넣기

  reference "docker.io/rancher/mirrored-pause:3.6" failed to do request: Head "https://registry-1.docker.io ... i/o timeout

  FailedCreatePodSandBox ... 의 기록이 나와있다면 이는 

  1. 우리는 K3s를 설치할 때 `--docker` 옵션을 주어 **"K3s에게 내장 엔진 쓰지 말고 우리 서버에 깔린 Docker를 써라!"**라고 명령함
  2. K3s는 시스템 가동에 필요한 `k3s-airgap-images...` 파일을 자기 폴더에 잘 챙겨두었지만, 정작 실무를 담당하는 **Docker 엔진의 창고에는 이 기초 공사용 `pause` 이미지를 넣어주지 않았음**
  3. 파드를 띄우라는 명령을 받은 Docker는 자기 창고에 `pause` 이미지가 없으니, 당연히 인터넷(Docker Hub)에 접속해서 다운받으려다가 타임아웃(`i/o timeout`)으로 뻗어버린 것입니다.

  해결방법: K3s시스템 이미지 묶음(`k3s-airgap-images-amd64.tar`)을 Docker 엔진에 넣으면 됨

  ```bash
  sudo docker load -i k3s-airgap-images-amd64.tar
  ```

  - 잘 들어갔는지 확인

    ```bash
    sudo docker images | grep pause
    ```

- 기존 job 제거

  cronjob 특성장 자기 스케줄에 맞춰 스스로 작업 지시서(job) 을 생성함. 
  만약 예전버전으로 올려두었다면 이 **실패한 예전버전 job 을 계속 재시도** 하게 됨
  다음버전을 업데이트해도 이미 만들어져 **재시도중인 과거의 job 은 업데이트 되지 않음**.

  ```bash
  # 1. 현재 돌고 있거나 실패해서 남아있는 Job 목록 확인
  kubectl get jobs
  
  # 2. 목록에 analy-producer-어쩌구 하는 Job들이 있다면 싹 다 지워줍니다.
  kubectl delete jobs --all
  ```

  



# 모음

- 컨테이너 확인
  ```bash
  kubectl get nodes
  ```

- 배포 실행

  ```bash
  kubectl apply -f k8s-deploy.yaml
  ```

  ※ 이 명령어는 쿠버네티스의 핵심 철학인 "선언적 상태 유지(Declarative)" 를 보여주는 핵심임
  우선, 이 명령어는 기존 자원을 냅다 지우고 새로 만드는 것이 **아님**.

  1. 현대 돌아가고 있는 상태와 방금 던져진 Yaml 파일의 내용을 비교
  2. 변경된 부분(이미지 태그, 변수, Replicas 숫자 등)을 정밀하게 업데이트
  3. 변경사항이 없는 경우 "unchanged" 메시지와 함께 아무런 일도 하지 않음

  즉 굳이 서비스를 `delete` 하는 것은 오히려 **쿠버네티스의 장점을 버리는 행위**임
  ```bash
  kubectl rollout restart deployment/analy-consumer
  ```

  위의 명령어가 있다면 쿠버네티스는 **Rolling Update 방식**으로 톱니바퀴를 갈아 끼움

  1. 새로운 설정/이미지를 적용받은 새 작업자 A 를 띄움
  2. A 가 정상적으로 부팅되어 Redis 작업 큐에 접속하면 기존 "구형 작업자 1번" 에게 종료신호를 보냄
  3. 구형 작업자 1번이 완전히 꺼지면(작업자A로 교대 완료) 새 작업자 B 를 띄우고 "구형 작업자 2번"을 종료
  4. 반복...

- 배포 삭제
  ※ 재실행되지 않는 완전 삭제 방법

  ```bash
  kubectl delete -f k8s-deploy.yaml
  ```

- 특정 디플로이먼트만 삭제
  ```bash
  kubectl delete deployment <서비스 이름> # 예: analy-module
  ```

- 파드 삭제
  ※ 실행된 상태에서는 파드만 삭제해도 바로 다시 재실행됨

  ```bash
  kubectl delete pod <파드이름>
  ```

- 파드 상태 확인
  ```bash
  kubectl get pods
  kubectl get pods -w # 실시간으로 보기
  ```

- 파드 로그 확인
  ```bash
  kubectl logs <파드의_전체이름>
  ```

  - 복수 파드 동시 확인
    ```bash
    kubectl logs -f -l app=analy-consumer --max-log-requests=10
    ```

- 크론잡 확인
  ```bash
  kubectl get cronjob
  ```

- 컨테이너 내부에 들어가기

  ```bash
  kubectl exec -it <파드이름> -- /bin/bash
  ```

- 컨테이너 내부 파일 내용 보기
  ※ vim, nano 등은 사용 불가

  ```bash
  kubectl exec -it <파드이름> -- cat /path/to/file
  ```

- 네임스페이스 확인
  ```bash
  kubectl get pods --all-namespaces
  ```

- 컨테이너 내부 파일 복사해서 가져오기
  ```bash
  kubectl cp <네임스페이스>/<파드이름>:/path/to/container/file ./local_file
  ```

  ``` tar: Removing leading `/' from member names ``` 는 절대경로를 상대경로로 바꾸어 처리했다는 일종의 경고성 알림으로 문제 없음

## 로깅

- 갯수 확인
  ※ 내부 파이썬 소스코드에서 관련 변수가 지정되어야 볼 수 있음

  ```python
  # 내부 소스코드 예시
  stats_mapping = {
      'total': 1000,
      'success': 0,
      'error': 0,
      "drop":0,
      'off': 0
  }
  redis_conn.hset('current_batch_stats', mapping=stats_mapping)
  redis_conn.hincrby('current_batch_stats', 'success', 1)
  redis_conn.hincrby('current_batch_stats', 'error', 1)
  redis_conn.hincrby('current_batch_stats', 'drop', 1)
  redis_conn.hincrby('current_batch_stats', 'off', 1)
  ```

  ```bash
  kubectl exec -it deployment/redis-master -- redis-cli HGETALL current_batch_stats
  ```

  

## DB

쿠버네티스 환경은 격리된 환경 이기에 그 앱 컨테이너 안에서 `127.0.0.1` 을 호출하면 그 요청이 컨테이너 밖으로 나가지 못하고 자기자신에서만 찾아서 "없다" 는 에러가 나옴.

이를 해결하기 위한 방법으로는 간단하게는 `host = host.minikube.internal`를 하는 방법이 있음.

아예 다른 IP 인 경우 대부분은 문제없이 작동함

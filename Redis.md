# 순서

## [기본]

### 1) 다운로드

- 가장 가벼운 Redis 이미지 다운로드

  ```bash
  docker pull redis:7-alpine
  ```

- tar 파일로 압축
  ※ 물리적 단절된 운영서버 반입 시

  ```bash
  docker save -o redis-image.tar redis:7-alpine
  ```

### 2) 설치

- K8s 에 Redis 설치
  ```bash
  minikube image load redis-image.tar
  ```

- Redis 전용 K8s 설정 파일 생성
  ```bash
  # 1. Redis 컨테이너 띄우기 (Deployment)
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
          image: redis:7-alpine
          imagePullPolicy: Never  # (중요) 인터넷에서 찾지 말고 방금 로드한 tar 이미지 사용!
          ports:
          - containerPort: 6379
  
  ---
  # 2. Redis 전용 내부 네트워크 주소 만들기 (Service)
  apiVersion: v1
  kind: Service
  metadata:
    name: redis-svc  # 파이썬 코드에서는 이 이름을 IP 대신 사용합니다!
  spec:
    selector:
      app: redis
    ports:
    - port: 6379
      targetPort: 6379
  ```

- 쿠버네티스에 적용 (설치)
  ```bash
  kubectl apply -f redis-deploy.yaml
  ```

- 설치 확인
  ```bash
  kubectl get pods
  # redis-master-xxxxxx 파드가 Running 상태인지 확인
  ```

  ```bash
  kubectl get svc
  # redis-svc 라는 서비스가 6379 포트로 열려있는지 확인
  ```

- 파이썬 라이브러리 설치
  ```bash
  pip install redis
  ```

### 3) 검증

- K8s 안의 Redis 를 로컬로 끌어오기
  ※ 로컬에서 파이썬을 돌리려면 K8s 안에 있는 Redis에 접근할 수 있어야 한다. 이를 위해 **새로운 터미널 창을 열고** 포트포워딩을 진행해야함

  ```bash
  kubectl port-forward svc/redis-svc 6379:6379
  ```

  이 명령어를 켜두면 로컬 PC 에서 `127.0.0.1:6379` 로 가는 모든 통신이 K8s 내부의 Redis 파드로 들어가게됨

# 모음

- 접속 에러
  ```Error -3 connecting to redis-svc:6379. Temporary failure in name resolution```
  와 같은 에러가 나는 경우 보통은 네트워크 시야 (DNS) 문제임.
  로컬 외부에서 실행할때는 host 를 직접 만든 값이 아니라 `127.0.0.1` 을 바라보도록 해야함
  예:

  ```python
  if self.local:
      redis_host = "127.0.0.1"
  else:
      redis_host = "redis-svc"
  # IP 주소 대신 Service 이름(redis-svc)을 기입
  """
  환경에 상관없이 쿠버네티스 위에 올라가기만 하면 이름 하나로 Redis 를 검색 가능.
  설정 파일 ini 등에 하드코딩할 필요 X
  """
  r = redis.Redis(host=redis_host, port=6379, db=0, decode_responses=True)
  # 연결이 잘 되었는지 핑 테스트
  r.ping() 
  ```

- 작업 분할
  producer 와 consumer 로 나누는 것은 좋지만 consumer 가 작업이 끝나지도 않았는데 producer 가 계속해서 작업을 쌓으면 쌓인 작업만 수천 수만개가 넘어갈 수 있다. (배치 겹침 문제)
  이를 방지하기 위해 "작업 리스트에 존재하면" 넣지 않거나 아예 "작업 리스트가 비지 않으면" 작업 추가를 안 하는 방식으로 변경하는 것이 좋다.
  이를 방지하는 예제 함수 코드(바로 쓰기는 어려움)

  ```python
  # 💡 [핵심] 쿠버네티스 종료 신호(SIGTERM)나 Ctrl+C(SIGINT)가 오면 
  # 프로그램이 그냥 죽지 말고 _graceful_shutdown 함수를 먼저 실행하도록 설정!
  signal.signal(signal.SIGTERM, self._graceful_shutdown)
  signal.signal(signal.SIGINT, self._graceful_shutdown) 
  
  def _graceful_shutdown(self, signum, frame):
      print("\n🚨 [경고] 쿠버네티스(또는 사용자)로부터 종료 신호를 받았습니다! 시스템을 안전하게 정지합니다.")
      
      # 현재 손에 쥐고 있는 일거리가 있다면 Redis에 반납!
      if self.redis_conn and self.current_job_payload:
          bus_id = self.current_job_payload.get('bus_id')
          print(f"⚠️ 작업 중이던 버스 [{bus_id}] 데이터를 큐에 반납합니다...")
          
          # 1. 큐의 맨 뒤(rpush)가 아니라 맨 앞(lpush)에 넣어서 다음 작업자가 최우선으로 가져가게 함!
          self.redis_conn.lpush('job_queue', json.dumps(self.current_job_payload))
          
          # 2. 작업 중 명부에서 내 이름 지우기
          self.redis_conn.srem('working_buses', bus_id)
          
          print(f"✅ [{bus_id}] 반납 완료! 파드를 종료합니다.")
      else:
          print("✅ 현재 진행 중인 작업이 없습니다. 파드를 종료합니다.")
          
      sys.exit(0) # 반납을 마쳤으니 미련 없이 프로그램 완전 종료
  ```

  ※ 이 경우 발생할 수 있는 문제 사항

  1. 1000개 작업 중 1번 작업이 매우 중요한 작업인데 "1000개가 끝날때까지 대기" 상태면 대응이 늦어질 수 있음 (Priority Queue 필요)
  2. 1000개 작업 중 999개를 끝냈는데 마지막 1개 작업이 에러가 발생해 영원히 끝나지 않는 상황이라면? (Timeout 및 Dead Letter Queue 필요)

- 작업 없애기
  로직이 꼬이든 어떤이유든 테스트 도중엔 모든 작업을 강제로 취소하고 다시 시작해야할 때가 있음.

  ```bash
  # redis-master 파드에 접속해서 DEL 명령어 실행
  kubectl exec -it deployment/redis-master -- redis-cli DEL job_queue working_buses
  ```

  - 완전히 싹다 지우기
    ```bash
    kubectl exec -it deployment/redis-master -- redis-cli FLUSHDB
    ```

- 작업 갯수 확인
  ```bash
  kubectl exec -it deployment/redis-master -- redis-cli LLEN job_queue
  ```

  - 현재 연산 중인 갯수
    ```bash
    kubectl exec -it deployment/redis-master -- redis-cli SCARD working_buses
    ```

    

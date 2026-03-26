# 사용 방법

## 1. 기본

ollama 기반 RAG 용 사용방법

1. 모델 다운로드
   ```bash
   ollama pull qwen2.5:7b
   ```

   더 가벼운 `qwen2.5:1.5b`  `qwen2.5:3b` 사용 가능

   - 모델 리스트 확인
     ```bash
     ollama list
     ```

   - 모델 위치 (기본)
     ```bash
     /usr/share/ollama/.ollama/models/blobs, manifest
     ```

     `blobs`: 모델의 가중치와 데이터가 담긴 이진 파일들

     `manifest` : 모델의 메타데이터

2. 예시 코드 실행
   ```python
   def main():
       import ollama
   
       response = ollama.chat(model='qwen2.5:7b', messages=[
       {
           'role': 'user',
           'content': 'KeyError 가 발생하는 원인을 설명해줘.',
       },
       ])
       print(response['message']['content'])
   
   if __name__ == "__main__":
       main()
   ```

## 2. 모델 저장소 변경

0. 만약 이미 기본 다운로드 받은 경우 모델 삭제
   ```bash
   ollama rm qwen2.5:7b
   ```

1. 서비스 설정 수정
   ```bash
   sudo systemctl edit ollama.service
   ```

   아래의 내용 입력
   ```bash
   [Service]
   Environment="OLLAMA_MODELS=/home/path/to/model"
   ```

   **※주의** : 파일 내부에 보면 
   `Lines below this comment will be discarded`

   라는 부분이 있는데 **반드시** 이 문구의 위쪽에 적어야한다. 이 문구 아랫쪽은 수정이 반영되지 않아 적용되지 않는다.

2. 설정 적용 및 재시작
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl restart ollama
   ```

3. 모델 재다운로드
   ```bash
   ollama pull qwen2.5:7b
   ```

   원하는 경로에 파일이 생성되었는지 확인
   ```bash
   ls -R /home/path/to/model
   ```

### 2.1. 에러 발생시

1. 서비스 설정 파일 변경이 적용되지 않을 때
   파일 내부에 보면 
   `Lines below this comment will be discarded`
   라는 부분이 있는데 **반드시** 이 문구의 위쪽에 적어야한다. 이 문구 아랫쪽은 수정이 반영되지 않아 적용되지 않는다.

   만약 그래도 저장되지 않으면 override 파일을 직접 생성하여 강제 작성
   ```bash
   sudo mkdir -p /etc/systemd/system/ollama.service.d
   sudo nano /etc/systemd/system/ollama.service.d/override.conf
   ```

   ```bash
   [Service]
   Environment="OLLAMA_MODELS=/home/your_user/Qwen/model"
   ```

2. `Error: could not connect to ollama server, run 'ollama serve' to start it`

   위의 에러는 **Ollama 백그라운드 서비스가 실행되지 않았거나 설정한 시스템 서비스가 정상적으로 올라오지 않았을때** 보통 발생한다.

   1. 서비스 상태 확인
      ```bash
      sudo systemctl status ollama
      ```

      ```bash
      ● ollama.service - Ollama Service
           Loaded: loaded (/etc/systemd/system/ollama.service; enabled; vendor preset: enabled)
          Drop-In: /etc/systemd/system/ollama.service.d
                   └─override.conf
           Active: active (running) since Mon 2026-03-09 14:26:37 KST; 16min ago
         Main PID: 360780 (ollama)
            Tasks: 30 (limit: 463780)
           Memory: 4.5G
              CPU: 2min 16.082s
           CGroup: /system.slice/ollama.service
                   └─360780 /usr/local/bin/ollama serve
      ```

      이런식으로 Active: 에 `active` 가 뜨지 않고 `inactive`, `failed`, `exit-code` 등이 떠 있는 경우 서비스가 뜨지 않은 것
      이 경우 서비스 재시작 필요

      ```bash
      sudo systemctl daemon-reload
      sudo systemctl enable ollama
      sudo systemctl start ollama
      ```

      만약 서비스가 시작했는데도 문제가 발생하면 소켓 충돌의 가능성도 있음
      ```bash
      # 이미 실행 중인 ollama 프로세스 모두 종료
      sudo pkill ollama
      # 다시 서비스 시작
      sudo systemctl start ollama
      ```

   2. **권한 문제 확인**
      저장하고자 하는 새로운 경로 디렉토리가 특정 사용자 디렉토리 내부일 경우 ollama 시스템 자체가 접근이 불가능한 경우가 있다.

      ```bash
      # 폴더 권한을 현재 로그인한 사용자로 변경 (수동 실행 시 필요)
      sudo chown -R $USER:$USER /home/your_user/Qwen/model
      ```

      아예 소유권을 ollama 서비스 계정으로 바꾸는 방법도 있다.
      ```bash
      # 소유권을 ollama 서비스 계정으로 변경
      sudo chown -R ollama:ollama /home/path/to/model
      
      # 폴더 권한 수정 (읽기/쓰기 권한 부여)
      sudo chmod -R 775 /home/path/to/model
      ```

      또한 실행 자체의 권한이 막혀있을 경우 서비스 설정 파일에 다음을 추가한다.
      ```bash
      sudo systemctl edit ollama.service
      ```

      ```bash
      [Service]
      User=<유저 ID>  # 본인의 리눅스 계정명 입력
      Group=<유저 ID> # 본인의 리눅스 그룹명 입력
      ```

      보안상 추천되지 않지만 실행자를 root 로 하는 것도 가능하다.
      ```bash
      User=root
      Group=root
      ```

      




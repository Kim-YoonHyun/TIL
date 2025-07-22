# 1. 개요

git 에서 branch 를 활용하는 명령어 및 시나리오 정리

# 2. 명령어

## 2.1. branch 활용 기본 시나리오

1. root - commit 진행
   ```bash
   $ git init
   $ touch README.md
   $ git add .
   $ git commit -m 'README!'
   $ git log # 확인
   ```

2. branch 생성

### 2.1.1. 상세 분류(설명)


# 1. 개요

서버 포트 관리 도구

# 2. 명령어

모든 명령어는 root 계정 하에서 실행 가능.

## 2.1. 포트

### 2.1.1. 특정 포트 열기

```bash
$ firewall-cmd --zone=public --permanent --add-port=8080/tcp
```

```bash
$ firewall-cmd --reload
```

### 2.1.2. 열린 포트 확인

```bash
$ firewall-cmd --zone=public --list-all
```


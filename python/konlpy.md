# JVM 에러

버전문제든 폐쇄망 문제든, konlpy 를 쓰는 과정에서 JVM 을 찾지 못해서 문제가 발생하는 경우가 있다.

이를 해결하기 위해서는 JVM 설치가 필요하다.

## 폐쇄망

1. oracle 에 접속(https://www.oracle.com/java/technologies/javase/jdk11-archive-downloads.html)

2. Linux x64 Compressed Archive 다운로드 (jdk-11.0.27_linux-x64_bin.tar.gz)

3. 설치용 디렉토리 생성후 압축해제
   ```bash
   mkdir opt
   cd opt
   tar -zxvf jdk-11.0.27_linux-x64_bin.tar.gz
   ```

4. 환경변수 설정
   ```bash
   export JAVA_HOME=/opt/jdk/jdk-11.0.28
   export PATH=$JAVA_HOME/bin:$PATH
   ```

5. 버전 확인
   ```bash
   java -version
   ```

   

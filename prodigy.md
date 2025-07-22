# 1. 개요

- annotation tool

# 2. 명령어

- 프로디지는 설정 세팅 및 실행문 두 가지의 준비를 해야함.
- [참고](https://prodi.gy/docs/)

python -m pip install prodigy -f https://CFF8-35A6-BE53-C2C9@download.prodi.gy

## 2.1. NER

### 2.1.1. 실행문

```bash
prodigy ner.manual <dataset name> blank:en <data file> --label label1,label2,label3,... --highlight-chars
prodigy ner.manual 0001 blank:en data_0001.txt --label label1,label2,label3 --highlight-chars
```

```bash
prodigy ner.manual 0001 blank:en kca_data_8040.txt --label 상호,상품,이름,주소,계좌번호,주민등록번호,전화번호,사업자등록번호 --highlight-chars
```



[레시피 설명 참고](https://prodi.gy/docs/recipes#ner)

- `ner.manual`: ner 를 하기 위한 옵션.
- `<dataset name>`: DB 에 저장할때 적용할 데이터셋 이름
- `blank:en`: spacy model for english
- `<data file>`: 태깅할 데이터셋. 보통 txt 파일.
- `--label`: 라벨 설정.
- `label1,label2,label3,...`:라벨 이름 설정. 띄어쓰기 없이 , 로만 구분.
- `--highlight-chars`: 저장되는 메타 결과값에 tokens 정보를 제외.
  tokens 정보는 문장의 각 부분에 대한 위치값 및 인덱스 값 등임.
  라벨링 값만 확인할 경우 불필요한 정보.

### 2.1.2. prodigy.json (설정 세팅)

- 프로디지를 설치할 경우 기본적으로 .prodigy 폴더가 같이 생성됨.
- 해당 폴더 내에 prodigy.db 및 prodigy.json 이 기본적으로 존재.
- 별다른 로컬 세팅을 진행하지 않으면 기본적으로 해당 2개 파일을 사용함.
- prodigy.json 을 프로젝트 디렉토리 별로 생성할 경우 로컬세팅이 적용.

```python
{
    "port" : 8080,
    "host" : "59.25.131.135",
    "batch_size": 20,
    "history_size": 20,
    "db": "sqlite",
    "db_settings": {
        "sqlite": {
            "name": "ner_dataset.db",
            "path": "/home/theimc/prodigy_text"
        }
    }
}
```

- 기본 설정 부분
  [상세 참고](https://prodi.gy/docs/install#config)
  prodigy 를 실행할 사이트 및 결과 표시할 갯수 등의 기본적인 세팅값을 필요로함. 필수적으로 필요한 host, port 값을 제외하면 나머지는 설정하지 않을 시 기본값으로 적용됨.

  ```python
  "port" : 8080,
  "host" : "59.25.131.135",
  "batch_size": 20,
  "history_size": 20,
  ```

  

- DB 설정 부분.
  [상세 참고](https://prodi.gy/docs/api-database)
  mysql 등의 다른 db 베이스를 활용할 수 있지만 DB 드라이버를 서버에 설치해야하는 번거로움이 존재하기에 아무 설정없이 빠르게 활용가능한 sqlite 베이스 DB를 사용.

  ```python
  "db": "sqlite",
  "db_settings": {
          "sqlite": {
              "name": "<name>.db",
              "path": "<path/to/directory>"
          }
  ```


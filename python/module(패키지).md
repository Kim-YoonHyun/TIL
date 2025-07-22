1. 폴더 만들기

2. 내부에 \__init__.py 파일 생성(3.3 이후는 없어도 되지만 하위버전과의 호환성)

   ```python
   from . import <패키지>
   
   
   # 이 문구가 있어야 불러오기에서 from~import 방식 사용 가능
   ```

   

3. 불러오기

   ```python
   import <폴더이름>
   import <폴더이름>.<변수>
   import <폴더이름>.<함수>
   from <폴더이름> import <함수>
   
   ```

   

4. 13

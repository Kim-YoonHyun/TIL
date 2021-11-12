# class basic

## 기본 구조

### 예시 코드

```python
class MyClass:
    class_variable = 0	# 클래스 변수
    
    def __init__(self, name):	# initializer, self: 인스턴스 변수
        self.name = name		# name 속성, 같은 이름으로 쓰는 것이 관례
        self.age = 0			# age 속성
        self.tall = 0			# tall 속성
        self.__money = 0		# 비공개 속성
    
    def print_name(self):					# method
        print(self.name)
    
    def get_age_and_tall(self, age, tall):	# method 에 변수 지정 가능
        self.age += age
        self.tall += tall
    
    def print_name_method(self):			# method 에서 다른 method 호출 
        self.print_name()
        
    def __str__(self):		# 인스턴스 자체를 print 시 출력
        return f'{self.name}{self.age}{self.tall}{self.__money}'
    
    @staticmethod				# 정적 메소드 Decorator
    def calculate(num1, num2):	# 인스턴스변수에 접근 불가능
        print(num1 + num2)	
        
    @classmethod				# 클래스 메소드 Decorator
    def cls_method(cls):		# cls을 통해 클래스변수에 접근 가능
        print(cls.class_variable)
```

### 사용 예시 코드

```python
kim_yh = MyClass('Kim yoon hyun')		
kim_yh.print_name()						
kim_yh.get_age_and_tall(28, 170)
kim_yh.print_name_method()
print(kim_yh)
kim_yh.calculate(1, 2)
kim_yh.cls_method()
```

```python
kim_yh.sex = 'male'		# 신규 속성 추가 가능
```

### 외부 명령어

```python
MyClass.__dict__		# 클래스 정보를 dict 형태로 불러오기
```

```python
isinstance(kim_yh, MyClass)
```

## 상속

### 예시 코드

```python
class Man(MyClass, MyClass2:			# class 상속, 추가 상속 가능
    def __init__(self, name):	
        super().__init__(name)		# 부모 class의 인스턴스변수 살리기
        self.sex = 'male'
        
    def personality(self):
        print('kind')
```

```python
kim_yh2 = Man('kim')
kim_yh2.personality()
print(kim_yh2.sex)
kim_yh2.get_age_and_tall(28, 170)		# 부모 class의 메소드 사용 가능
print(kim_yh2.tall)						# 부모 class의 인스턴스 변수 사용 가능
```

## 추상 클래스

- **인스턴스로 만들 수 없으며** 상속에만 사용됨

```python
import abc import *

class MyClass_abs(metaclass=ABCMeta):		# 추상 클래스
    @abstractmethod    # 상속시 반드시 만들어야하는 메소드 decorator
  	def study(self):	# 인스턴스를 만들수 없기에 메소드 내용은 의미없음
    	pass
```

```python
class Man(MyClass_abs):
    def __init__(self):
        self.sex = 'male'
    
    def study(self):		# 추상클래스의 메소드가 없으면 에러 발생
        print('study math')
```


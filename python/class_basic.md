# 1. 개념

python 에서 객체를 만들어 내기 위한 기본 틀.

# 2. 상세

### self

클래스 내에 정의된 `self` 는 클래스 인스턴스이다.

### method

클래스 내부에 정의된 함수.

클래스의 성질이나 행동을 정의한다는 개념.

### 인스턴스(객체)

```python
class Male:
    def __init__(self, age):
        self.age = age
        self.__raw = age * 10  # __ 는 내부적으로 사용하는 변수에 관례적으로 붙임. 외부에서 접근은 가능함.
    
    def say_hello(self):
        print(id(self)) # 5
        print('hello')
    
    def say_good_bye():
        print('good bye') # 3, 4
        
kim = Male(27)	# 1
kim.say_hello()	# 2
kim.say_good_bye()	# 3 error
Male.say_good_bye()	# 4

lee = Male(25)
lee.say_hello()	# 6

Male.say_hello(lee) # 7
```

1. kim 이라는 객체(인스턴스) 에 Male 이라는 클래스를 부여.

2. Male 이라는 클래스는 say_hello 라는 method를 가지고 있기에
   Male 이라는 클래스에 속한 객체(인스턴스)인 kim 은 say_hello 라는 method 사용 가능.

   ```python
   >>> hello
   ```

   

3. 해당 메서드에는 아무런 인자가 주어져 있지 않지만, 
   파이썬 method의 첫 번째 인자로 항상 인스턴스(객체)가 전달되기때문에
   '받을 인자가 없는데 하나의 인자(kim 인스턴스)를 받았다'는 오류가 발생.
   즉, self 를 넣어야 항상 자동 전달되는 첫번째 인자인 인스턴스를 받을 수 있음.

   ```python
   TypeError: say_good_bye() takes 0 positional arguments but 1 was given
   ```

4. 클래스명.method 의 경우 클래스에 인스턴스(객체)가 할당되어있지않음.
   즉, 첫번째 인자로 보낼 인스턴스가 존재하지 않기에 출력 가능

   ```python
   >>> good bye
   ```

5. 인스턴스(객체 kim)에 할당된 메모리 주소를 보여줌.
   즉, self 는 클래스에 할당된 인스턴스인 kim 과 동일하다고 볼 수 있다.
6. lee 라는 새로운 객체에 Male 클래스를 부여하였기에
   kim 과는 다른 메모리 주소값이 프린트됨.
7. say_hello method는 인스턴스 인자(self)를 하나의 인자로 받기때문에
   lee 라는 인스턴스 인자를 주면 self 에는 lee 가 부여된다고 볼 수 있다.



## 기본 구조

### 예시 코드

```python
class MyClass:
    class_variable = 0	# 클래스 변수
    
    def __init__(self, name):	# initializer, self: 인스턴스 변수들
        self.name = name		# name 속성, 같은 이름으로 쓰는 것이 관례
        self.age = 0			# age 속성
        self.tall = 0			# tall 속성
        self.__money = 0		# 비공개 속성 --> 이 값은 상속되지 않음
    
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
class Man(MyClass, MyClass2):			# class 상속, 추가 상속 가능
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

## 2.2. 특수 설정

### 2.2.2. \__init__

```python
def __init__(self, a):
    self.a = a
```



### 2.2.1. \__len__

```python
def __len__(self):
    return
```

